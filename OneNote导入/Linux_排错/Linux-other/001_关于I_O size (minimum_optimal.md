关于I/O size (minimum/optimal

2025年5月7日

15:54

I/O size (minimum/optimal）的值是怎么来的，为什么有些机器会不一样。

官方KB

<https://access.redhat.com/solutions/2150101>

 

然后通过自己的测试得到的结论：

 

\# fdisk -l /dev/sdb

Disk /dev/sdb: 745.2 GiB, 800166076416 bytes, 1562824368 sectors

Units: sectors of 1 \* 512 = 512 bytes

Sector size (logical/physical): 512 bytes / 4096 bytes

I/O size (minimum/optimal): 4096 bytes / 4096 bytes[    ]\<- 4096是如何计算出来的？

Disklabel type: gpt

Disk identifier: 3133D739-036C-40C8-9DB5-8F9EB7CD44DF

 

 

如下，有两台不同的机器，使用 sg_vpd \--page=bl /dev/sdb 命令。

看到 其中一台 Optimal transfer length granularity: 8 blocks ，而因为一台是 Optimal transfer length granularity: 0 blocks 

 

![[Linux-other_001_关于I_O size (minimum_optimal_001.png]]

 

所以：

1）"Optimal Transfer Length Granularity" 指的是在数据传输中，最适合的数据块大小或单位以达到最佳效率。"8 blocks" 表示该系统中定义的最优粒度为8个数据块。

2）结合 Sector size (logical/physical): 512 bytes / 4096 bytes，可以得出每次最优I/O的实际大小为 8 \* 512 = 4096 bytes（即4KB）。

3）这意味着设备在处理数据时，最有效率的块大小是4KB，这也被fdisk报告中的I/O size (minimum/optimal) 为4096 bytes所证实。

 

综上所述，使用 sg_vpd \--page=bl /dev/sdb 命令可以深入了解到存储设备在块层面的详细配置，对于优化存储系统和提升性能具有重要意义。

 

另外不同RAID（条带）也会有影响。\
 

命令结构：

sg_vpd：这是一个用于查询SCSI设备Vital Product Data（VPD）的工具。VPD提供了关于设备的详细技术信息，如制造商、型号、固件版本等。

 

\--page=bl：指定要查询块层（Block Layer）相关的VPD页。这里的"bl"代表块层，会返回与存储块配置相关的属性，如扇区大小、最优读写块尺寸等。

 

/dev/sdb：目标设备文件，表示要查询的是系统中识别到的第二个硬盘驱动器（SCSI/SATA/USB等）。

 

 

 

\#[  grep -v \"zz\" /sys/block/sdb/queue/\*io_size]

/sys/block/sdb/queue/minimum_io_size:4096

/sys/block/sdb/queue/optimal_io_size:0

 

\#[  grep -v \"zz\" /sys/block/sdb/queue/\*io_size]

/sys/block/sdb/queue/minimum_io_size:512

/sys/block/sdb/queue/optimal_io_size:0

 

 

已使用 OneNote 创建。
