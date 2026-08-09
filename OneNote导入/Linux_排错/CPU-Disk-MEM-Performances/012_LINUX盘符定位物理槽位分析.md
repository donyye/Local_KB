LINUX盘符定位物理槽位分析

2026年4月13日

11:17

-  

  000181314

   

  常见的硬盘工作在直通模式和RAID（非直通）模式，通常情况下无法直接通过系统直观的看到盘符对应到物理槽位信息，我们需要借助smartmontools和perccli来协助获取更多信息用于准确的定位物理槽位信息。

   

  一、硬盘工作在直通模式

  直通硬盘分析示例：Linux操作系统中出现/dev/sdh硬盘Medium Error，如下图

  ![[CPU-Disk-MEM-Performances_012_LINUX盘符定位物理槽位分析_001.png]]

  使用smartctl -i查看/dev/sdh的S.M.A.R.T信息中的序列号信息

  [\[root@hdfs-10-68-3-9 \~\]# ]smartctl -i /dev/sdh

  [smartctl 6.2 2013-07-26 r3841 \[x86_64-linux-3.10.107-1-tlinux2-0046\] (local build)]

  Copyright (C) 2002-13, Bruce Allen, Christian Franke, [www.smartmontools.org](http://www.smartmontools.org/)

  === START OF INFORMATION SECTION ===

  Device Model:     HGST HUH721212ALE600

  Serial Number:    8CGHTUHH

  LU WWN Device Id: 5 000cca 26fc72ebf

  Add. Product Id:  DELL(tm)

  Firmware Version: LEDENT05

  [User Capacity:    12,000,138,625,024 bytes \[12.0 TB\]]

  Sector Sizes:     512 bytes logical, 4096 bytes physical

  Rotation Rate:    7200 rpm

  [Device is:        Not in smartctl database \[for details use: -P showall\]]

  ATA Version is:   ACS-2, ATA8-ACS T13/1699-D revision 4

  SATA Version is:  SATA \>3.1, 6.0 Gb/s (current: 6.0 Gb/s)

  Local Time is:    Wed Aug 26 11:33:16 2020 CST

  SMART support is: Available - device has SMART capability.

  SMART support is: Enabled

   

  注：如提示smartctl命令不存在，需要在系统中安装smartmontools

  Red Hat / Centos: sudo yum -y install smartmontools

  Ubuntu / Debian: sudo apt-get install smartmontools

  （需提前配置好相应的软件源）

   

  结合查看TSR日志，所有直通硬盘的列表中，找到序列号相同的硬盘，并确认物理槽位信息。

   

  ![[CPU-Disk-MEM-Performances_012_LINUX盘符定位物理槽位分析_002.png]]

  如没有TSR日志，有阵列卡的配置也可以用PERCCLI来获取每个槽位的序列号信息，方法请继续参考下文。

   

  二、PERC10 硬盘工作在RAID模式（非直通模式）

  非直通硬盘分析示例：

  用户反映CentOS中/dev/sdq磁盘所对应的/dev/sdq1分区无法访问数据，系统提示IO错误，如下图

   

  检查硬件配置：R730XD + H730P Mini

  阵列配置信息：（12+4）个3.5寸机械硬盘，每一个单盘配置RAID0，2个2.5寸SSD配置RAID1安装操作系统。

   

  1.  查找和定位Target ID

  ls -l /sys/class/block \> /tmp/lsblock.txt

  将输出保存到/tmp/lsblock.txt文件中，打开文件检查/dev/sdq盘符所对应的TargetID为16

  ![[CPU-Disk-MEM-Performances_012_LINUX盘符定位物理槽位分析_003.png]]

   

  1.  查找和定位Device ID

  perccli64 /call show alilog \> /tmp/target.txt

  将输出保存到/tmp/target.txt文件中，打开文件在Logical Drive中找到Target ID 为16对应的Device ID为17。

  ![[CPU-Disk-MEM-Performances_012_LINUX盘符定位物理槽位分析_004.png]]

   

  （注：如果是多物理硬盘组成的VD，Device IDs将列出所有RAID成员物理硬盘的Device ID）

   

  1.  查找和定位硬盘序列号和Slot信息

  继续在这个文件中的Device Information中找到 Device ID 为17的硬盘物理信息

  ![[CPU-Disk-MEM-Performances_012_LINUX盘符定位物理槽位分析_005.png]]

   

  其中，Vendor Specific 为TSR日志中显示的Serial字段（具有唯一性），Slot Number即为物理槽位信息。

  因此我们可以确定，用户当前这个配置中，操作系统的/dev/sdq盘符所对应的物理槽位为Slot 17。

   

  常见的硬盘工作在直通模式和RAID（非直通）模式，通常情况下无法直接通过系统直观的看到盘符对应到物理槽位信息，我们需要借助smartmontools和perccli来协助获取更多信息用于准确的定位物理槽位信息。

   

  一、硬盘工作在直通模式

  直通硬盘分析示例：Linux操作系统中出现/dev/sdh硬盘Medium Error，如下图

  使用smartctl -i查看/dev/sdh的S.M.A.R.T信息中的序列号信息

  [\[root@hdfs-10-68-3-9 \~\]# ]smartctl -i /dev/sdh

  [smartctl 6.2 2013-07-26 r3841 \[x86_64-linux-3.10.107-1-tlinux2-0046\] (local build)]

  Copyright (C) 2002-13, Bruce Allen, Christian Franke, [www.smartmontools.org](http://www.smartmontools.org/)

  === START OF INFORMATION SECTION ===

  Device Model:     HGST HUH721212ALE600

  Serial Number:    8CGHTUHH

  LU WWN Device Id: 5 000cca 26fc72ebf

  Add. Product Id:  DELL(tm)

  Firmware Version: LEDENT05

  [User Capacity:    12,000,138,625,024 bytes \[12.0 TB\]]

  Sector Sizes:     512 bytes logical, 4096 bytes physical

  Rotation Rate:    7200 rpm

  [Device is:        Not in smartctl database \[for details use: -P showall\]]

  ATA Version is:   ACS-2, ATA8-ACS T13/1699-D revision 4

  SATA Version is:  SATA \>3.1, 6.0 Gb/s (current: 6.0 Gb/s)

  Local Time is:    Wed Aug 26 11:33:16 2020 CST

  SMART support is: Available - device has SMART capability.

  SMART support is: Enabled

   

  注：如提示smartctl命令不存在，需要在系统中安装smartmontools

  Red Hat / Centos: sudo yum -y install smartmontools

  Ubuntu / Debian: sudo apt-get install smartmontools

  （需提前配置好相应的软件源）

   

  结合查看TSR日志，所有直通硬盘的列表中，找到序列号相同的硬盘，并确认物理槽位信息。

   

   

  如没有TSR日志，有阵列卡的配置也可以用PERCCLI来获取每个槽位的序列号信息，方法请继续参考下文。

   

  二、PERC10 硬盘工作在RAID模式（非直通模式）

  非直通硬盘分析示例：

  用户反映CentOS中/dev/sdq磁盘所对应的/dev/sdq1分区无法访问数据，系统提示IO错误，如下图

   

  检查硬件配置：R730XD + H730P Mini

  阵列配置信息：（12+4）个3.5寸机械硬盘，每一个单盘配置RAID0，2个2.5寸SSD配置RAID1安装操作系统。

   

  1.  查找和定位Target ID

  ls -l /sys/class/block \> /tmp/lsblock.txt

  将输出保存到/tmp/lsblock.txt文件中，打开文件检查/dev/sdq盘符所对应的TargetID为16

   

   

  1.  查找和定位Device ID

  perccli64 /call show alilog \> /tmp/target.txt

  将输出保存到/tmp/target.txt文件中，打开文件在Logical Drive中找到Target ID 为16对应的Device ID为17。

   

  （注：如果是多物理硬盘组成的VD，Device IDs将列出所有RAID成员物理硬盘的Device ID）

   

  1.  查找和定位硬盘序列号和Slot信息

  继续在这个文件中的Device Information中找到 Device ID 为17的硬盘物理信息

   

   

  其中，Vendor Specific 为TSR日志中显示的Serial字段（具有唯一性），Slot Number即为物理槽位信息。

  因此我们可以确定，用户当前这个配置中，操作系统的/dev/sdq盘符所对应的物理槽位为Slot 17。

   

  结合TSR日志做二次验证：

   

  ![[CPU-Disk-MEM-Performances_012_LINUX盘符定位物理槽位分析_006.png]]

   

  如果无TSR日志，我们也可以使用PERCCLI来获取所有槽位的硬盘序列号信息和槽位信息。

  perccli64 /call/eall/sall show all \> /tmp/slot.txt

  将输出保存在/tmp/slot.txt文件，打开并查找序列号

   

  ![[CPU-Disk-MEM-Performances_012_LINUX盘符定位物理槽位分析_007.png]]

   

  其中，c0表示Controller 0, e32表示Enclosure 32, s17表示Slot17

  \*注：以实际的配置为准，特别是双控制器，或者多个Enclosure的配置，要特别留意。

   

  PD list:

  ![[CPU-Disk-MEM-Performances_012_LINUX盘符定位物理槽位分析_008.png]]

   

  VD list:

  ![[CPU-Disk-MEM-Performances_012_LINUX盘符定位物理槽位分析_009.png]]

   

   

  \*方法仅供学习和参考，文中截图涉及到ServiceTag、PPID、序列号等机密信息，请勿随便转发到外部。

  \*PERCCLI安装请参考《阵列卡管理工具PERCCLI命令行工具安装大全》

  \*PERCCLI程序如果没有复制到/usr/sbin，需要到PERCCLI的安装目录或所在目录执行。

   

  三、PERC11 在默认启用Firmware Device Order，VD存在倒叙问题。

  需要将lsblk中看到的Traget ID 加上128 即可对应TSR日志中的VD序号。(仍需要多个案例进行再次验证)

  例如Target ID为111的 /dev/sde

  ![[CPU-Disk-MEM-Performances_012_LINUX盘符定位物理槽位分析_010.png]]

  111+128=239

  ![[CPU-Disk-MEM-Performances_012_LINUX盘符定位物理槽位分析_011.png]]

   

  需要获取的信息无非就只有这几个：

  系统中报错截图；

  命令行执行之后，/tmp 目录下的三个文件；

  ls -l /sys/class/block \> /tmp/lsblock.txt

  perccli64 /call show alilog \> /tmp/target.txt

  perccli64 /call/eall/sall show all \> /tmp/slot.txt

  TSR日志；

   

   

   

   

   

 

已使用 OneNote 创建。
