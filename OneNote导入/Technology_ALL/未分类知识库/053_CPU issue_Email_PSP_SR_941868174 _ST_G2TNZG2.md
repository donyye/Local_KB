CPU issue\|Email\|PSP\|SR:941868174 \|ST:G2TNZG2

Friday, February 03, 2017

8:57 AM

  -------------------------------------- ----------------------------------------------------------------------------------------------------------------------
  主题       CASE CLOSED\|Normal Escalation\|R730XD\|CPU issue\|Email\|PSP\|SR:941868174 \|ST:G2TNZG2
  发件人     Chen9, Jack
  收件人     Liao, Jone
  抄送       CN XMN TS ENT L2 SME; CN XMN GSD TS ESG MGMT; CN XMN TS Server Coach
  发送时间   Sunday, January 29, 2017 2:53 PM
  -------------------------------------- ----------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

 Hi Jone,

 

I closed the case.

Issue:

CPU issue.

Solution: 

Replace CPU2.

Root Cause:

CPU Problem.

Comments:

 

![[Technology_ALL_未分类知识库_053_CPU issue_Email_PSP_SR_941868174 _ST_G2TN_001.png]]

 

7401转换成0174

 

0174-\>0000 0001 0111 0100

 

解析出来是Cache Hierarchy Errors，更换CPU2,问题解决。

 

Closed email link：

<http://intranet.dell.com/services/CCC_TS/CCC_PLE_Enterprise_L2_Portal/coach/SitePages/Escalation%20Case.aspx?WikiPageMode=Edit&InitialTabId=Ribbon.EditingTools.CPEditTab&VisibilityContext=WSSWikiPage>

 

Jack Chen

2017-1-29

 

 

From: Shen, Mouse

Sent: Tuesday, January 10, 2017 11:45 AM

To: Liao, Jone \<Jone_Liao@Dell.com\>; Chen9, Jack \<Jack_Chen9@Dell.com\>

Cc: Wong1, Jack \<Jack_wong1@Dell.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

Subject: RE: R730XD CPU issue\|SR：941868174 

 

Dell - Internal Use - Confidential 

Hi Jack:

 

               Please help.

 

Mouse Shen

Enterprise Product Engineer

Dell \| Enterprise Support Services

Phone [+86 592 818 5321](tel:1-512-694-9988)

[Mouse.Shen@Dell.com](mailto:Mouse.Shen@Dell.com)

 

From: Liao, Jone

Sent: 2017年1月10日 11:31

To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

Cc: Wong1, Jack \<<Jack_wong1@Dell.com>\>

Subject: R730XD CPU issue\|SR：941868174 

 

Dell - Internal Use - Confidential 

a.     Detail Symptom Descriptions

详细的故障现象描述:服务器CPU2有machine check error

故障的时间点 :

是否可以复现故障 :

如何复现故障 :

 

b.    Troubleshooting Steps

详细的诊断步骤:

1.腾讯客户，报修CPU2 machine check error。

2.派单DSP上门，当前已收集到DSET日志。

3.错误rawdata：0x06000204327358B10004C1287E007401h。

维修记录: (单号/更换的部件/更换后的现象)

Bios/Driver/FW及存储控制器相关FW版本:

 

c.     Current status

客户公司名称:腾讯

业务影响:

升级的原因: 需要L2帮忙分析日志

RM/TAM:michael_xie

 

d.     Must Collect Logs

已收集的日志(请上传至SR下，若无法收集，请注明无法收集的原因): 

DSET

 

 

Jone_Liao

Enterprise Engineer

Enterprise Support Services

Dell \| Global Support and Deployment

如果您对我的服务有任何意见或建议,也可以联系我的经理    [Ray_Wong@dell.com](mailto:jonson_c@dell.com) 

[![[Technology_ALL_未分类知识库_053_CPU issue_Email_PSP_SR_941868174 _ST_G2TN_002.jpg]]](http://www.dell.com.cn/Support)

[Dell TechDirect](http://techdirect.dell.com/)\|在线报修门户网站:提供在线报修,自助部件派单以及报修事件在线管理

使用快速服务代码, 直达售后团队。[获取及了解快速服务代码信息请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)

[![[Technology_ALL_未分类知识库_053_CPU issue_Email_PSP_SR_941868174 _ST_G2TN_003.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Email/TagChange)

 

[![[Technology_ALL_未分类知识库_053_CPU issue_Email_PSP_SR_941868174 _ST_G2TN_004.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Chat/NonConsumerForm?svcTag=8JQMKY1&segment=BSD&chatQueueName=CN.ZH.TS.ENT.CCC&chatQueueClient=UNIFIEDCLIENT&chatQueueMaxWaitTime=1000000&chatQueueUnifiedChatClientTemp=Dell&chatQueueLanguageId=18&productModel=PModel&IsOLIMTag=False)

 

[![[Technology_ALL_未分类知识库_053_CPU issue_Email_PSP_SR_941868174 _ST_G2TN_005.jpg]]](http://www.dell.com/support/article/cn/zh/19/SLN298781)

 

 

已使用 OneNote 创建。
