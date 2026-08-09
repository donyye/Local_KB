PERC卡或RAID卡驱动导致Windows unstable

Thursday, February 04, 2016

9:33 AM

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       CASE CLOSED\| Normal Escalation: 7RVPGZ1服务器再次出现故障（第5次）\|SR 923210813
  发件人     Wang, Andy King
  收件人     Chou, Shaozheng
  抄送       CN XMN TS Server Coach; CN XMN GSD TS ESG MGMT
  发送时间   Wednesday, February 03, 2016 11:59 PM
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Shaozheng,

I had done the RA.

Issue:

Windows 2003 unstable issue.

Solution: 

Uninstall 360 same issue

Replace raid card and update raid card drivers.

Root Cause:

在OMSA日志中发现Controller event log: Controller encountered a fatal error and was reset: Controller 0 (PERC H710P Mini)

Raid card drivers:

\- Fixed an issue that could result in a system lock-up requiring a system reboot for a non-boot controller or a BSOD for the boot controller. This was seen when running AppAssure backup and recovery software under Windows, but could potentially happen in other heavy I/O environments. This issue can occur with an H310, H710, H710P or an H810 controller.

Comments:

 

 

 

Closed email link：

<http://intranet.dell.com/services/CCC_TS/CCC_PLE_Enterprise_L2_Portal/coach/SitePages/Escalation%20Case.aspx?WikiPageMode=Edit&InitialTabId=Ribbon.EditingTools.CPEditTab&VisibilityContext=WSSWikiPage>

 

B.R

Andy King Wang

 

 

 

From: Li, Jiangxiong

Sent: Wednesday, January 20, 2016 1:45 PM

To: Chou, Shaozheng \<Shaozheng_Chou@DELL.com\>; Wang, Andy King \<Andy_King_Wang@dell.com\>

Cc: Chen, Kevin \<Kevin_Chen@Dell.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

Subject: RE: 7RVPGZ1服务器再次出现故障（第5次）\|SR 923210813

 

Dell - Internal Use - Confidential 

Andy

Please help on this case

 

 

Li Jiangxiong

 

 

From: Chou, Shaozheng

Sent: 2016年1月20日 13:38

To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

Cc: Li, Jiangxiong \<<Jiangxiong_Li@DELL.com>\>; Chen, Kevin \<<Kevin_Chen@Dell.com>\>

Subject: FW: 7RVPGZ1服务器再次出现故障（第5次）\|SR 923210813

 

Hi Jiangxiong,

 

请帮忙升级这个Case给Andy，TAM之前请Andy帮忙搞过，问题依旧，需要解决，多谢啦！

SR 923210813

 

Shaozheng Chou

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From: Chen, Kevin

Sent: Tuesday, January 19, 2016 10:18:33 PM

To: CN, EEC; Zhang, Eileen

Cc: Wang, Andy King; Li, Jiangxiong; Wang, Xing Fang

Subject: RE: 7RVPGZ1服务器再次出现故障（第5次） 

Auto forwarded by a Rule

 

Dell Customer Communication

 

EEC,

请马上向客户发送raid tty log收集工具及方法，并升级给L2 Andy,用户：[fanghao@foundersc.com](mailto:fanghao@foundersc.com)  ，谢谢！

 

 

Best Regards

Kevin Chen 谌衍民

Dell GSD Technical Account Manager

 

MP : +86 18668237999

How am I doing? Email my manager at [Steven_BJ_Zhu@dell.com](mailto:Steven_BJ_Zhu@dell.com)

 

 

From: Chen, Kevin

Sent: Wednesday, January 20, 2016 12:05 PM

To: \'方昊\'; CN, EEC

Cc: <ouxueliang@foundersc.com>; Wang, Andy King; Wang, Xing Fang; Zhang, Eileen; Zhu, Steven BJ; jiang, hao

Subject: RE: 7RVPGZ1服务器再次出现故障（第5次）

 

Dell Customer Communication

 

方工，

 

您好！

已经知悉此2013年10月产R720服务器还有"卡死"现象和您的要求。 方正证券是戴尔的大客户，我们将以大客户的方式 处理您所遇到的设备问题。 

 作为技术部门，有责任向公司内部说明发生了什么技术故障，能否协助发送最新的DSET日志，以便我们能向上汇报处理。 当然您允许，我也将派遣人员上门收集DSET日志。谢谢！

 

上次我们的技术客服人员向您说明以下技术状况，如有不同意见，请提出，我会马上给您打电话：

 

