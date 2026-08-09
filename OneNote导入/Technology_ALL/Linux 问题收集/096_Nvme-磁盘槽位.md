Nvme-磁盘槽位

2024年1月8日

19:08

客户系统里有 nvme磁盘报错，但是TSR是没有显示，NVME 走背板，后面接cable 直接到MB NVME 接口的。不是走的PERC卡。

![[Technology_ALL_Linux 问题收集_096_Nvme-磁盘槽位_001.png]]

 

240:24:00 十进制等于十六进制 0000:f0:18.0

\# ls -al /sys/block/ \|grep nvme

![[Technology_ALL_Linux 问题收集_096_Nvme-磁盘槽位_002.png]]

 

![[Technology_ALL_Linux 问题收集_096_Nvme-磁盘槽位_003.png]]

这样换算来是 11 槽位

 

 

===========

这种是PERC卡的情况下

<https://blog.csdn.net/lemon6666666/article/details/101208184>

Linux操作系统下查询NVMe盘符、Slot ID和Bus ID的对应关系

 

 

已使用 OneNote 创建。
