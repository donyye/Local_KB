Windows 2012 R2 HA 收log

Monday, April 25, 2016

2:27 PM

- ::: 
    -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: ST：7041D92\|SR：928716716\|cluster切换慢需要10分钟\|pros\|TW
    发件人     Yin, Guoxun
    收件人     Li, Jiangxiong; F, Chao; Ye, Dony
    抄送       Xu, Xiaoming; Weng, Mark; Lv, Zhiwei; CN XMN TS ENT L2 SME
    发送时间   Monday, April 25, 2016 2:14 PM
    -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Hi Chao

  请收集一下内容回来供分析：

   

  1.  上一次HA切换的具体时间点
  2.  每个节点上的C:\\Windows\\System32\\Winevt\\Logs文件夹下的所有文件，请按节点主机名称分别放在不同的文件夹以区分归属。
  3.  以下命令所产生的所有节点的 cluster log

  get-clusterlog --Destination D:\\ FolderName  -UselocalTime

  1.  每个节点的MPS report log
  2.  网络和存储的拓扑

   

  另外特别说明，请不要把非售后人员放在升级邮件中，如果有需要可以单独邮件Loop！

   

   

  From: Li, Jiangxiong

  Sent: 2016年4月25日 13:57

  To: F, Chao \<Chao_F@Dell.com\>; Ye, Dony \<dony_ye@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

  Cc: Xu, Xiaoming \<Xiaoming_Xu@dell.com\>; Lu, Eason \<Eason_Lu@DELL.com\>; Weng, Mark \<Mark_Weng@Dell.com\>; Lv, Zhiwei \<Zhiwei_Lv@Dell.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

  Subject: RE: ST：7041D92\|SR：928716716\|cluster切换慢需要10分钟\|pros\|TW

   

  Dell - Internal Use - Confidential 

  Guoxun

  Please help on this case

   

   

  Li Jiangxiong

   

   

  From: Li, Jiangxiong

  Sent: 2016年4月25日 13:48

  To: F, Chao \<<Chao_F@Dell.com>\>; Ye, Dony \<<dony_ye@Dell.com>\>

  Cc: Xu, Xiaoming \<<Xiaoming_Xu@dell.com>\>; Lu, Eason \<<Eason_Lu@DELL.com>\>; Weng, Mark \<<Mark_Weng@Dell.com>\>; Lv, Zhiwei \<<Zhiwei_Lv@Dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>

  Subject: RE: ST：7041D92\|SR：928716716\|cluster切换慢需要10分钟\|pros\|TW

   

  Dell - Internal Use - Confidential 

  Dony

  Please help on this case

   

   

  Li Jiangxiong

   

   

  From: F, Chao

  Sent: 2016年4月25日 13:41

  To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

  Cc: Xu, Xiaoming \<<Xiaoming_Xu@dell.com>\>; Lu, Eason \<<Eason_Lu@DELL.com>\>; Weng, Mark \<<Mark_Weng@Dell.com>\>; Lv, Zhiwei \<<Zhiwei_Lv@Dell.com>\>

  Subject: ST：7041D92\|SR：928716716\|cluster切换慢需要10分钟\|pros\|TW

   

  Dell - Internal Use - Confidential 

  case详情：

  cluster切换慢，要10分钟

  客户反馈重开机HA测试时，却出现重开或关闭单一台R730主机后，另外一台R730主机Failover过慢问题(大约要10分钟左右)，

  在这段Failoverfu, 切换期间，其提供给前端应用的档案服务会中断，这点用户是无法接受的，需要协助解决

  之前存储问题已经升级L2，xiaoming，当前问题建议开case到OS的SME处理

  之前问题创建的SR：928522251

  日志已经上传共享文件夹：[\\\\XMNTSDB03\\EntTS_Log\\928716716](file://XMNTSDB03/EntTS_Log/928716716)

   

  冯超 chao feng

  企业级产品工程师

  戴尔\|企业级技术支持  

  客户反馈\| 我表现如何？请联系我的经理[ray_wong@dell.com](mailto:ray_wong@dell.com)

  减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

  24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

   

 

已使用 OneNote 创建。
