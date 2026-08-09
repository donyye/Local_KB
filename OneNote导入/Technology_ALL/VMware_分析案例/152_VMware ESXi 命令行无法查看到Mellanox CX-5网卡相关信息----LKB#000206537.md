VMware ESXi 命令行无法查看到Mellanox CX-5网卡相关信息\-\-\--LKB#000206537

2023年3月23日

10:19

  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       VMware ESXi 命令行无法查看到Mellanox CX-5网卡相关信息\-\-\--LKB#000206537
  发件人     Wong1, Jack
  收件人     CN XMN TS ENT L2 SME
  发送时间   2022年12月16日 20:30
  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

故障现象：

客户配置了[Mellanox MT27800 Family \[ConnectX-5\] InfiniBand]网卡，VMware ESXi 使用命令#esxcli network nic list无法看到这张网卡信息，其他机器Cx-6可以看到。

![[Technology_ALL_VMware_分析案例_152_VMware ESXi 命令行无法查看到Mellanox CX-5网卡相关信息_001.png]]

 

诊断过程：

1.  检查TSR日志，硬件可以正常识别到这张卡，这张卡的状态也是正常的。

![[Technology_ALL_VMware_分析案例_152_VMware ESXi 命令行无法查看到Mellanox CX-5网卡相关信息_002.png]]

1.  在ESXi 系统PCI设置那里也是可以正常看到这张卡，状态也是正常的。

![[Technology_ALL_VMware_分析案例_152_VMware ESXi 命令行无法查看到Mellanox CX-5网卡相关信息_003.png]]

1.  更新网卡固件，还是一样，没有变化。
2.  客户反馈VMware已经帮忙安装了驱动还是一样。

 

故障原因：

#esxcli network nic list命令查看的是Ethernet设备，如果InfiniBand网卡使用的类型不是Ethernet的话，那这个命令就无法查看到信息的。

 

 

解决方案：

在BIOS---System Setup---Device Setting对应的CX-5网卡，将Network Link Type 设置成Ethernet就可以了。

 

![[Technology_ALL_VMware_分析案例_152_VMware ESXi 命令行无法查看到Mellanox CX-5网卡相关信息_004.png]]

 

 

Jack Wong

Senior Engineer \| China - ISG Global Network and Compute

Dell Technologies \| APJC ISG Support Services

Office +86 0592 818 6526 EXT. 8886526

Certified: Redhat RHCS,RHCE7,RHCE8\| Linux LPIC1&LPIC2\| Vmware VCP6,VCP2022,VCAP2022,VSAN6.7 \|DELLEMC DEA-41T1, DES-4121, DCPPE-200\|ITIL \| CCNA\|Nutanix NPP5\|

[Jack_wong1@dell.com](mailto:Yongliang_lin@Dell.com)

 

已使用 OneNote 创建。
