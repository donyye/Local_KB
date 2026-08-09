ISO损坏导致安装ESXI有问题

Monday, December 05, 2016

9:40 AM

- ::: 
    -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: 来自Dell公司的邮件（SR# 939733480 ST#BV11JD2）
    发件人     Yin, Guoxun
    收件人     Lin, Yongliang; Chou, Shaozheng
    抄送       Yang, Yuehang; CN XMN TS ENT L2 SME
    发送时间   Monday, December 05, 2016 9:28 AM
    -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Hi  ShaoZheng,

  这是一个常见的问题，多数跟安装介质损坏有关，请务必按照以下过程处理并反馈每一步具体结果：

   

  1.  重置BIOS，关闭C1E/CSTATS，USB  3.0，  Memory map Above 4G.
  2.  将服务器关机，移除所有电源线，等待5分钟
  3.  观察系统启动的时候是否已经识别到所有内存
  4.  请客户用MD5工具校验下下载的ISO 介质的MD5值，确保与下面的值完全一致，若有一位不同则证明文件已损坏，需要重新下载并校验，若OK则进行下一步。

  <http://www.dell.com/support/home/us/en/19/Drivers/DriversDetails?driverId=6J3M9>

  ![[Technology_ALL_VMware_分析案例_037_ISO损坏导致安装ESXI有问题_001.png]]

  1.  将此ISO通过Idrac挂载并启动进行安装，
  2.  如果仍然失败，请反馈以上操作是否切实执行和具体结果，并提供TSR report

   

   

   

   

  From: Lin, Yongliang

  Sent: 2016年12月5日 9:03

  To: Chou, Shaozheng \<Shaozheng_Chou@DELL.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

  Cc: Yang, Yuehang \<Yuehang_Yang@Dell.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

  Subject: 答复: 来自Dell公司的邮件（SR# 939733480 ST#BV11JD2）

   

  Dell - Internal Use - Confidential 

  Hi guoxun:

   

  help

   

  Thanks & Best Regards.

  Yongliang Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

   

  发件人: Chou, Shaozheng 

  发送时间: 2016年12月2日 18:12

  收件人: CN XMN TS Server Escalation \<[CNXMNTSServerEscalation@DELL.com](mailto:CNXMNTSServerEscalation@DELL.com)\>

  抄送: Lin, Yongliang \<[Yongliang_Lin@Dell.com](mailto:Yongliang_Lin@Dell.com)\>; Yang, Yuehang \<[Yuehang_Yang@Dell.com](mailto:Yuehang_Yang@Dell.com)\>

  主题: FW: 来自Dell公司的邮件（SR# 939733480 ST#BV11JD2）

   

  Dell - Internal Use - Confidential 

  Hi Yongliang,

   

  帮忙找个L2 跟进下这个Esxi安装系统失败的问题，TAM建议升级给Guoxun，多谢！

  当前客户MB和Raid卡已经更换成新备件，机器升级了Bios到2.2.5，用的已经是6.0 U2 的官方包。客户安装系统后重新启动报错如下，已经查了KB，没有看到类似情况，请帮忙协助下，多谢！

  SR# 939733480 ST#BV11JD2

  Regards!

  Shaozheng Chou

   

  From: <Yang6.Liu@cicc.com.cn> \[[mailto:Yang6.Liu@cicc.com.cn](mailto:Yang6.Liu@cicc.com.cn)\]

  Sent: 2016年12月2日 17:13

  To: Chou, Shaozheng \<<Shaozheng_Chou@DELL.com>\>

  Cc: Yang, Yuehang \<<Yuehang_Yang@Dell.com>\>

  Subject: 答复: 来自Dell公司的邮件（SR# 939733480 ST#BV11JD2）

   

  Chou gong，我刚才又修改了以下内容，发现可以安装了，但是重启又遇到以下问题

  Biso中的关闭C1E/C states ,

  CPU Power Management/Memory Frequency :Maximum Performance

   

   

  ![[Technology_ALL_VMware_分析案例_037_ISO损坏导致安装ESXI有问题_002.jpg]]

  发件人: Yang6 Liu (IT) 

  发送时间: 2016年12月2日 16:29

  收件人: \'Shaozheng.Chou@dell.com\'

  抄送: [Yuehang.Yang@dell.com](mailto:Yuehang.Yang@dell.com)

  主题: 答复: 来自Dell公司的邮件（SR# 939733480 ST#BV11JD2）

   

  刚才进行了bios升级，使用您提供的介质进行安装，还没有到安装步骤就出现以下问题

  ![[Technology_ALL_VMware_分析案例_037_ISO损坏导致安装ESXI有问题_003.jpg]]

   

  ![[Technology_ALL_VMware_分析案例_037_ISO损坏导致安装ESXI有问题_004.jpg]]

   

  发件人: [Shaozheng.Chou@dell.com](mailto:Shaozheng.Chou@dell.com) \[[mailto:Shaozheng.Chou@dell.com](mailto:Shaozheng.Chou@dell.com)[\] ]

  发送时间: 2016年12月2日 15:30

  收件人: Yang6 Liu (IT)

  抄送: [Yuehang.Yang@dell.com](mailto:Yuehang.Yang@dell.com)

  主题: 来自Dell公司的邮件（SR# 939733480 ST#BV11JD2）

   

  尊敬的刘先生：

   

  您好！我是Dell公司的技术，今天给您打过电话。附件中是相关升级Bios的方法和具体下载链接，请参考。

   

  Vmware Esxi 6.0 U2下载链接如下：

  [http://downloads.dell.com/FOLDER03955014M/1/VMware-VMvisor-Installer-6.0.0.update02-4192238.x86_64-Dell_Customized-A03.iso](http://downloads.dell.com/FOLDER03955014M/1/VMware-VMvisor-Installer-6.0.0.update02-4192238.x86_64-Dell_Customized-A03.iso)

   

  官方介绍界面原始链接：

  <http://www.dell.com/support/home/cn/zh/cnbsd1/Drivers/DriversDetails?driverId=6J3M9&fileId=3583508147&osCode=XI60&productCode=poweredge-r630&languageCode=cs&categoryId=EC>

   

  Biso中的关闭C1E/C states ,

  CPU Power Management/Memory Frequency :Maximum Performance

   

  Hi Yuehang，

   

  我下周夜班，如果客户白天的Email，[请帮忙转到EEC_CN@dell.com](mailto:请帮忙转到EEC_CN@dell.com) ，同事会帮忙跟进

   

  Shaozheng Chou

  Enterprise Product Engineer

  Dell \| Enterprise Support Services 

  How am I doing? Email my manager <Ray_Wong@dell.com>  with any feedback

  [Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx) \|一款软件插件，可实现远程监控硬件环境、自动收集数据和日志、主动创建案例和派遣零部件

  回复邮件获取详细资料或点击 SupportAssist超链接了解更多信息！！

  戴尔技术支持官方网站：最新驱动程序下载或查询相关技术支持文档可直接访问[www.dell.com.cn/home](http://www.dell.com.cn/home)

  ::: 
    ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    [![[Technology_ALL_VMware_分析案例_037_ISO损坏导致安装ESXI有问题_005.gif]]](http://zh.community.dell.com/techcenter/f/)   [![[Technology_ALL_VMware_分析案例_037_ISO损坏导致安装ESXI有问题_006.gif]]](http://www.weibo.com/techsupportdell)   [![[Technology_ALL_VMware_分析案例_037_ISO损坏导致安装ESXI有问题_007.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Contact-Information)   [![[Technology_ALL_VMware_分析案例_037_ISO损坏导致安装ESXI有问题_008.gif]]](http://www.dell.com/support/contents/cn/zh/cnbsd1/category/Product-Support/Self-support-Knowledgebase)
    ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

   

   

  敬启: 本邮件内容为保密信息，发件人保留与本邮件相关的一切权利。若您误收到本邮件，敬请立刻删除并通知发件人。有关本邮件的保密性和电子邮件风险，请点击[http://www.cicc.com.cn/CICC/chinese/disclaimer](http://www.cicc.com.cn/CICC/chinese/disclaimer) 确保阅读。 

   

  NOTICE: This message may contain confidential or privileged information. If you are not the intended recipient, please advise us immediately and delete this message. Please see <http://www.cicc.com.cn/CICC/english/disclaimer> for further information on confidentiality and the risks inherent in electronic communication.

 

已使用 OneNote 创建。
