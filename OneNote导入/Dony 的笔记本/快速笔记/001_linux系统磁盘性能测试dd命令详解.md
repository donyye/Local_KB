linux系统磁盘性能测试dd命令详解

2025年10月10日

14:15

dd命令的常用参数可分为以下几类，结合功能场景说明如下：

 

1.1 基础输入/输出参数if=文件名：指定输入文件路径；of=文件名：指定输出文件路径；ibs=bytes/obs=bytes：分别设置输入/输出的单次块大小（单位：字节），bs可同时覆盖两者（常用）；

 

1.2 块控制参数bs=size：统一设置读写块大小（如1M、4k），直接影响I/O效率；count=N：限制复制的块数，与bs配合确定总数据量（如bs=1M count=100生成100MB文件）；skip=N/seek=N：分别跳过输入/输出文件的前N个块（需结合ibs/obs计算），不常用。

 

1.3 I/O模式标志iflag=FLAGS/oflag=FLAGS：控制读写行为，常用标志包括：direct：绕过系统缓存直接操作磁盘，提升性能测试真实性；sync：每次操作同步写入物理磁盘，确保数据持久性；dsync：类似sync但仅同步数据（非元数据）； nonblock：非阻塞I/O模式。

 

1.4 数据转换与进度显示conv=关键字：指定数据转换方式，如：ascii/ebcdic：字符编码转换；ucase/lcase：大小写转换；noerror：出错时不中断。status=progress：实时显示传输进度和速率。

 

 

2. 典型应用场景参数组合

 

2‌.1 磁盘性能测试‌：dd if=/dev/zero of=/your/test/filepath/testfile bs=1G count=10oflag=direct,sync说明：if=/dev/zero：指定输入源为Linux系统的零值设备文件，该设备会无限生成空字符（ASCII NUL）作为数据源 ;bs=1G：设置每次读写操作的块大小为1GB（可接受单位包括K/M/G，默认字节）。较大的块尺寸可提升I/O效率，但需平衡内存占用；count=10：限制复制10个块，总数据量为bs×count=10GB。若未指定则持续写入直到手动终止 。oflag=direct,sync：排除缓存干扰。

 

2‌.2 磁盘克隆‌：dd if=/dev/sda of=/dev/sdb bs=4M conv=noerror,sync说明：conv=noerror避免因坏块中断，sync确保数据完整。2‌.3 创建空文件‌：dd if=/dev/zero of=/tmp/empty_file bs=1M count=100利用/dev/zero在/tmp目录下生成指定大小为100M的empty_file文件。

 

已使用 OneNote 创建。
