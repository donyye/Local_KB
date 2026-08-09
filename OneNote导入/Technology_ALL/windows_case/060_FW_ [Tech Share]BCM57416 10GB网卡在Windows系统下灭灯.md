[FW: \[Tech Share\]BCM57416 10GB]网卡在Windows系统下灭灯

2020年4月16日

11:52

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   [FW: \[Tech Share\]BCM57416 10GB]网卡在Windows系统下灭灯
  From      Wang, Xing Fang
  To        CN XMN TS ENT L2 SME; CN XMN TS ENT L2 Coach
  Sent      2020年4月16日 11:49
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Fyi

 

XingFang Wang

Manager, Great China Infrastructure & Client Solutions Support

Dell EMC \| Support and Deployment Services

office +86-592-818-5846

Mobile +86-180-3023-3742

[Xing_Fang_Wang@dell.com](mailto:Xing_Fang_Wang@dell.com)

 

 

From: Liao, Hanet \<Hanet_Liao@DELL.com\>

Sent: Thursday, April 16, 2020 11:10 AM

To: Wang, Xing Fang; Lin, Qiang

Cc: Chen, Paul

Subject:[ FW: \[Tech Share\]BCM57416 10GB]网卡在Windows系统下灭灯

 

FYI. It might be helpful for TS as well.

 

 

Regards,

Hanet Liao

更多服务请访问  [RDC部署实施售前支持网站](https://my.one.dell.com/personal/hanet_liao/Blog/IDS/_layouts/15/start.aspx#/)

 

From: Jian, Xincan \<<Xincan_Jian@Dell.com>\>

Sent: Thursday, April 16, 2020 10:03 AM

To: CCC_RS_DE

Subject:[ \[Tech Share\]BCM57416 10GB]网卡在Windows系统下灭灯

 

Hi team

 

客户环境： 2套R540均配置了BCM57416 10GB网卡，安装Windows2016系统

故障现象：2台服务器在操作系统内查看网卡均是灭灯状态，进入BIOS时2张网卡均可点亮。期间更换固件及驱动版本，重新拔插网卡均无法解决

 

![[Technology_ALL_windows_case_060_FW_ [Tech Share]BCM57416 10GB网卡在Windows系统_001.png]]

解决方法：进入操作系统，将设备管理器下的网口速率从自动改为10GB 全双工后，网卡就up起来了。

![[Technology_ALL_windows_case_060_FW_ [Tech Share]BCM57416 10GB网卡在Windows系统_002.jpg]]

 

![计算机生成了可选文字: 过老(V) 产和0 〕w《ServerBacu 助（H） BroadcomNetXtremeE-SeriesAdvancedDual-port10GbSFP+Ethern. 幫规罱级驱程详盲旦事#资电源管理 此网名适配器可使甲下列到性·在肀边单壬能的性，然后在右 厩 边远择它的0 性回： ReceiveSideScaling RecvSegmentCo引"ci四0 RecvSegmentCoalescing06） RoCEMTLJ ROCEProtocolVersion RSSBaseProcessorGroup RSSBaseProcessorNumber RSSloadbalancingprofile RSSMaxProcessorGroup Speed&0」司宀 SR-IOV TCP/IJDPChecksum0删oad04 TCP/UDPChecksumOffload05 TransmitBuffers(0=Auto) VFSpoofingProtection 10GbpsFullDuplex 1，0GbpsFull0凵p\] 10GbpsFull」plex AutoNegotiation Adapter Adapter#](attachments/Technology_ALL_windows_case_060_FW_%20%5BTech%20Share%5DBCM57416%2010GB网卡在Windows系统_003.png)

 

 

Warmest regards,

 

Jian,Xincan

Senior Deployment Engineer

APJ ProDeploy \| Remote Installation Services (RIS)

Dell EMC \| Infrastructure Delivery Services, Great China

[![[Technology_ALL_windows_case_060_FW_ [Tech Share]BCM57416 10GB网卡在Windows系统_004.jpg]]](http://www.dell.com/prodeploy)

 

[![[Technology_ALL_windows_case_060_FW_ [Tech Share]BCM57416 10GB网卡在Windows系统_005.jpg]]](http://www.dell.com/certification)

 

 

已使用 OneNote 创建。
