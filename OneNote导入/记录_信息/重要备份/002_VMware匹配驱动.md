VMware匹配驱动

2023年2月9日

14:04

通过 网卡的VID DID 与 SVID 来匹配网卡的驱动：

1\. 先在TSR确定这个几个值

![[记录_信息_重要备份_002_VMware匹配驱动_001.png]]

2\. 然后再通过VMware的兼容列表来找到合适的驱动。

在vm-support里查找：

\# egrep -A1 -i \'ethernet\|Emulex\|Fibre Channel\' commands/lspci\_-v.txt

\...\...

0000:3b:00.1 Ethernet controller Network controller: Broadcom Limited BCM57416 NetXtreme-E 10GBASE-T RDMA Ethernet Controller \[vmnic5\]

  Class 0200: 14e4:16d8

# 此命令会返回网卡的 VID 和 DID，返回格式为：nnnn:nnnn。第一组编号为 VID；第二组编号为 DID。

通过VID和DID来查找SVID与SSID，如下：

\# grep \'14e4:16d8\' commands/esxcfg-info\_-a.txt\*

commands/esxcfg-info\_-a.txt.FRAG-00017: \|\-\-\--Display identifier\...\...\...\...\...\...\...\...\...14e4:16d8 14e4:4160

commands/esxcfg-info\_-a.txt.FRAG-00017: \|\-\-\--Display identifier\...\...\...\...\...\...\...\...\...14e4:16d8 14e4:4160

commands/esxcfg-info\_-a.txt.FRAG-00017: \|\-\-\--Display identifier\...\...\...\...\...\...\...\...\...14e4:16d8 14e4:4161

commands/esxcfg-info\_-a.txt.FRAG-00017: \|\-\-\--Display identifier\...\...\...\...\...\...\...\...\...14e4:16d8 14e4:4161

对应关系：

VID : 14e4

DID : 16d8

SVID : 14e4

SSID : 4160

![[记录_信息_重要备份_002_VMware匹配驱动_002.png]]

================================

环境：

R640 + BCM 57416 + BCM 5720

VMware ESXi 6.5.0 build-5310538

 

背景：

R640安装ESXI6.5发现其中BCM 57416 找不到，而另外一张BCM 5720可以被系统认到。

 

1. 要确定 ESXi/ESX 主机是否能查看到57416的网卡设备，运行命令：

\[root@esxia:\~\] lspci -v \|grep -A1 -i ethernet

0000:01:00.0 Ethernet controller Network controller: Broadcom Corporation NetXtreme BCM5720 Gigabit Ethernet \[vmnic0\]

    Class 0200: 14e4:165f

\--

0000:01:00.1 Ethernet controller Network controller: Broadcom Corporation NetXtreme BCM5720 Gigabit Ethernet \[vmnic1\]

    Class 0200: 14e4:165f

\--

0000:19:00.0 Ethernet controller Network controller: Broadcom Limited BCM57416 NetXtreme-E Dual-port 10GBase-T RDMA Ethernet 

    Class 0200: 14e4:16d8

\--

0000:19:00.1 Ethernet controller Network controller: Broadcom Limited BCM57416 NetXtreme-E Dual-port 10GBase-T RDMA Ethernet 

    Class 0200: 14e4:16d8

\[root@localhost:\~\] 

# 两张网卡系统都能被正确认到，说明有可能是驱动问题导致或检查系统是否支持此网卡。记下返回的 PCI 插槽和总线编号 (xx:xx)，进行下一步。

 

2. 要获取新网卡的供应商 ID (VID) 和设备 ID (DID)，请运行以下命令：

\[root@esxia:\~\] lspci -n \|grep 00:19

0000:19:00.0 Class 0200: 14e4:16d8 

0000:19:00.1 Class 0200: 14e4:16d8 

# 此命令会返回网卡的 VID 和 DID，返回格式为：nnnn:nnnn。第一组编号为 VID；第二组编号为 DID。

 

 

 

 

 MAX SSID：有时候会找不到，是vendor迭代版本信息。

