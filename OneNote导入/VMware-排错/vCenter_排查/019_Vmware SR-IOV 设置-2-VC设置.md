Vmware SR-IOV 设置-2-VC设置

2023年6月1日

13:21

环境：VC 8.0 + vsphere 8.0

  --------------------- ------------- ------------ ---------- ----------
  vCenter Server 8.0b   8.0.0.10200   2023-02-14   21216066   21216066
  --------------------- ------------- ------------ ---------- ----------

  ------------- ------------- ------------ ----------
  ESXi 8.0 GA   ESXi 8.0 GA   2022/10/11   20513097
  ------------- ------------- ------------ ----------

 

SR-IOV 可以虚拟多个网卡，在VMware上的体现是多个直通的网卡，给到vm进行使用。

 

选择Host的物理设备器，查看默认是禁用的状态

![[VMware-排错_vCenter_排查_019_Vmware SR-IOV 设置-2-VC设置_001.png]]

 

 

启动 SR-IOV

虚拟数是指能虚拟多少个直通的网卡出来，给到多少台VM加载。

比如这里是4，说明虚拟4个直通网卡，可用给4个不同vm加载

另外配置完后需要重启 host 才能生效

![[VMware-排错_vCenter_排查_019_Vmware SR-IOV 设置-2-VC设置_002.png]]

 

启动后VC会把它虚拟成直通的设备

![[VMware-排错_vCenter_排查_019_Vmware SR-IOV 设置-2-VC设置_003.png]]

 

 

然后启动所有可启动的SR-IOV

![[VMware-排错_vCenter_排查_019_Vmware SR-IOV 设置-2-VC设置_004.png]]

 

 

然后在vm添加网卡上选择类型，确保预留vm内存

![[VMware-排错_vCenter_排查_019_Vmware SR-IOV 设置-2-VC设置_005.png]]

 

 

![[VMware-排错_vCenter_排查_019_Vmware SR-IOV 设置-2-VC设置_006.png]]

 

![[VMware-排错_vCenter_排查_019_Vmware SR-IOV 设置-2-VC设置_007.png]]

 

 

系统看到的结果。2019 自带有驱动可用看到。

 

![[VMware-排错_vCenter_排查_019_Vmware SR-IOV 设置-2-VC设置_008.png]]

 

 

 

 

vmnic2 和 vmnic4 没接网线所以是X。

 

![[VMware-排错_vCenter_排查_019_Vmware SR-IOV 设置-2-VC设置_009.png]]

 

 

 

 

 

 

 

 

 

 

 

 

 

![[VMware-排错_vCenter_排查_019_Vmware SR-IOV 设置-2-VC设置_010.png]]

 

 

 

 

已使用 OneNote 创建。
