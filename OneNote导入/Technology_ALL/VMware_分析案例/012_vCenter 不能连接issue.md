vCenter 不能连接issue

Monday, August 24, 2015

9:37 AM

  -------------------------------------- ----------------------------------------------------------------------------------------------------------------------
  主题       RE: R720\|other query\|EMAIL\|PSP[  SR: 915421391 ST: 7GQ6D02]
  发件人     C, Will
  收件人     Han, Ruyang; Ye, Dony; Yin, Guoxun
  发送时间   Saturday, August 22, 2015 1:37 PM
  -------------------------------------- ----------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Hi Ruyang,

 

现象：

意外断电，全部服务器、虚拟机都宕机。

电力恢复后，服务器、虚拟机依次启动，都正常。

vcenter appliance 5.5开机正常；

vsphere client连接不上；

vsphere web client虽然能够登录，但实际连接不上vcenter，清单内空无一物。

![[Technology_ALL_VMware_分析案例_012_vCenter 不能连接issue_001.png]]

 

远程检查，手动启动服务

![[Technology_ALL_VMware_分析案例_012_vCenter 不能连接issue_002.png]]

 

5480端口登录vcenter管理网页：

手动启动vcenter服务，显示running；

手动test 数据库显示failed. 

 

在putty登录vcenter时，du --h查看空间，发现根目录100%使用量（这边没截图，dony哥有虚拟机，有空可以补个截图哈）。

直觉就去查看/var/log目录，根目录默认是10G，log目录就占掉了6G。（虽然vcenter app整体是125G磁盘，但主要空间都分给数据库存放了，自身的空间还是太小）

ls --lht 就看到logs目录下所有的文件大小，发现ldapmessage-20150819  这个文件6G，其他文件都是几k。。（这货难道是在出错时候会不断的写入数据？这个也算是一个bug吧）

 

删除之后重启，再手动启动服务，查看主服务vpxd以及数据库vpostgres服务状态都是running，就放心了。

果然再次连接后就成功了。

下面是vcenter app里能执行的命令。

![[Technology_ALL_VMware_分析案例_012_vCenter 不能连接issue_003.png]]

 

 

 

花了大把时间，一开始以为是系统挂了，然后觉得是数据库哪里出错了，最后绕了一圈才想到空间问题哈。。

命令不熟，思路跟着客户走，都是挺蛋疼的哈。

 

感谢dony哥指点哈\~

 

 

 

Best regards

 

陈晓伟   Will Chen

企业级产品工程师

戴尔\|企业级技术支持

客户反馈\| 我表现如何？请联系我的经理[Ray_Wong@dell.com](mailto:Ray_Wong@dell.com)

减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

 

From: Han, Ruyang

Sent: 2015年8月20日 20:34

To: C, Will \<will_c@Dell.com\>; Li, Jiangxiong \<Jiangxiong_Li@DELL.com\>; Ye, Dony \<dony_ye@Dell.com\>; Huang, Antti \<Antti_Huang@Dell.com\>

Subject: 答复: R720\|other query\|EMAIL\|PSP SR: 915421391 ST: 7GQ6D02

 

 

顺便检查一下时间：

 

<http://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2116795>

 

 

 

韩汝阳 Han, Ruyang

企业级产品工程师

戴尔\|企业级技术支持  

客户反馈\| 我表现如何？请联系我的经理[Jungle_wu@dell.com](mailto:Jungle_wu@dell.com)

减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

 

发件人: C, Will 

发送时间: Thursday, August 20, 2015 7:09 PM

收件人: Han, Ruyang; Li, Jiangxiong; Ye, Dony; Huang, Antti

主题: RE: R720\|other query\|EMAIL\|PSP SR: 915421391 ST: 7GQ6D02

 

Dell - Internal Use - Confidential 

Hi ruyang,

 

觉得问题应该还是在vcenter server appliance上，下午远程过去收集了一些日志。。

 

Web client虽然能登录上去，但是实际上是连接不到vcenter的，所以整个清单都是空的。。。

初步看vcenter的database没有起来。。。明早客户会把今天收下来的日志发到FTP上，再具体看看吧\~

 

 

 

Best regards

 

陈晓伟   Will Chen

企业级产品工程师

戴尔\|企业级技术支持

客户反馈\| 我表现如何？请联系我的经理[Ray_Wong@dell.com](mailto:Ray_Wong@dell.com)

减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

 

From: Han, Ruyang

Sent: 2015年8月20日 18:46

To: Li, Jiangxiong \<<Jiangxiong_Li@DELL.com>\>; C, Will \<<will_c@Dell.com>\>; Ye, Dony \<<dony_ye@Dell.com>\>; Huang, Antti \<<Antti_Huang@Dell.com>\>

Subject: 答复: R720\|other query\|EMAIL\|PSP SR: 915421391 ST: 7GQ6D02

 

 

Hi Antti

 

估计Will说的软件应该是vRanger，从描述来看问题应该出在这个软件上，从来没接触过vRanger，COE Team能帮忙么？

 

 

韩汝阳 Han, Ruyang

企业级产品工程师

戴尔\|企业级技术支持  

客户反馈\| 我表现如何？请联系我的经理[Jungle_wu@dell.com](mailto:Jungle_wu@dell.com)

减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

 

发件人: Li, Jiangxiong 

发送时间: Thursday, August 20, 2015 12:33 PM

收件人: C, Will; Han, Ruyang; Ye, Dony

抄送: CN XMN TS ENT L2 SME

主题: RE: R720\|other query\|EMAIL\|PSP SR: 915421391 ST: 7GQ6D02

 

Dell - Internal Use - Confidential 

Dony and ruyang

Please help on this case

 

 

Li Jiangxiong

 

 

From: C, Will

Sent: 2015年8月20日 11:30

To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

Cc: C, Will \<<will_c@Dell.com>\>

Subject: R720\|other query\|EMAIL\|PSP SR: 915421391 ST: 7GQ6D02

 

Dell - Internal Use - Confidential 

VMware vSphere 5 Enterprise, 2 CPU, 1 Year Subscription  2014/1

ESXi 5.5

vronger备份

18号晚上意外断电，重启后虚拟机基本都恢复生产。

目前问题是在管理以及备份上。

vsphere client 登录vcenter appliance提示未知网络问题。

vronger备份软件在与vcenter连接时提示连接断开。

web client能够登录，但发现虚拟机都不在清单中。

重启vcenter appliance在启动ldap 与smtp服务时failed. 

 

TAM kent lu

TSR rick fei

升级L2 跟进。

 

 

 

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
