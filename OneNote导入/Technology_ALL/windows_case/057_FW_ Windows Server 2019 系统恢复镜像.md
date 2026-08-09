FW: Windows Server 2019 系统恢复镜像

2020年1月13日

13:48

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   FW: Windows Server 2019 系统恢复镜像
  From      Zhuang, Jarvis
  To        Ye, Dony
  Sent      2020年1月13日 13:47
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

一：以管理员身份运行CMD，输入命令：DISM /online /Get-CurrentEdition 查询当前版本

标准评估版显示为：ServerStandardEval

数据中心评估版显示为：ServerDatacenterEval

 

![[Technology_ALL_windows_case_057_FW_ Windows Server 2019 系统恢复镜像_001.jpg]]

 

二：使用命令将评估版转换正式版（命令请先在记事本中编辑完毕，然后再复制进CMD中，否则会出现报错）

标准评估版命令为：Dism /online /set-edition:serverstandard /productkey:xxxxx-xxxxx-xxxxx-xxxxx-xxxxx /AcceptEula

数据中心评估版命令为：Dism /online /set-edition: ServerDatacenter /productkey:xxxxx-xxxxx-xxxxx-xxxxx-xxxxx /AcceptEula

 

![[Technology_ALL_windows_case_057_FW_ Windows Server 2019 系统恢复镜像_002.png]]

 

三：按"Y"重启完成升级,此时在系统中就可以查看到此版本为：Windows server 2016 Standard的正式版

 

![[Technology_ALL_windows_case_057_FW_ Windows Server 2019 系统恢复镜像_003.jpg]]

 

 

 

 

 

Best Regards,

Jarvis

 

From: Zhuang, Jarvis

Sent: 2020年1月13日 10:50

To: Ye, Dony

Cc: W, King

Subject: FW: Windows Server 2019 系统恢复镜像

 

OEM镜像我下载好了放在这里，有需要自取。

[\\\\W10CSMZXL2\\Jarvis\\ISO\\MS_2019_SCHI_STD_DDL](file://W10CSMZXL2/Jarvis/ISO/MS_2019_SCHI_STD_DDL)

 

![[Technology_ALL_windows_case_057_FW_ Windows Server 2019 系统恢复镜像_004.png]]

 

 

Best Regards,

Jarvis

 

From: Zhuang, Jarvis

Sent: 2020年1月10日 15:08

To: CN XMN TS ENT L2 Coach; CN XMN TS ENT L2 SME

Cc: Ye, Dony; Wang, Xing Fang; CN XMN TS Server TW

Subject: Windows Server 2019 系统恢复镜像

 

Windows Server 2019 OEM版本部分开始采用数字恢复镜像的方式，而不在是传统的系统安装光盘，客户可通过DDL获取ISO镜像下载地址。

 

Turbo中记录如下：

![[Technology_ALL_windows_case_057_FW_ Windows Server 2019 系统恢复镜像_005.png]]

 

 

DEE 查询结果：

![Machine generated alternative text: ExportReport Recordsperpage庙0二汾\] Showing1-10f1 PartQty IsThisAnUpgrade False IsTransferabIe GLOBAL ProductType WindowsServerMedia First\<1 冫Last ProductOfferDescription WindowsServer2D19Standard,16CORE,DiitallFulfilledRecove GoTo Ima巳，S-Chinese IsUpgradabIeSKUProductDimension False Single N Regeneratebuttondisabled？〔readbelowforpossiblereasons〕](attachments/Technology_ALL_windows_case_057_FW_%20Windows%20Server%202019%20系统恢复镜像_006.png)

 

 

如何从DDL中获取系统恢复镜像下载，请联络Team Coach 使用DEE 查询订单所有权的DDL账户。

[https://www.dell.com/support/software/cn/zh/cndhs1](https://www.dell.com/support/software/cn/zh/cndhs1)[  \--]》DDL入口

请客户使用所有权账户登陆DDL中，即可获取。

获取方法如下：

 

![[Technology_ALL_windows_case_057_FW_ Windows Server 2019 系统恢复镜像_007.png]]

 

 

![[Technology_ALL_windows_case_057_FW_ Windows Server 2019 系统恢复镜像_008.png]]

 

![[Technology_ALL_windows_case_057_FW_ Windows Server 2019 系统恢复镜像_009.png]]

 

 

 

 

Jarvis

Quality Lead, Enterprise Tech Support

Dell Technologies \| Great China Infrastructure & Client Solutions Support

Office [+86-592-8182315](tel:+86-592-8180662) / My manager is <Mars_Zeng@Dell.com> Thanks!

 

Please consider the environment before printing this email.

Confidentiality Notice: This email message, including any attachments, is for the sole use of the intended recipient(s) and may contain confidential or proprietary information. Any unauthorized review, use, disclosure or distribution is prohibited. If you are not the intended recipient, immediately contact the sender by reply e-mail and destroy all copies of the original message.

 

 

已使用 OneNote 创建。
