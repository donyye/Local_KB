案例1：iSCSI Lun 0B （0KB）

2023年4月27日

12:21

 

 

从iscsi 挂载过来的lun 看到空间是 0B，它是重启后变这样了，而多次重启也是这样。

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_001.png]]

 

每个vm都会跳出这个警告提示

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_002.png]]

 

lun还挂给了其它两台物理机，上面能看到使用空间多少，剩下多少，但是这两台没有重启过。

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_003.png]]

 

记录lun的信息

HEARTBEAT  naa.6000d3bf3600000000000000005, partitlon 1

 

 

 

裸设备可以看到容量：

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_004.png]]

 

 

esxcfg-scsidevs -m

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_005.png]]

 

记录信息：

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_006.png]]

 

通过收集的信息来查找log。

cd /var/run/log/

grep -I \"naa.xxxxxxxxxxxxxxxx\" ./vmkernel.log

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_007.png]]

这些日志正常。

 

grep -I \"naa.xxxxxxxxxxxxxxxx\" ./vobd.log

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_008.png]]

这些log没看到问题。

 

esxcfg-mpath -b

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_009.png]]

正常

 

Vmware -vl

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_010.png]]

 

检测文件系统

partedUtil getptbl /vmfs/device/disks/naa.xxxxxxxxxxxxxxxxxx03

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_011.png]]

分区有在，是正常的。

 

 

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_012.png]]

 

esxcli storage vmfs snapshot list

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_013.png]]

 

[ ]esxcfg-vswtich -l

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_014.png]]

 

esxcfg-vmknic -l

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_015.png]]

 

Syslog.log

有出现 login target failed

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_016.png]]

 

 

再一次重启 host , 查看上一次启动的记录：

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_017.png]]

从最后几行看是正常的。

 

所有启动日志过一遍发现问题

最后发现有 No space left on device 错误信息，说明 storage 空间满了，无法挂载此lun。

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_018.png]]

 

最后到storage 里看，果然是存储空间满了。SC存储

![[VMware-排错_ESXI_排查_011_案例1：iSCSI Lun 0B （0KB）_019.png]]

 

 

最后需要通过要存储扩容lun空间才能被正常的挂载。

 

已使用 OneNote 创建。
