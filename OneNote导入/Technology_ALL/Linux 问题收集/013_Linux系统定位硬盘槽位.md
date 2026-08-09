Linux系统定位硬盘槽位

2015年12月10日

10:30

- ::: 
  +--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | 主题     | RE: DCS7200Z\|HD issue\|pros SR：920678017                                                                                                                                                                                                              |
  +--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | 发件人   | Cao, Ian                                                                                                                                                                                                                                   |
  +--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | 收件人   | Han, Ruyang; Wang, Andy King                                                                                                                                                                                                               |
  +--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | 抄送     | CN XMN TS ENT L2 SME; W, Robin; Li, Jiangxiong; Wang, Xing Fang; Wang, Frederic; Zhang, Bingo; Huang, Percy                                                                                                                                |
  +--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | 发送时间 | 2015年12月10日 10:24 |
  +--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  | 附件     | \<\<SAS2IRCU_P16.zip\>\>                                                                                                                                                                                                                   |
  |                                      |                                                                                                                                                                                                                                            |
  |                                      | \<\<DCS7200Z硬盘定位方法.pdf\>\>                                                                                                                                                                                                           |
  |                                      |                                                                                                                                                                                                                                            |
  |                                      |                                                                                                                                                                                                                                            |
  +--------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
  :::

   

  Andy, Ruyang,

   

  附件是定位硬盘在哪个位置的方法和工具。

  请转发给客户，请客户按照文档提供每个机器故障硬盘的slot ID号，并打开locate LED。

  然后，请安排DSP上门，不需停机。（客户向我抱怨说，我们的DSP总要求停机，3T硬盘的故障其实可以不停机。）

   

  Thanks!

   

  Ian Cao

  Systems Senior Engineer

  Dell \| Datacenter Scalable Solutions \| Data Center Solutions

  Mobile: +86-10-18510251037

  [ian_cao@dell.com](mailto:ian_cao@dell.com) 

   

  From: Han, Ruyang

  Sent: Thursday, December 10, 2015 9:52 AM

  To: Wang, Andy King \<Andy_King_Wang@dell.com\>; Cao, Ian \<Ian_Cao@Dell.com\>; Zeng, Roy \<Roy_Zeng@Dell.com\>

  Cc: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>; W, Robin \<robin_w@Dell.com\>; Li, Jiangxiong \<Jiangxiong_Li@DELL.com\>; Wang, Xing Fang \<Xing_Fang_Wang@DELL.com\>

  Subject: 答复: DCS7200Z\|HD issue\|pros SR：920678017

   

  Dell - Internal Use - Confidential 

  Ian

   

  补充一下，DCS 7200Z中原配是500GB硬盘，报修的故障硬盘是3TB的，

   

  后来又提供了几个存储模块的编号（FNPK242，2QPK242，7PPK242，JPPK242，JPPK242， 8QPK242，8TPK242，DSPK242），型号是：DCS 8000CZ，看配置是匹配的。

   

  这是客户给的信息：

  ![[Technology_ALL_Linux 问题收集_013_Linux系统定位硬盘槽位_001.jpg]]

   

  谢谢！

  韩汝阳 Han, Ruyang

  企业级产品工程师

  戴尔\|企业级技术支持  

  客户反馈\| 我表现如何？请联系我的经理[Jungle_wu@dell.com](mailto:Jungle_wu@dell.com)

  减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

  24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

   

  [![[Technology_ALL_Linux 问题收集_013_Linux系统定位硬盘槽位_002.jpg]]](http://www.dell.com/prodeploy)

    

  [![[Technology_ALL_Linux 问题收集_013_Linux系统定位硬盘槽位_003.jpg]]](http://www.dell.com/certification)

   

  发件人: Wang, Andy King 

  发送时间: 2015年12月10日 9:35

  收件人: Cao, Ian \<[Ian_Cao@Dell.com](mailto:Ian_Cao@Dell.com)\>; Zeng, Roy \<[Roy_Zeng@Dell.com](mailto:Roy_Zeng@Dell.com)\>

  抄送: CN XMN TS ENT L2 SME \<[CN_XMN_TS_ENT_L2_SME@Dell.com](mailto:CN_XMN_TS_ENT_L2_SME@Dell.com)\>; Han, Ruyang \<[Ruyang_Han@Dell.com](mailto:Ruyang_Han@Dell.com)\>; W, Robin \<[robin_w@Dell.com](mailto:robin_w@Dell.com)\>; Li, Jiangxiong \<[Jiangxiong_Li@DELL.com](mailto:Jiangxiong_Li@DELL.com)\>; Wang, Xing Fang \<[Xing_Fang_Wang@DELL.com](mailto:Xing_Fang_Wang@DELL.com)\>

  主题: RE: DCS7200Z\|HD issue\|pros SR：920678017

   

  Dell - Internal Use - Confidential 

  Ian，

   

  ST: DFRK242

  SR: 920678017

  DCS 7200Z

  DATANG SOFTWARE JISHU GROUP LTD 大唐软件技术股份有限公司

  巡检发现8台机器有问题，每台有一个硬盘故障，需要安排DPS上门更换

  出厂配置：

  379-BBUJ 1 x LSI9217-414e

  379-BBUF 2 x 500G Seagate 2.5\" 7.2K SATA

  客户提供报错设备/dev/sdx， 需要确认物理硬盘

   

  请帮忙提供troubleshooting的步骤和定位物理硬盘的方法，谢谢！

   

  B.R

  Andy

   

  From: Li, Jiangxiong

  Sent: Thursday, December 10, 2015 9:09 AM

  To: Han, Ruyang; W, Robin; Wang, Andy King

  Cc: Wang, Andy King; CN XMN TS ENT L2 SME

  Subject: RE: DCS7200Z\|HD issue\|pros SR：920678017

   

  Dell - Internal Use - Confidential 

  Andy

  Please help on this case

   

   

  Li Jiangxiong

   

   

  From: Li, Jiangxiong

  Sent: 2015年12月10日 9:08

  To: Han, Ruyang \<<Ruyang_Han@Dell.com>\>; W, Robin \<<robin_w@Dell.com>\>

  Cc: Wang, Andy King \<<Andy_King_Wang@dell.com>\>; CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>

  Subject: RE: DCS7200Z\|HD issue\|pros SR：920678017

   

  Dell - Internal Use - Confidential 

  Robin

  Please help on this case

   

   

  Li Jiangxiong

   

   

  From: Han, Ruyang

  Sent: 2015年12月9日 18:25

  To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

  Cc: Wang, Andy King \<<Andy_King_Wang@dell.com>\>; W, Robin \<<robin_w@Dell.com>\>

  Subject: DCS7200Z\|HD issue\|pros SR：920678017

   

   

  EU:大唐软件技术股份有限公司

   

  客户报修硬盘直接安排了服务单。后来他们反馈无法确认哪块盘有问题，服务单超时让我们取消单子。

   

  又过一周突然抱怨是我们处理不及时，让我们确认问题，只提供linux下的/dev/sdx。

   

  DCS机器，我们对他的系统和机器都不了解。

   

  按流程升级给L2接口人。

   

   

  ======================================

  Updated.

   

  针对中国电信/中国移动(华胜天成科技)/优酷网的DCS产品报修请按照以下方式处理:

  - 按照客户提供的故障排查结果进行派单;
  - 如果是重复派单或者客户无法排查故障的情况,请升级case 到 L2 Team, 由Case L2 作为接口人升级给DCS Engineer处理 ;

  (DCS产品 的L2联络人: Wang, Andy King/W, Robin)

   

  韩汝阳 Han, Ruyang

  企业级产品工程师

  戴尔\|企业级技术支持  

  客户反馈\| 我表现如何？请联系我的经理[Jungle_wu@dell.com](mailto:Jungle_wu@dell.com)

  减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

  24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

   

  [![[Technology_ALL_Linux 问题收集_013_Linux系统定位硬盘槽位_002.jpg]]](http://www.dell.com/prodeploy)

    

  [![[Technology_ALL_Linux 问题收集_013_Linux系统定位硬盘槽位_003.jpg]]](http://www.dell.com/certification)

   

 

已使用 OneNote 创建。
