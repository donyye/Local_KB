EDAC与Linux

2022年6月20日

15:31

 

Red Hat Enterprise Linux 中提供错误检测和纠正 (EDAC) 支持

<https://access.redhat.com/solutions/3896>

 

EDAC 不是所有的 cpu 都支持

 

![[Technology_ALL_Linux 问题收集_079_EDAC与Linux_001.png]]

 

 

如果不不支持有可能导致 EDAC在系统上无法启动。

 

 

Kernel Reaction to Types of Error Reported

- The kernel response to an ECC or parity error is determined by the chip support in the kernel and the motherboard support. There are three possible configurations:
  - motherboard with EDAC supported chipset
  - motherboard with no EDAC support, but has on-board NMI support
  - motherboard with no EDAC support, and no on-board NMI support

 

 

==========客户环境展示的 CentOS7.9=========

[\[root@lg-compute-10-142-20-52 \~\]# modprobe edac_core]

 

[\[root@lg-compute-10-142-20-52 \~\]# modprobe skx_edac]

modprobe: ERROR: could not insert \'skx_edac\': No such device

 

 

[\[root@lg-compute-10-142-20-52 \~\]# grep \[0-9\] /sys/devices/system/edac/mc/mc\*/csrow\*/ch\*\_ce_count]

grep: /sys/devices/system/edac/mc/mc\*/csrow\*/ch\*\_ce_count: No such file or directory

 

[\[root@lg-compute-10-142-20-52 \~\]# ls /sys/devices/system/edac/mc/]

power  subsystem  uevent

 

 

![[Technology_ALL_Linux 问题收集_079_EDAC与Linux_002.png]]

 

 

 

 

 

 

已使用 OneNote 创建。
