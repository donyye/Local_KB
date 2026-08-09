ESXi7U1/U2 USB/SD 设备故障 

2021年6月28日

15:15

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   [FW: \[GSS-China\]  ESXi7U1/U2 USB/SD ]设备故障 
  From      Yin, Guoxun
  To        Ye, Dony
  Sent      2021年6月28日 12:11
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

王工忘记了发你，

所以转给你下

 

 

 

From: Chang Wang \<changw@vmware.com\>

Sent: Monday, June 28, 2021 12:10 PM

To: Lin, Qiang; Yin, Guoxun

Cc: Wan, Chong (VMware); Wang, Chang (VMware)

Subject:[ \[GSS-China\] ESXi7U1/U2 USB/SD ]设备故障 

 

Hi  Qiang

 

最近7.0U2的版本中出现了USB/SD相关的问题，主要涉及两个：

1.  写坏USB/SD -- 需要更换硬件
2.  指令无法写到USB/SD

 

这两个问题操作系统级别的修复会在7.0P03版本修复（预计发布时间2021年7月15日）

 

============================================

问题1: （写坏USB/SD）ESXi 7.0U1/U2 SD Card Corruption

============================================

故障现象：

1. 升级7.0U1/U2之后，USB/SD启动盘发生文件系统损坏（基于目前的SR，U2故障概率高于U1）

2. 可能会导致主机无响应

3. 可能会导致主机发生紫屏

3. 客户环境可能同时间大批量主机发生问题，

 

触发环境：

1. ESXi7.0U1/U2

2. 使用USB/SD作为启动设备

 

日志分析：

1. 日志[\[vmkwarning.log\]]提示找不到bootbank

2021-04-07T08:44:42.242Z cpu23:3614392)ALERT: Bootbank cannot be found at path \'/bootbank\'

2. 【重要: 快速判断依据】日志[\[vobd.log,vmkernel.log\]]提示本地启动盘发生文件系统corruption 

[2021-02-02T19:26:34.577Z: \[vmfsCorrelator\] 2938471510366us: \[esx.problem.vmfs.resource.corruptondisk\] 5fecb7ff-8662d6b5-7732-24b6fde3ff81 LOCKER-5fecb7ff-8662d6b5-7732-24b6fde3ff81]

[2021-02-02T19:31:38.981Z: \[vmfsCorrelator\] 2938700315575us: \[vob.vmfs.resource.corruptondisk\] Volume 5fecb7ff-8662d6b5-7732-24b6fde3ff81 (\"LOCKER-5fecb7ff-8662d6b5-7732-24b6fde3ff81\") might be damaged on the disk. Resource cluster metadata corruption has been detected.]

3. 日志[\[vmkernel.log\]]提示大量和启动设备相关的SCSI SENSE CODE

2021-02-02T19:38:02.798Z cpu7:2097218)ScsiDeviceIO: 4062: Cmd(0x45ba922e3e80) 0x1a, CmdSN 0x1fea2a0 from world 0 to dev \"mpx.vmhba32:C0:T0:L0\" failed H:0x7 D:0x0 P:0x0 Invalid sense data: 0x0 0x0 0x0.

2021-02-02T19:38:02.935Z cpu14:2097225)ScsiDeviceIO: 4062: Cmd(0x45ba92263280) 0x1a, CmdSN 0x1fea2a8 from world 0 to dev \"mpx.vmhba32:C0:T0:L0\" failed H:0x7 D:0x0 P:0x0 Invalid sense data: 0xc7 0xf 0x43.

4. HCL里不对USB/SD卡做就兼容性测试，因此无法验证USB/SD是否受支持

 

故障原因：

1. 发生Corruption的分区使用VMFS-L文件系统，常见为/scratch分区

2. ESXi7.0U1之后 原来使用FAT文件系统的启动盘系统分区现在使用VMFS-L文件系统。

3. 这种文件系统会使得更多更快的IO（R/W）指令写入到分区中，且ESXi不会对IO流量进行限制。

4. 这种高频率的IO（R/W）会造成低性能USB/SD设备发生损坏。

5. ESXi安装手册上要求\"must be created on high-endurance storage devices\"，但是实际情况中，真正使用的USB/SD设备无法到达要求。

6. 任何厂商的USB/SD设备都可能受影响。

 

解决方法：

更换硬件，把Scratch指向其他存储位置

 

参考文档：

1. KB# VMFS-L Locker partition corruption on SD cards in ESXi 7.0 (83376)

 

============================================

问题2: （指令写不到USB/SD上）Bootbank cannot be found at path \'/bootbank\' errors being seen after upgrading to ESXi 7.0 U2 (83963)

============================================

故障现象：

1. 升级7.0U2之后，使用USB/SD作为启动盘的主机发生无响应

触发环境：

1. ESXi7.0U1/U2

2. 使用USB/SD作为启动设备

日志分析：

1. vCenter告警

Alarm \'Host error\' on [esxi.gsslabs.org](http://esxi.gsslabs.org/) triggered by event 75855264 \'Issue detected on [esxi.gsslabs.org](http://esxi.gsslabs.org/) in DATACENTER: Bootbank cannot be found at path \'/bootbank\'

2. 日志[\[vmkwarning.log\]]提示启动设备链路异常

2021-05-17T03:53:57.655Z cpu22:2097681)WARNING: NMP: nmp_DeviceRequestFastDeviceProbe:237: NMP device \"mpx.vmhba32:C0:T0:L0\" state in doubt; requested fast path state update\...

2021-05-17T03:54:17.280Z cpu22:2097681)WARNING: NMP: nmp_DeviceRequestFastDeviceProbe:237: NMP device \"mpx.vmhba32:C0:T0:L0\" state in doubt; requested fast path state update\...

3. 【重要: 快速判断依据】日志[\[vmkernel.log\]]有如下报错

2021-05-16T22:54:29.999Z cpu68:2097681)ScsiPath: 8058: Cancelled Cmd(0x45c900f21240) 0x1a, cmdId.initiator=0x4538f041a598 CmdSN 0x1407e8 from world 0 to path \"vmhba32:C0:T0:L0\". Cmd count Active:0 Queued:2.

这种报错表示scsipath在cancel commands

 

【故障已经发生】解决方法：

1. 重新扫描磁盘可能会解决该问题

2. 重启主机

3. 不需要更换USB/SD设备

 

参考文档：

KB# Bootbank cannot be found at path \'/bootbank\' errors being seen after upgrading to ESXi 7.0 U2 (83963)

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Regards.

Chang Wang 王畅

vSAN Escalation Engineer, GSS China, VMware Inc. 

Office : [86-10-59846480](mailto:86-10-59846480) \| Email : [changw@vmware.com](mailto:changw@vmware.com)

![[Technology_ALL_VMware_分析案例_125_ESXi7U1_U2 USB_SD 设备故障_001.png]]

 

 

 

已使用 OneNote 创建。
