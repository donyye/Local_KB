Linux 下的BMC网口设置

Wednesday, August 05, 2015

10:49 AM

  -------------------------------------- ---------------------------------------------------------------------------------------------
  主题       FW: C6220II\|other query\|EMAIL\|PROS Sr: 914683771 ST: 1KQD762
  发件人     Wang, Xing Fang
  收件人     Xu, Xiaoming
  抄送       CN XMN TS ENT L2 SME
  发送时间   Wednesday, August 05, 2015 10:47 AM
  -------------------------------------- ---------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Xiaoming, Well done!

 

XingFang Wang

Technical Support Manager for CHK LEP & PUB

Dell \| Global Customer Support Services

office +86-592-818-5846

Email [Xing_Fang_Wang@Dell.com](mailto:Your%20name@Dell.com)

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

中文官方技术支持论坛：[http://bbs.dell.com.cn/](http://bbs.dell.com.cn/)

Dell硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

 

From: Wu1, Peter

Sent: Wednesday, August 05, 2015 10:45 AM

To: C, Will; Xu, Xiaoming

Cc: Wang, Xing Fang

Subject: 答复: C6220II\|other query\|EMAIL\|PROS Sr: 914683771 ST: 1KQD762

 

Dell - Internal Use - Confidential 

赞一个xiaoming的专业和速度！！

 

 

 

Peter Wu

吴培刚

China Technical Account Manager

Tel: +8610-58261966

Fax:+8610-58261000

MP:+86 13501399210

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

中文官方技术支持论坛：[http://bbs.dell.com.cn/](http://bbs.dell.com.cn/)

Dell硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

How am I doing? 欢迎您对我的服务做出评价 [Xin_Zheng@dell.com](mailto:Xin_Zheng@dell.com)

 

发件人: C, Will 

发送时间: Wednesday, August 05, 2015 10:36 AM

收件人: Xu, Xiaoming; Wu1, Peter

主题: RE: C6220II\|other query\|EMAIL\|PROS Sr: 914683771 ST: 1KQD762

 

Dell - Internal Use - Confidential 

Hi Xiaoming, Peter,

 

客户使用BMC工具，成功修改BMC网口设置，问题解决。

 

感谢xiaoming哈\~

 

 

 

Best regards

 

陈晓伟   Will Chen

企业级产品工程师

戴尔\|企业级技术支持

客户反馈\| 我表现如何？请联系我的经理[Ray_Wong@dell.com](mailto:Ray_Wong@dell.com)

减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

 

From: Xu, Xiaoming

Sent: 2015年8月3日 16:10

To: Li, Jiangxiong; C, Will

Cc: CN XMN TS ENT L2 SME

Subject: RE: C6220II\|other query\|EMAIL\|PROS Sr: 914683771 ST: 1KQD762

 

Dell - Internal Use - Confidential 

具体命令格式

./bmc nic_mode set \[dedicated\|shared\]

Sets the BMC port to use the dedicated NIC port or the Shared LOM on the system

 

客户系统下直接输入./bmc会显示该命令的具体用法。

 

详细参考KB：SLN266202

 

Best Regards

徐小明

China Commercial Technical Support 

戴尔（中国）技术支持部

 

From: Xu, Xiaoming

Sent: 2015年8月3日 15:59

To: Li, Jiangxiong; C, Will

Cc: CN XMN TS ENT L2 SME

Subject: RE: C6220II\|other query\|EMAIL\|PROS Sr: 914683771 ST: 1KQD762

 

Dell - Internal Use - Confidential 

Will：

    从这里下载[http://www.poweredgec.com/](http://www.poweredgec.com/)  

bmc tool

用这个工具可以在linux系统下直接修改BMC的网络模式。

具体用法参考帮助。

 

Best Regards

徐小明

China Commercial Technical Support 

戴尔（中国）技术支持部

 

From: Li, Jiangxiong

Sent: 2015年8月3日 15:27

To: C, Will; Xu, Xiaoming

Cc: CN XMN TS ENT L2 SME

Subject: RE: C6220II\|other query\|EMAIL\|PROS Sr: 914683771 ST: 1KQD762

 

Dell - Internal Use - Confidential 

Xiaoming

Please help on this case

 

 

Li Jiangxiong

 

 

From: C, Will

Sent: 2015年8月3日 14:56

To: CN XMN TS Server Escalation

Cc: C, Will

Subject: C6220II\|other query\|EMAIL\|PROS Sr: 914683771 ST: 1KQD762

 

Dell - Internal Use - Confidential 

字节跳动

客户一百多台C6220II，需要通过命令行修改BMC网口模式：从shared改为dedicated. 

尝试让用户在服务器上使用ipmitool工具使用相关命令测试：

ipmitool lan print 1 有输出；

ipmitool raw命令就提示命令不可用：

root@n6-130-012\^:\~# ipmitool raw 0x30 0x25

Unable to send RAW command (channel=0x0 netfn=0x30 lun=0x0 cmd=0x25 rsp=0xc1): Invalid command； 

 

升级L2关注，并帮助测试看是否有ipmitool下可用的命令，或者其他可用工具可以实现。

谢谢\~

 

 

 

附上常用的工具和相关的链接，更便捷为您提供主动和自助技术支持，预先硬件日志下载，系统和驱动安装，保修和配置查询：

·         [Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx)工具 \| 可实现远程监控硬件环境、自动收集数据和日志、主动创建案例和派遣部件，试用或是了解更详细的信息，请点[这里](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist) 

·         Dell Dset日志收集工具 \| 是硬件故障分析的利器，更加高效的报修，缩短问题解决时间，建议提前收集好。Windows®版:[点击收集](http://zh.community.dell.com/techcenter/b/weblog/archive/2012/10/21/dset-windows-174)；Linux版:[点击收集](http://zh.community.dell.com/techcenter/b/weblog/archive/2012/10/29/dell-poweredge-dset-linux) ；VMware ESXi5.0版:[点击收集](http://zh.community.dell.com/techcenter/b/weblog/archive/2012/12/27/how-to-collect-dset-log-in-vmware-esxi)    

·         Windows 2008安装指南: 通过Lifecycle安装，请点[这里](http://zh.community.dell.com/support_forums/poweredge/f/279/t/2812)；Dell服务器驱动（Windows )及工具 下载大全:请点[这里](http://zh.community.dell.com/support_forums/poweredge/f/279/t/9585)；保修和配置查询，请点[这里](http://supportapj.dell.com/support/topics/topic.aspx/ap/shared/support/my_systems_info/zh/cn/details?c=cn&l=zh&s=gen)；技术支持论坛：获取更多的资料信息，请点[这里](http://zh.community.dell.com/support_forums/poweredge/f/279/t/9585) 

 

陈晓伟   Will Chen

企业级产品工程师

戴尔\|企业级技术支持  

客户反馈\| 我表现如何？请联系我的经理[Ray_Wong@dell.com](mailto:Ray_Wong@dell.com)

减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

 

 

已使用 OneNote 创建。
