Disk-blktrace-btt

2024年4月24日

11:42

- \
  RHEL7 和 RHEL8 测试可用\
  准备工作：

  1）在 /mnt 上建个目录，把 nvme0n1 的盘格式化成 xfs 挂载到 /mnt/nvme0n1 上。\
  2）生成一个随机读写 60G的文件。等下在收集数据时拷贝到[  /mnt/nvme0n1 ]上。

  dd if=/dev/urandom of=Test_data bs=1M count=60960\
  3）一边拷贝一边查看\" iostat -zxmd 1 nvme0n1 \"的数据，查看延时状态。

  \# /usr/bin/cp -arf test_data /mnt/nvme0n1

  4）如果系统没安装blktrace，那就 yum install blktrace 安装一下。\
  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--\
  1. 挂载 debugfs 文件系统 (系统默认有挂载)

  \# mount -t debugfs debugfs /sys/kernel/debug

   

  2. 采集数据\
  \# mkdir data [ (]先建一个文件夹因为采集数据时会有很多文件)

  \# blktrace -d /dev/nvme0n1

  Ctrl + c 收集结束

  或者

  \# blktrace -d /dev/nvme0n1 -o nvme-trace -w 60

  输出并采集60秒数据

   

  3\. 把采集的数据合并

  \#  blkparse -i nvme0n1 -d nvme0n1.blktrace.bin

  需要一点时间，合并完成后有个 nvme0n1.blktrace.bin 的文件

  或者

  \#  blkparse -i nvme0n1 -d nvme0n1.blktrace.bin -o nvme0n1-trace.txt

   

  4. 用btt去分析

  \# btt -i nvme0n1.blktrace.bin \|less

  或导出看，因为这个 \*.bin 无法直接打开看。

  \# btt -i nvme0n1.blktrace.bin -o btt.nvme0n1.txt

   

  5\. 概念方面

  从采集后合并的数据来看，如下每个对应的参数

  ![[CPU-Disk-MEM-Performances_008_Disk-blktrace-btt_001.png]]

   

  - 第一个字段：8,0 这个字段是设备号 major device ID 和 minor device ID。
  - 第二个字段：3 表示 CPU
  - 第三个字段：11 序列号
  - 第四个字段：0.009507758 Time Stamp 是时间偏移
  - 第五个字段：PID 本次 I/O 对应的进程 ID
  - 第六个字段：Event，这个字段非常重要，反映了 I/O 进行到了哪一步
  - 第七个字段：R 表示 Read， W 是 Write，D 表示 block，B 表示 Barrier Operation
  - 第八个字段：223490+56，表示的是起始 block number 和 number of blocks，即我们常说的Offset 和 Size
  - 第九个字段：进程名

   

  其中第六个字段表示I/O事件，它代表了 I/O 请求进行到了哪一阶段，有如下这些事件：

  - A ： remap 对于栈式设备，进来的 I/O 将被重新映射到 I/O 栈中的具体设备。
  - X ： split 对于做了 Raid 或进行了 device mapper(dm) 的设备，进来的 I/O 可能需要切割，然后发送给不同的设备。
  - Q ： queued I/O 进入 block layer，将要被 request 代码处理（即将生成 I/O 请求）。
  - G ： get request I/O 请求（request）生成，为 I/O 分配一个 request 结构体。
  - M ： back merge 之前已经存在的 I/O request 的终止 block 号，和该 I/O 的起始 block 号一致，就会合并，也就是向后合并。
  - F ： front merge 之前已经存在的 I/O request 的起始 block 号，和该 I/O 的终止 block 号一致，就会合并，也就是向前合并。
  - I ： inserted I/O 请求被插入到 I/O scheduler 队列。
  - S ： sleep 没有可用的 request 结构体，也就是 I/O 满了，只能等待有 request 结构体完成释放。
  - P ： plug 当一个 I/O 入队一个空队列时，Linux 会锁住这个队列，不处理该 I/O，这样做是为了等待一会，看有没有新的 I/O 进来，可以合并。
  - U ： unplug 当队列中已经有 I/O request 时，会放开这个队列，准备向磁盘驱动发送该 I/O。这个动作的触发条件是：超时（plug 的时候，会设置超时时间）；或者是有一些 I/O 在队列中（多于 1 个 I/O）。
  - D ： issued I/O 将会被传送给磁盘驱动程序处理。
  - C ： complete I/O 处理被磁盘处理完成。

  这些Event中常见的出现顺序如下：

  Q -- 即将生成 I/O 请求 

  \|

  G -- I/O 请求生成 

  \|

  I -- I/O 请求进入 I/O Scheduler 队列 

  \|

  D -- I/O 请求进入 Driver 

  \|

  C -- I/O 请求执行完毕

   

   

  Q2C 就是一个完整的IO过程，也是整个IO消耗的时间。

   

  由于每个Event都有出现的时间戳，根据这个时间戳就可以计算出 I/O 请求在每个阶段所消耗的时间，比如从Q事件到C事件的时间叫Q2C，那么常见阶段称呼如下：

   

  - Q2G -- 生成 I/O 请求所消耗的时间，包括 remap 和 split 的时间；
  - G2I -- I/O 请求进入 I/O Scheduler 所消耗的时间，包括 merge 的时间；
  - I2D -- I/O 请求在 I/O Scheduler 中等待的时间，可以作为 I/O Scheduler 性能的指标；
  - D2C -- I/O 请求在 Driver 和硬件上所消耗的时间，可以作为硬件性能的指标；
  - Q2C -- 整个 I/O 请求所消耗的时间(Q2I + G2I + I2D + D2C = Q2C)，相当于 iostat 的 await。
  - Q2Q -- 2个 I/O 请求的间隔时间。

   

  例子：

  ![[CPU-Disk-MEM-Performances_008_Disk-blktrace-btt_002.png]]

   

  注意这里面的D2C与Q2C，D2C代表硬盘实际处理时间，不包含在I/O队列中的等待时间，而Q2C代表整个I/O在块层的处理时间。

  可以看到上面Q2C(IO处理时间)平均耗时1.402ms，最大8.234ms，D2C(硬盘处理时间)平均耗时0.718ms，最大4.076ms，而旋转磁盘耗时一般在几毫秒到十几毫秒，所以这个磁盘表现正常，但如果D2C经常到100ms以上，则可能磁盘损坏了，需要尽快更换磁盘。

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

  D2C: 表征块设备性能的关键指标

   

  Q2C: 客户发起请求到收到响应的时间

  D2C 平均时间：0.000718769 秒，即 0.718 毫秒

  Q2C 平均时间：0.001402772 秒，即 1.40 毫秒

  平均下来，D2C 阶段消耗时间占比 51.2391% （下图查看）

   

  \# 这里看到整个IO下来，每个阶段所消耗的时间，占用百分比

  ![[CPU-Disk-MEM-Performances_008_Disk-blktrace-btt_003.png]]

   

  在 I2D 的时间是 36.6269% ，也就是说 I/O Scheduler 所消耗的时间不少。

  在 D2C 的时间是 51.2391%。在Driver 和硬件上所消耗的时间。

   

   

   

  FYI:

  <https://www.cnblogs.com/liulianzhen99/articles/17973221>

  <https://developer.aliyun.com/article/698568>

  <https://blog.csdn.net/Z_Stand/article/details/116304288>

   

   

   

   

 

已使用 OneNote 创建。
