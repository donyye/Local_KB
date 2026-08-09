Disk-FIO-测试

2024年4月17日

14:51

 

SDD 4K 随机读

\# fio -filename=/dev/sde -iodepth=1 -ioengine=libaio -direct=1 -rw=randread -bs=4k -size=4g -numjobs=32 -group_reporting -name=test-rand-read-32

 

![[CPU-Disk-MEM-Performances_007_Disk-FIO-测试_001.jpg]]

 

![[CPU-Disk-MEM-Performances_007_Disk-FIO-测试_002.jpg]]

最后 r=1288MiB/s

 

SDD  4K 随机写入 \# 需要注意，随机写会破坏磁盘数据

\# fio -filename=/dev/sde -iodepth=1 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=4g -numjobs=32 -group_reporting -name=test-rand-write-32

![[CPU-Disk-MEM-Performances_007_Disk-FIO-测试_003.jpg]]

最后 w=1304MiB/s

 

通过此方法，把测试的磁盘写入表汇总

  ------------- ----------------------- ----------------------- ----------------------- ---------------------- ---------------------- ---------------------- ---------------------- ---------------------- ---------------------- ----------------------
                SJC3N5997I1504H12/S11   SJC3N5997I1504H18/S10   SJC3N5997I1504H1C/S12   SJC3N5997I1504H13/S0   SJC3N5997I1504H17/S8   SJC3N5997I1504H11/S9   SJC3N5997I1504H16/S2   SJC3N5997I1504H10/S1   SJC3N5997I1504H0Z/S3   SJC3N5997I1504H0Y/S4
  4K-32-Read    1228                    1311                    1121                    1340                   1737                   1129                   1798                   1761                   1834                   1376
  4K-32-Write   1304                    1459                    1121                    1701                   1634                   1051                   1710                   1716                   1742                   1697
  ------------- ----------------------- ----------------------- ----------------------- ---------------------- ---------------------- ---------------------- ---------------------- ---------------------- ---------------------- ----------------------

 

 

 

 

\# fio -filename=/dev/sde -iodepth=1 -ioengine=libaio -direct=1 -rw=randread -bs=4k -size=4g -numjobs=32 -group_reporting -name=test-rand-read-32

参数解释：

-filename=/dev/sde: 指定要测试的文件或设备路径。

-iodepth=1: 每个作业的I/O深度，即同时执行的I/O请求数量。

-ioengine=libaio: 指定使用的I/O引擎，这里是异步I/O。

-direct=1: 启用直接I/O，数据不会被缓存到操作系统的页面缓存中。

-rw=randread: 读取随机位置的数据。

-bs=4k: 每个I/O请求的块大小为4KB。

-size=4g: 测试文件的大小为4GB。

-numjobs=32: 同时执行的作业数量，即并行作业数。

-group_reporting: 将每个作业的结果合并成一个报告。

-name=test-rand-read-4g: 为测试指定一个名称，用于标识测试任务。

 

 

LKB:   000199315

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

R750 H755 PERC+4块4T硬盘,slot02-slot5 配置raid5, slot0-slot1 两块SSD配置raid1 安装定制Ubuntu系统。

客户反馈OS下对SDB （raid5的VD）进行1MB dd读取测试，发现读取速率只有几MB/S左右,正常服务器ST: 88MQ214 是5块4T配置raid5，相同的测试命令能达到几百兆

 

![[CPU-Disk-MEM-Performances_007_Disk-FIO-测试_004.jpg]]

 

 

Next action:

1:当前TSR日志没有看到链路或磁盘存在明显问题，VD默认策略为回写预读 ,收集的磁盘smartlog显示4块4T磁盘均正常无坏块。

所以接下来是想让客户启用livecd 系统后，使用相同的dd测试命令来测试下sdb读取速率来排除OS的问题

测试读取命令：

dd if=/dev/sdb of=/dev/null bs=1M count=10000 iflag=direct

 

fio测试命令，需要客户提前备份数据，重新挂载客户的sdb分区，创建test文件

顺序写入：

fio -filename=test -iodepth=1 -ioengine=libaio -direct=1 -rw=write -bs=1m -size=64g -numjobs=1 -group_reporting -name=test-seq-write

顺序读取：

fio -filename=test -iodepth=1 -ioengine=libaio -direct=1 -rw=read -bs=1m -size=64g -numjobs=1 -group_reporting -name=test-seq-read

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

如果fio和dd测试结果都很低(客户需要提供正常服务器的1M dd测试结果对比)，建议安排PERC+SAS/I2C cable+BP+4T硬盘\*1 onsite；提醒客户务必备份重要数据

to dsp:

1:请dsp提前下载好livecd ISO镜像并刻录启动U盘，带笔记本电脑上门方便远程测试

链接: [https://pan.baidu.com/s/1s-wIaHxRRx9T3C5atnCk4g?pwd=ba13](https://pan.baidu.com/s/1s-wIaHxRRx9T3C5atnCk4g?pwd=ba13) 

提取码:ba13

2：上门后先不更换硬件，将带过去的4T硬盘单独配置一个raid0 ,然后启动livecd系统后，对单盘raid0进行dd和fio的性能测试并记录结果

测试命令：

（sdb为需要测试的raid0盘符，请务必根据实际情况确认清楚）

 

测试读取：

dd if=/dev/sdb of=/dev/null bs=1M count=10000 iflag=direct

测试写入：

dd if=/dev/zero of=/dev/sdb bs=1M count=10000 oflag=direct

顺序写入：

fio -filename=/dev/sdb -iodepth=1 -ioengine=libaio -direct=1 -rw=write -bs=1m -size=64g -numjobs=1 -group_reporting -name=test-seq-write

顺序读取：

fio -filename=/dev/sdb -iodepth=1 -ioengine=libaio -direct=1 -rw=read -bs=1m -size=64g -numjobs=1 -group_reporting -name=test-seq-read

 

 

 

已使用 OneNote 创建。
