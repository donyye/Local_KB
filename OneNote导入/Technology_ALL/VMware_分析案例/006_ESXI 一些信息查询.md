ESXI 一些信息查询

2015年3月6日

9:01

日志分析和判断结果如下：

 

Lspci只能看到以下HBA，并没有fc hba存在，，这与通过ST反查订单显示该服务器并没有配套购买光纤卡相符合，所以初步判断该客户不是通过fc 连接CML

0000:00:1f.2 SATA controller Mass storage controller: Intel Corporation Patsburg 6 Port SATA AHCI Controller \[vmhba0\]

0000:03:00.0 RAID bus controller Mass storage controller: LSI Dell PERC H710 Mini \[vmhba1\]

所查文件：commands/lspci\_-v.txt

 

检查该机目前所挂载的lun，可以看到仅有一个，相关信息如下，初步判断是通过iscsi连接。

naa.6000d31000771000000000000000000d:

   Display Name: COMPELNT iSCSI Disk (naa.6000d31000771000000000000000000d)

   Has Settable Display Name: true

   Size: 8388608

   Device Type: Direct-Access

   Multipath Plugin: NMP

   Devfs Path: /vmfs/devices/disks/naa.6000d31000771000000000000000000d

   Vendor: COMPELNT

   Model: Compellent Vol 

   Revision: 0603

所查文件：commands/localcli_storage-core-device-list.txt 

 

使用的是software iscsi initiator:

iScsiDevice:

   Adapter: vmhba38

   UID: iqn.1998-01.com.vmware:ESXI-1-113d8999

   Driver: iscsi_vmk

   State: online

   Description: iSCSI Software Adapter

所查文件：commands/localcli_storage-san-iscsi-list.txt

 

路径只有一条：

iqn.1998-01.com.vmware:ESXI-1-113d8999-00023d000001,iqn.2002-03.com.compellent:5000d3100077102b,t,0-naa.6000d31000771000000000000000000d:

   UID: iqn.1998-01.com.vmware:ESXI-1-113d8999-00023d000001,iqn.2002-03.com.compellent:5000d3100077102b,t,0-naa.6000d31000771000000000000000000d

   Runtime Name: vmhba38:C0:T1:L1

   Device: naa.6000d31000771000000000000000000d

   Device Display Name: COMPELNT iSCSI Disk (naa.6000d31000771000000000000000000d)

   Adapter: vmhba38

   Channel: 0

   Target: 1

   LUN: 1

   Plugin: NMP

   State: active

   Transport: iscsi

   Adapter Identifier: iqn.1998-01.com.vmware:ESXI-1-113d8999

   Target Identifier: 00023d000001,iqn.2002-03.com.compellent:5000d3100077102b,t,0

   Adapter Transport Details: iqn.1998-01.com.vmware:ESXI-1-113d8999

   Target Transport Details: IQN=iqn.2002-03.com.compellent:5000d3100077102b Alias= Session=00023d000001 PortalTag=0

   Maximum IO Size: 131072

 

 

绑定的是vmkernel port 0，此为management port，不应该用来作为iscsi network使用

Adapter  Vmknic  MAC Address        MAC Address Valid  Compliant

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

vmhba38  vmk0    c8:1f:66:f3:f3:4f  true               false

 

检查vSwitch布局，可以确认未做管理和数据网络的隔离，没有单独的iscsi network

Switch Name      Num Ports   Used Ports  Configured Ports  MTU     Uplinks  

vSwitch0         5632        12          128               1500    vmnic0,vmnic1,vmnic2,vmnic3

 

  PortGroup Name        VLAN ID  Used Ports  Uplinks  

  vlan10                10       0           vmnic0,vmnic1,vmnic2,vmnic3

  VM Network            210      2           vmnic0,vmnic1,vmnic2,vmnic3

  vlan210               210      1           vmnic0,vmnic1,vmnic2,vmnic3

 

至此可以说，目前该主机的iscsi链路和设备配置完全不符合正常应用方式，请协助用户按照附件手册对iscsi 逻辑链路和物理链路调整和配置，使之符合推荐的应用模式，然后继续观察。

另外请提醒用户关于交换机的iscsi 优化也需要考虑。

 

已使用 OneNote 创建。
