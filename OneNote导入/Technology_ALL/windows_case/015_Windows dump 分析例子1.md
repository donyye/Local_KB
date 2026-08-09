Windows dump 分析例子1

Thursday, September 17, 2015

1:27 PM

- ::: 
    -------------------------------------- ---------------------------------------------------------------------------------------------------------
    主题       close case: R730XD\|Unstable issue\|PROS\|SR:916392297
    发件人     Li, Jiangxiong
    收件人     Jiang, Jimmy
    抄送       CN XMN TS ENT L2 SME; CN XMN GSD TS ESG MGMT; CN XMN TS ENT L2 Coach; APJ Ent Resolution Managers China
    发送时间   Thursday, September 17, 2015 11:29 AM
    -------------------------------------- ---------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

  Hi jimmy

  I have done the case

  Solution：upgrade windows 2008 R2 sp1 pack fixed issue

  通过dump分析

  SYMBOL_NAME:  srvnet!SrvAdminIsScopedName+43

   

  FOLLOWUP_NAME:  MachineOwner

   

  MODULE_NAME: srvnet

   

  IMAGE_NAME:  srvnet.sys

   

  DEBUG_FLR_IMAGE_TIMESTAMP:  4a5bc24a

   

  FAILURE_BUCKET_ID:  X64_0x50_srvnet!SrvAdminIsScopedName+43

   

  BUCKET_ID:  X64_0x50_srvnet!SrvAdminIsScopedName+43

   

  Followup: MachineOwner

   

  这个是一个OS的问题，硬件都正常，客户的系统连sp1补丁都没有打，请引导客户升级sp1补丁

  微软介绍sp1补丁解决了此问题，参考下面的连接

  [https://support.microsoft.com/zh-tw/kb/2719594](https://support.microsoft.com/zh-tw/kb/2719594)

   

   

  Case Closed 邮件保存路径：

  [http://moss.ap.dell.com/sites/CCC%20PLE%20Enterprise%20L2%20Portal/L2%20Closed%20Case%20Summary/default.aspx](http://moss.ap.dell.com/sites/CCC%20PLE%20Enterprise%20L2%20Portal/L2%20Closed%20Case%20Summary/default.aspx)

   

   

   

   

  Li Jiangxiong

   

   

  From: Li, Jiangxiong

  Sent: 2015年8月28日 13:19

  To: Jiang, Jimmy \<Jimmy_Jiang@Dell.com\>

  Cc: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

  Subject: RE: R730XD\|Unstable issue\|PROS\|SR:916392297

   

  Dell - Internal Use - Confidential 

  Jimmy

  I will work on this case

   

   

  Li Jiangxiong

   

   

  From: Jiang, Jimmy

  Sent: 2015年8月28日 12:04

  To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

  Cc: Jiang, Jimmy \<<Jimmy_Jiang@Dell.com>\>

  Subject: R730XD\|Unstable issue\|PROS\|SR:916392297

   

  Dell - Internal Use - Confidential 

  Dell - Internal Use - Confidential 

   

  a.  Detail Symptom Descriptions

  详细的故障现象描述:

  客户说机器自6月报修后又出现蓝屏重启.

  查看之前的报修记录，当时工程师check the dump,cust find caused by srvnet.sys

  2015.6.9      8:33

  2015.6.19 8:33

  tel him not hw problem , and may 360 caued . tel him uninstall and observer .

  客户说删除360软件后，还是有同样问题，客户说没有保存蓝屏的图片。

  客户已经收集了最新的mini dump和DSET日志。

  客户公司：四川望锦机械有限公司

  机器用途：文件服务器

   

  a.  Troubleshooting Steps

  详细的诊断步骤:

  维修记录: (单号/更换的部件/更换后的现象)

   

  a.  Current status

  客户公司名称: 四川望锦机械有限公司

  业务影响:/升级的原因: 文件服务器

   

  a.  Must Collect Logs

  已收集DSET日志和system event log， dump

   

   

   

   

   

   

   

  蒋勇 Jimmy_Jiang

  企业级产品工程师

  戴尔\|企业级技术支持  

  客户反馈\| 我表现如何？请联系我的经理[Richa_Zeng@dell.com](mailto:Richa_Zeng@dell.com)

  减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

  24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

   

 

已使用 OneNote 创建。