1. 当前操作系统是Windows2003，由于R720的机器最低支持到2008，所以已经告知客户非官方授权系统。

2  .硬件日志未发现相关报错

3. BIOS/IDRAC/RAID CARD 固件过期，可以尝试更新。

4. 阵列卡固件过期，重启会丢失之前记录，所以无法查看到重启之前的日志信息

5. 查看系统有安装360安全中心，经常遇到360防病毒软件导致的系统不稳定情况，建议完全卸载观察

6.  操作系统有安装OMSA, 从OMSA记录的阵列卡日志：信息 Server Administrator Mon Dec 14 13:19:46 2015 2334 Controller event log: Shutdown command received from host: Controller 0 (PERC H710P Mini)  

   说明硬件工作正常，阵列卡可以正常收到关机指令，并正常执行，所以可以判断故障为软件方面。

 

 

 

Best Regards

Kevin Chen 谌衍民

Dell GSD Technical Account Manager

 

MP : +86 18668237999

How am I doing? Email my manager at [Steven_BJ_Zhu@dell.com](mailto:Steven_BJ_Zhu@dell.com)

 

 

From: 方昊 \[[mailto:fanghao@foundersc.com](mailto:fanghao@foundersc.com)[\] ]

Sent: Wednesday, January 20, 2016 11:39 AM

To: CN, EEC

Cc: Chen, Kevin; 欧学亮

Subject: 7RVPGZ1服务器再次出现故障（第5次）

 

之前已报修过的7RVPGZ1服务器又一次出现卡死故障。上次已配合DELL二线工程师及现场工程师做过日志收集分析等工作，当时二线工程师的处理意见是升级固件firmware及卸载安装的360软件。目前看来没有效果。

如果DELL售后技术不能找到故障节点，我们能否申请更换一台整机。谢谢？

 

 

方昊  Fang Hao

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

方正证券股份有限公司 Founder Securities Co., Ltd.

信息技术中心Information Technology Centor

Tel：+ 86 731 82240424

Fax：+ 86 731 82240240

Mobile：+86 18073133735

Email：[fanghao@foundersc.com](mailto:fanghao@foundersc.com)

URL：[www.foundersc.com](http://www.foundersc.com)

湖南省长沙市车站北路459号证券大厦三层 410001

3F Security Mansion,No.459, North chezhan Road,Changsha, Hunan, P.R.China. 410001

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

方正证券（601901.SH）是行业领先的大型综合类证券公司，致力于为客户提供交易、投融资、财富管理等全方位金融服务。

Founder Securities (601901.SH), an industry-leading large comprehensive securities company, is committed to providing its clients with full services in stock transactions, investment \&amp; financing, wealth management, among others.

 

发件人: 方昊

发送时间: 2015年12月14日 16:07

收件人: [EEC_CN@dell.com](mailto:EEC_CN@dell.com)

抄送: [kevin_chen@dell.com](mailto:kevin_chen@dell.com); 欧学亮

主题: 两台DELL服务器出现故障，请报修 

 

服务器1：序列号B6WRF52，硬盘1亮黄灯告警。已尝试替换一块新硬盘，告警恢复正常。请安排工程师上门维修。

 

服务器2：序列号7RVPGZ1，之前10月份已报修过一次（服务单：80903903140）。今天又一次出现同样的故障，屏幕卡死且鼠标键盘无反应，只能强制重启。已抓取DSET日志，请尽快分析排错。

 

谢谢！

 

方昊  Fang Hao

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

方正证券股份有限公司 Founder Securities Co., Ltd.

信息技术中心Information Technology Centor

Tel：+ 86 731 82240424

Fax：+ 86 731 82240240

Mobile：+86 18073133735

Email：[fanghao@foundersc.com](mailto:fanghao@foundersc.com)

URL：[www.foundersc.com](http://www.foundersc.com)

湖南省长沙市车站北路459号证券大厦三层 410001

3F Security Mansion,No.459, North chezhan Road,Changsha, Hunan, P.R.China. 410001

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

方正金融是方正集团下属的五大核心产业集团之一。

方正金融业务范围涉及证券、期货、公募基金、投行、直投、信托、财务公司、保险、商业银行、租赁等。

Founder Financial, one of the five core sectors of Founder Group. 

Its business covers securities, futures, mutual fund, investment banking, direct investment, trust, corporate financing, 

insurance, commercial banking and leasing.

 

已使用 OneNote 创建。
