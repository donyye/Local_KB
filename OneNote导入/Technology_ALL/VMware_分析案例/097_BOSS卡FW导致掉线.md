BOSS卡FW导致掉线

2018年11月30日

9:57

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   RE: boss卡又出新固件了
  From      Chen9, Jack
  To        Yin, Guoxun; Ruan, Garuda; CN XMN TS ENT L2 SME; Wang, Zhen1
  Sent      2018年11月30日 9:42
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

 

Hi，

 

VM-support日志如下：反复reset boss卡后，有可能会出现ESXI系统无响应，IDRAC识别不到BOSS卡。

 

![[Technology_ALL_VMware_分析案例_097_BOSS卡FW导致掉线_001.png]]

 

VMware相关KB如下：需要升级驱动到VMware_bootbank_sata-ahci_3.0-27vmw.600.3.104.9919195。

 

[https://kb.vmware.com/s/article/57806](https://kb.vmware.com/s/article/57806)

 

查看驱动版本命令如下：

 

esxcli software vib list \| grep \"sata-ahci\"

 

![[Technology_ALL_VMware_分析案例_097_BOSS卡FW导致掉线_002.png]]

 

现在还是建议客户升级BOSS卡固件到2.5.13.3016，ESXI系统的驱动也升级到VMWARE要求的驱动版本。

 

Jack

2018-11-30

 

From: Yin, Guoxun

Sent: 2018年11月30日 9:24

To: Chen9, Jack; Ruan, Garuda; CN XMN TS ENT L2 SME; Wang, Zhen1

Subject: RE: boss卡又出新固件了

 

ESXI无响应问题和SSD掉线问题

这个有log样本吗？

 

\@WangZhen,

你之前看的那个log片段估计跟这个会有关联，

 

From: Chen9, Jack

Sent: 2018年11月30日 9:21

To: Ruan, Garuda; CN XMN TS ENT L2 SME

Subject: RE: boss卡又出新固件了

 

Dell - Internal Use - Confidential 

3016固件IPS说过会在11月30日发布，看来提前发布了，在十一期间香港遇到的case，3011版本的固件和ESXI发布的BOSS卡驱动没有解决ESXI无响应问题和SSD掉线问题，所以再次发布固件修复出现的问题。希望这次能完全解决BOSS卡的问题。

 

From: Ruan, Garuda

Sent: 2018年11月29日 23:54

To: CN XMN TS ENT L2 SME

Subject: boss卡又出新固件了

 

看修复，真不让人省心啊。。。

 

<https://www.dell.com/support/home/us/en/04/drivers/driversdetails?driverId=MKV82&osCode=WST14&productCode=poweredge-t640>

 

BOSS-S1 Adapter firmware

BOSS-S1 Adapter ROM version 2.5.13.3016

Fixes & Enhancements

Fixes:

\- Fixed a behavior of BOSS-S1 firmware incorrectly marking M.2 drive offline/failed

\- Fixed a behavior where ESXi Host goes unresponsive

\- Fixed a behavior where BOSS-S1 Management path will not respond to Management commands

\- Fixed a behavior where BOSS-S1 boot partition becomes inaccessible

\- Fixed a behavior where ESXi host results in PSOD due to unexpected I/O timeout

\- Fixed a behavior where rebuild will not be proceed during error handling condition

 

Enhancement:

\- Enhanced/ Added MVCLI events for command timeout

\- Added SLES15 Support

 

 

 

 

已使用 OneNote 创建。