3. 要确定正在运行的 ESX/ESXi 版本是否支持该网卡，请执行以下操作：

![[记录_信息_重要备份_002_VMware匹配驱动_003.png]]

 

 

注意：如果没看到所列出的 ESXi/ESX 版本，则表明网卡尚未认证。很有可能目前这个版本还不支持这张卡。

另外需要注意的是有些驱动是有固件版本要求的，所以升级驱动的时候需要升级FW到指定的版本。

 

![[记录_信息_重要备份_002_VMware匹配驱动_004.png]]

 

 

 

 

4. 确认已在系统上加载相应的驱动程序，请执行以下操作：

 

对于 ESXi 主机，请运行以下命令检查驱动是否被加载：

[\[root@esxia:\~\] vmkload_mod -l \|grep bnxtnet]

 

\# 你会发现没有任何输出，说明没找到BCM 57416 NIC的驱动，这个网卡自然不能被认到。

 

\[root@localhost:\~\] vmkload_mod -l \|grep ntg3

ntg3        2    52 

\# 而另外一张BCM 5720的NIC有被认到。

 

\[root@localhost:\~\] esxcli software vib list \|grep  bnxtnet

\[root@localhost:\~\] 

\[root@localhost:\~\] esxcli software vib list \|grep  ntg3

ntg3     4.1.0.0-1vmw.650.0.0.4564106       VMW    VMwareCertified  2001-01-01  

\# 你会发现BCM 57416根本就没有驱动，而另外一个BCM 5720是有驱动的。

 

 

5. 下载驱动并安装驱动

下载动作过于简单略过\...,直接跳到安装驱动步骤。

\[root@localhost:\~\] esxcli software vib install -v /tmp/bnxtnet-20.6.302.0-1OEM.650.0.0.4598673.x86_64.vib 

Installation Result

  Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.

  Reboot Required: true

  VIBs Installed: BCM_bootbank_bnxtnet_20.6.302.0-1OEM.650.0.0.4598673

  VIBs Removed: 

  VIBs Skipped: 

\[root@localhost:\~\] 

\# 安装成功后有提示需要重启才会生效。

 

 

6. 重启完成后的检查

\[root@localhost:\~\] esxcli software vib list \|grep bnxtnet

bnxtnet     20.6.302.0-1OEM.650.0.0.4598673      BCM    VMwareCertified  2001-01-01  

 

最后你也能成功看机器上的所有网口：

![[记录_信息_重要备份_002_VMware匹配驱动_005.png]]

 

 

==============

HBA卡驱动查找

普通查找方法 =============

![[记录_信息_重要备份_002_VMware匹配驱动_006.png]]

\# grep -A1 -i \'SCSI controller\' commands/lspci\_-v.txt 

0000:18:00.0 Mass storage controller Serial Attached SCSI controller: Avago (LSI Logic) Dell HBA330 Adapter \[vmhba7\]

  Class 0107: 1000:0097

\# VID 1000

\# DID 0097

 

VSAN HBA卡驱动 ===============

注意：如果是VSAN的HBA驱动不能在这里找，需要直接搜索，找到有VSAN字样的。

 

如

[DELL : HBA330/HBA330+](https://www.vmware.com/resources/compatibility/detail.php?deviceCategory=vsanio&productid=41261&vcl=true)

ESXi 6.7 U3 (vSAN 6.7 Update 3),ESXi 6.7 U2 (vSAN 6.7 Update 2),ESXi 6.7 U1 (vSAN 6.7 Upda\....

SAS\...

![[记录_信息_重要备份_002_VMware匹配驱动_007.png]]

需要注意，这个驱动是否支持 All Flash 或是 Hybrid的，如果只支持All Flash ，更新在客户混合模式下，那VSAN就挂了。

 

============驱动名字匹配============

lsi_msgpt3   \|   H330 

 

已使用 OneNote 创建。
