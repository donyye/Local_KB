vSAN 磁盘方面检测

2023年4月18日

14:26

ESXI

 

检测这个盘是否被vsan所支持

登录idrac \-- storage \-- physical disks

![[VMware-排错_VSAN_排错_006_vSAN 磁盘方面检测_001.png]]

使用 product ID 到 Vmware HCL 去查

 

ESXI 上的命令检测：

1. vdq -iH[   (vm-support ]日志里也有 vdq 文件)

![[VMware-排错_VSAN_排错_006_vSAN 磁盘方面检测_002.png]]

一个磁盘组，一个SDD加3个HDD。

 

下面这种是有问题的

![[VMware-排错_VSAN_排错_006_vSAN 磁盘方面检测_003.png]]

 

2. 对vSAN存储进行检查         

\[root@localhost:\~\] localcli vsan storage list \|grep CMMDS

   In CMMDS: true

   In CMMDS: true

   In CMMDS: true

   In CMMDS: true

 

3.  esxcfg-scsidevs -m

![[VMware-排错_VSAN_排错_006_vSAN 磁盘方面检测_004.png]]

 

esxcfg-scsidevs -l[  列出所有磁盘]详细信息

esxcli vsan storage list 列出vsan磁盘，如果没信息是有问题。

 

 

4. 如果vsan在建磁盘组时无法看到磁盘或是某些盘显示不出来，那检测一下磁盘是否有分区在，因为有分区的磁盘不会被vsan显示。

esxcfg-scsidevs -l \|grep -i naa

partedUtil getptbl /vmfs/devices/disks/naa.xxxxxxxxxxxxxxx

![[VMware-排错_VSAN_排错_006_vSAN 磁盘方面检测_005.png]]

 

有分区需要做一个格式的动作。

![[VMware-排错_VSAN_排错_006_vSAN 磁盘方面检测_006.png]]

 

如果清除分区很久没有完成

1）检测vsan的网络是否10G。

2）是否有很多任务叠加中无法执行。如下图这种情况，很多任务的等待执行中。

![[VMware-排错_VSAN_排错_006_vSAN 磁盘方面检测_007.png]]

3）尝试重启host

 

 

 

5. 磁盘0KB

![[VMware-排错_VSAN_排错_006_vSAN 磁盘方面检测_008.png]]

 

在日志里 smartinfo.txt 可以看到

![[VMware-排错_VSAN_排错_006_vSAN 磁盘方面检测_009.png]]

 

 

 

 

 

 

已使用 OneNote 创建。
