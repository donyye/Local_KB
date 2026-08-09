Vmware OEM

2019年7月1日

9:29

- ::: 
    ------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    Subject   答复: FC630｜ISSUE｜VMWARE｜SR#993048404
    From      Cheng, Felson
    To        Lin, Yongliang; Zhang, Anwin
    Cc        Tang, Hui; CN XMN TS ENT L2 SME; Dong, Peter; Xiong, John
    Sent      2019年6月28日 11:14
    ------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Hi anwin

   

  这个不是oem的许可，只是说出厂有预装esxi系统而已。请确认客户是否有单独采购许可订单。以下为如何辨别oem许可，请仔细查看，避免无效的升级。

   

  ![[Technology_ALL_VMware_license_问题_010_Vmware OEM_001.png]]

   

   

  一、OEM的

  旧的机型VMware 一般随主机下单，无单独的ST。ST下会有对应的订阅服务（建议在delta检查订单信息，以免遗漏）。新机型的vmware一般是单独下单。

  a.  有OEM的ST 7M6NZH2，下图客户订阅的VMware有效期是1年（从出厂日期计算）

  ![[Technology_ALL_VMware_license_问题_010_Vmware OEM_002.png]]

  a.  单独下单的OEM，有单独的ST

  确认ordre type 

  ![[Technology_ALL_VMware_license_问题_010_Vmware OEM_003.png]]

   

  Line item

  ![[Technology_ALL_VMware_license_问题_010_Vmware OEM_004.png]]

   

  ![[Technology_ALL_VMware_license_问题_010_Vmware OEM_005.png]]

   

  二、非OEM 

  （1）3rd 订单的

  ![[Technology_ALL_VMware_license_问题_010_Vmware OEM_006.png]]

   

  ![[Technology_ALL_VMware_license_问题_010_Vmware OEM_007.png]]

   

  （2）有些机器会预装esxi，但不代表一定购买授权，需要提供单独的VMware订单，才可以认为是OEM

      

  ![[Technology_ALL_VMware_license_问题_010_Vmware OEM_008.png]]

   

   

   

   

   

   

  Best Regards.

   

  Felson Cheng

  Server Engineer

  Ent Tech Support, Great China Infrastructure & Client Solutions Support

  Dell EMC \| Support and Deployment Services

   

  发件人: Lin, Yongliang 

  发送时间: 2019年6月28日 10:37

  收件人: Zhang, Anwin; Cheng, Felson

  抄送: Tang, Hui; CN XMN TS ENT L2 SME; Dong, Peter; Xiong, John

  主题: RE: FC630｜ISSUE｜VMWARE｜SR#993048404

   

  Hi felson:

   

  Help it .tks .

   

  Thanks & Best Regards.

  Yongliang Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

   

  From: Zhang, Anwin

  Sent: Friday, June 28, 2019 10:30 AM

  To: CN XMN TS Server Escalation

  Cc: Tang, Hui

  Subject: FC630｜ISSUE｜VMWARE｜SR#993048404

   

   

  Hi ，需要帮忙协助

  a.  操作系统版本: ESXi 6.0 U3 
  b.  OEM/非OEM:OEM 系统

  问题的简单描述：客户机器配置日志服务器和设置，但是客户发现日志服务器无法收到相关日志信息

                                                  客户还想了解 IPV6系统下配置方法和相关步骤

  a.  需要解决什么问题：客户日志服务器无法收集到日志信息
  b.  用户的其他要求，无

   

   

  Anwin_zhang

  Tech Support Analyst, Great China Customer Support Services

  Dell EMC \| Support and Deployment Services

  Dell line: 8887985

  Phone: +86 592 8187985

  [Anwin_zhang@dell.com](mailto:Anwin_zhang@dell.com)

   

  How am I doing? Please contact my manager  [Harvey_Jiang@Dell.com](mailto:Harvey_Jiang@Dell.com) 

   

 

已使用 OneNote 创建。
