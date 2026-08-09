storage-性能方面

2020年3月13日

16:02

 

\> ABRTS 

每秒钟有多少个scsi命令被丢弃。如有数值，需要检查vmkernel是否有相关报错。这个lun到scsi是有问题的。如网卡，驱动等。

esxtop 按 f , 然后选"Error Stats"

![[VMware-排错_ESXI-优化_001_storage-性能方面_001.jpg]]

 

![[VMware-排错_ESXI-优化_001_storage-性能方面_002.jpg]]

 

\> 选择d可以看到HBA卡

![[VMware-排错_ESXI-优化_001_storage-性能方面_003.jpg]]

vmhba0 \~ 31 编号的是硬件的HBA卡。

vmhba32  开始的都是software的HBA卡。

从DAVG 、KAVG、QAVG 、GAVG可以看到HBA卡的延时有多大。

例子：

![[VMware-排错_ESXI-优化_001_storage-性能方面_004.jpg]]

 

![[VMware-排错_ESXI-优化_001_storage-性能方面_005.jpg]]

 

\> u

看所有lun的信息

![[VMware-排错_ESXI-优化_001_storage-性能方面_006.png]]

 

这些lun的信息，是可以在vCenter上对应的。

![[VMware-排错_ESXI-优化_001_storage-性能方面_007.png]]

 

 

 

 

已使用 OneNote 创建。
