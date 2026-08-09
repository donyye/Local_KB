Dset 在各OS版本下的收集方法

Tuesday, February 02, 2016

9:52 AM

  -------------------------------------- ------------------------------------------------------------------------------------------------------
  主题       RE: SR# 923637886[  R720\|Product query\|PROS]
  发件人     Tian, LianGui
  收件人     Li, Jiangxiong
  抄送       CN XMN TS ENT L2 SME
  发送时间   Tuesday, February 02, 2016 9:47 AM
  附件       \<\<sandisk-das-cache-specsheet.pdf\>\>
  -------------------------------------- ------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

 

JiangXiong, 谢谢.

 

昨天还找了另一份资料,也附上.

 

 

 

附上常用的工具和相关的链接，更便捷为您提供主动和自助技术支持，预先硬件日志下载，系统和驱动安装，保修和配置查询：

·         [Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx)工具 \| 可实现远程监控硬件环境、自动收集数据和日志、主动创建案例和派遣部件，试用或是了解更详细的信息，请点[这里](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist) 

·         Dell Dset日志收集工具 \| 是硬件故障分析的利器，更加高效的报修，缩短问题解决时间，建议提前收集好。Windows®版:[点击收集](http://zh.community.dell.com/techcenter/b/weblog/archive/2012/10/21/dset-windows-174)；Linux版:[点击收集](http://zh.community.dell.com/techcenter/b/weblog/archive/2012/10/29/dell-poweredge-dset-linux) ；VMware ESXi5.0版:[点击收集](http://zh.community.dell.com/techcenter/b/weblog/archive/2012/12/27/how-to-collect-dset-log-in-vmware-esxi)    

·         Windows 2008安装指南: 通过Lifecycle安装，请点[这里](http://zh.community.dell.com/support_forums/poweredge/f/279/t/2812)；Dell服务器驱动（Windows )及工具 下载大全:请点[这里](http://zh.community.dell.com/support_forums/poweredge/f/279/t/9585)；保修和配置查询，请点[这里](http://supportapj.dell.com/support/topics/topic.aspx/ap/shared/support/my_systems_info/zh/cn/details?c=cn&l=zh&s=gen)；技术支持论坛：获取更多的资料信息，请点[这里](http://zh.community.dell.com/support_forums/poweredge/f/279/t/9585) 

 

 

LianGui Tian 田联桂

Enterprise Product Engineer

Dell \| Enterprise Support Services 

How am I doing? Email my manager <Gary_Luo@dell.com>   with any feedback

[![[Technology_ALL_未分类知识库_044_Dset 在各OS版本下的收集方法_001.jpg]]](http://www.dell.com/prodeploy)

 

From: Li, Jiangxiong

Sent: Tuesday, February 2, 2016 9:40 AM

To: Tian, LianGui \<LianGui_Tian@dell.com\>

Cc: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

Subject: RE: SR# 923637886 R720\|Product query\|PROS

 

Dell - Internal Use - Confidential 

Hi liangui

这个不支持，你这种是属于shared --storage，如果客户要这样加速，需要购买支出的加速软件Dell Fluid Cache

 

Can SanDisk DAS Cache be used with shared storage servers such as NAS or SAN? 

The SanDisk DAS Cache product is specifically for use with local, non-shared, Direct-Attached Storage (DAS). There are other products offered by Dell available for caching in servers that access shared storage such as Dell Fluid Cache for SAN for a storage area network; as well as network attached storage products.

 

 

Li Jiangxiong

 

 

From: Li, Jiangxiong

Sent: 2016年2月2日 9:28

To: Tian, LianGui \<<LianGui_Tian@dell.com>\>

Cc: CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>

Subject: RE: SR# 923637886 R720\|Product query\|PROS

 

Dell - Internal Use - Confidential 

Liangui

I will work on this case

 

 

Li Jiangxiong

 

 

From: Tian, LianGui

Sent: 2016年2月1日 17:26

To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

Cc: Tian, LianGui \<<LianGui_Tian@dell.com>\>

Subject: SR# 923637886 R720\|Product query\|PROS

 

Dell - Internal Use - Confidential 

 

Hi L2, please help on this case, thanks.

 

 

 

 

将虚拟机放置在服务器本地磁盘中时，利用vSphere Client 可以正常分配Cache给虚机将虚拟机放置在存储中时，利用vSphere Client 不能分配Cache给虚机

 

环境：

存储用的是Dell SCv2000 8GB FC双控制器

服务器是Dell R920，两块SSD配置成RAID 0 分配给SanDisk DAS Cache

vSphere ESXi 版本是 5.5.0-1331820

 

客户的环境比较简单，连的是Brocade 300,再连scv2000.

 

OOO查到资料如下,认为是sandisk das cache不支持san 网络,升级L2 double check.

 

 

Pick your storage: a. Rotating media inside the server b. Storage outside the server with select, direct-attached Dell JBOD\*\* Storage; or FD332 storage blocks within a FX2 server c. Or both - internal server storage and direct-attached JBOD storage

 

SanDisk DAS Cache is a server-level caching software product for Direct Attached Storage,

 

 

 

 

 

 

附上常用的工具和相关的链接，更便捷为您提供主动和自助技术支持，预先硬件日志下载，系统和驱动安装，保修和配置查询：

·         [Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx)工具 \| 可实现远程监控硬件环境、自动收集数据和日志、主动创建案例和派遣部件，试用或是了解更详细的信息，请点[这里](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist) 

·         Dell Dset日志收集工具 \| 是硬件故障分析的利器，更加高效的报修，缩短问题解决时间，建议提前收集好。Windows®版:[点击收集](http://zh.community.dell.com/techcenter/b/weblog/archive/2012/10/21/dset-windows-174)；Linux版:[点击收集](http://zh.community.dell.com/techcenter/b/weblog/archive/2012/10/29/dell-poweredge-dset-linux) ；VMware ESXi5.0版:[点击收集](http://zh.community.dell.com/techcenter/b/weblog/archive/2012/12/27/how-to-collect-dset-log-in-vmware-esxi)    

·         Windows 2008安装指南: 通过Lifecycle安装，请点[这里](http://zh.community.dell.com/support_forums/poweredge/f/279/t/2812)；Dell服务器驱动（Windows )及工具 下载大全:请点[这里](http://zh.community.dell.com/support_forums/poweredge/f/279/t/9585)；保修和配置查询，请点[这里](http://supportapj.dell.com/support/topics/topic.aspx/ap/shared/support/my_systems_info/zh/cn/details?c=cn&l=zh&s=gen)；技术支持论坛：获取更多的资料信息，请点[这里](http://zh.community.dell.com/support_forums/poweredge/f/279/t/9585) 

 

 

LianGui Tian 田联桂

Enterprise Product Engineer

Dell \| Enterprise Support Services 

How am I doing? Email my manager <Gary_Luo@dell.com>   with any feedback

[![[Technology_ALL_未分类知识库_044_Dset 在各OS版本下的收集方法_001.jpg]]](http://www.dell.com/prodeploy)

 

 

已使用 OneNote 创建。
