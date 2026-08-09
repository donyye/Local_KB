Failover 虚拟机不能驱动问题

Thursday, February 23, 2017

2:29 PM

  -------------------------------------- ----------------------------------------------------------------------------------------------------
  主题       RE: SR 942278712[    PS4100\|HW issue\|pros]
  发件人     Yin, Guoxun
  收件人     Lin, Yongliang; Wu, Rory
  抄送       Kang, Alix; CN XMN TS ENT L2 SME
  发送时间   Thursday, February 23, 2017 2:14 PM
  -------------------------------------- ----------------------------------------------------------------------------------------------------

 

Hi Rory,

非OEM windows不在我们的support范围，建议客户获得vendor support.

在此友情提醒下，Windows 默认的disk timeout值为5秒，请考虑下EQ的controller failover需要几秒，如果超过默认timeout值，那么是需要修改的，包括Host/VM等等。

 

![[Technology_ALL_VMware_分析案例_050_Failover 虚拟机不能驱动问题_001.png]]

 

 

BR.

Guoxun.

 

From: Lin, Yongliang

Sent: 2017年2月23日 14:06

To: Wu, Rory \<Rory_Wu@DELL.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

Cc: Kang, Alix \<Alix_Kang@DELL.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

Subject: 答复: SR 942278712 PS4100\|HW issue\|pros

 

Dell - Internal Use - Confidential 

Hi guoxun:

 

Help check OS issue .

 

Thanks & Best Regards.

Yongliang Lin

Enterprise Product Engineer

Dell \| Enterprise Support Services

 

发件人: Wu, Rory 

发送时间: 2017年2月23日 13:47

收件人: CN XMN TS Server Escalation \<[CNXMNTSServerEscalation@DELL.com](mailto:CNXMNTSServerEscalation@DELL.com)\>

抄送: Wu, Rory \<[Rory_Wu@DELL.com](mailto:Rory_Wu@DELL.com)\>; Kang, Alix \<[Alix_Kang@DELL.com](mailto:Alix_Kang@DELL.com)\>

主题: SR 942278712 PS4100\|HW issue\|pros

 

Dell - Internal Use - Confidential 

 

issue:

客户一台EQ在failover后更换电池。但客户发现failover后所有虚机都不能访问，要手动开启才可访问虚机。收集EQ日志L2 alix 分析非硬件问题，从EQL端只看到更换CM1，failover的过程，在50秒内完成，并没有发现服务器logout的记录，建议involve NOS  engineer看一下服务器日志，EQ 连接的两台R620服务器Tag是DC95YBX、CC95YBX安装wind2012 ,hyper-v ,非原厂wind2008,已经收集两台的日志并上传：[\\\\XMNTSDB03\\entts_log\\942278712](file://XMNTSDB03/entts_log/942278712)

 

request:

需OS engineer 帮分析wind2012 ,hyper-v 日志，检查是否是OS问题导致的不能访问

 

 

 

吴天培 Wu, Rory

企业级产品工程师

戴尔\|企业级技术支持  

客户反馈\| 我表现如何？请联系我的经理[Jungle_wu@dell.com](mailto:Jungle_wu@dell.com)

减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

 

 

已使用 OneNote 创建。
