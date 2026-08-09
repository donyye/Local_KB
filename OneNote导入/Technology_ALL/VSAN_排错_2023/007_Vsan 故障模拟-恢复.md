Vsan 故障模拟-恢复

2023年3月28日

17:01

先退出维护模式

 

看到vm目前状态 inaccessible

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_001.png]]

 

esxcfg-advcfg -g /VSAN/DOMPauseALLCCPs

esxcfg-advcfg -g /VSAN/IgnoreClusterMemberListUpdates

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_002.png]]

 

 

所有node做完之后就，刷新，vm就没有 inaccessible 显示了。

 

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_003.png]]

 

 

 

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_004.png]]

 

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_005.png]]

 

 

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_006.png]]

 

 

 

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_007.png]]

 

 

 

 

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_008.png]]

 

 

 

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_009.png]]

[vSAN Health Service - Data Health -- vSAN Object Health (2108319) (vmware.com)](https://kb.vmware.com/s/article/2108319)

Reduced availability with no rebuild:The object has suffered a failure, but VSAN was able to tolerate it. For example: I/O is flowing and the object is accessible. However, VSAN is not working on re-protecting the object. This is not due to the delay timer (reduced availability - no rebuild - delay timer) but due to other reasons. This could be because there are not enough resources in the cluster, or this could be because there was not enough resources in the past, or there was a failure to re-protect in the past and VSAN has yet to retry. Refer to the limits health check for a first assessment if any resources may be exhausted. You have to resolve the failure or add resources as quickly as possible in order to get back to being fully protected against a subsequent failure.

 

中文显示

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_010.png]]

点击立即修复没有效果

出现此问题vm无法开机。

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_011.png]]

 

 

 

最后是检测到有一个node无法退出维护模式，把"localcli vsan maintenancemode cancel"命令把它退出维护模式就可以了。

原因是资源不足，vsan 无法重建。

另外，可用性降低但是未重建，这个重建默认是等待60分钟的

 

![[Technology_ALL_VSAN_排错_2023_007_Vsan 故障模拟-恢复_012.png]]

 

已使用 OneNote 创建。
