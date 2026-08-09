Vmware SR-IOV 设置-1-开启BIOS

2023年5月31日

15:31

进入BIOS 

 

1. 开启 SR-IOV 开关。进入 System BIOS

![[VMware-排错_vCenter_排查_018_Vmware SR-IOV 设置-1-开启BIOS_001.png]]

 

 

2、网卡开启 SR-IOV，选择 Device Setting

![[VMware-排错_vCenter_排查_018_Vmware SR-IOV 设置-1-开启BIOS_002.png]]

 

 

选择相应的网卡

![[VMware-排错_vCenter_排查_018_Vmware SR-IOV 设置-1-开启BIOS_003.png]]

 

选项 Device Level Configuration

![[VMware-排错_vCenter_排查_018_Vmware SR-IOV 设置-1-开启BIOS_004.png]]

 

Virtualization Mode 选择是 SR-IOV 就是开启的，和下面那个NParEP Mode 没关系。

![[VMware-排错_vCenter_排查_018_Vmware SR-IOV 设置-1-开启BIOS_005.png]]

保存重启

 

======完成========

 

Idrac 可用看到 SR-IOV是否有开启。

![[VMware-排错_vCenter_排查_018_Vmware SR-IOV 设置-1-开启BIOS_006.png]]

 

 

TSR 日志检查

![[VMware-排错_vCenter_排查_018_Vmware SR-IOV 设置-1-开启BIOS_007.png]]

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

 

已使用 OneNote 创建。
