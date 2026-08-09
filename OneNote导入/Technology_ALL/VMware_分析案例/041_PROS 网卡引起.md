PROS 网卡引起

Thursday, December 15, 2016

10:57 AM

  -------------------------------------- -------------------------------------------------------------------------------------------------------
  主题       Case closed \|Normal Escalation\| M820\|HBA ISSUE\|(PROS)\|SR: 939801213 
  发件人     Han, Ruyang
  收件人     Zhou, Kevin
  抄送       CN XMN TS Server Coach; CN XMN GSD TS ESG MGMT
  发送时间   Thursday, December 15, 2016 10:51 AM
  -------------------------------------- -------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Hi Kevin

 

I have done the RA.

 

Issue:

ESXi PSOD

 

Solution: 

Upgrade HBA firmware.

 

Root Cause:

 

![[Technology_ALL_VMware_分析案例_041_PROS 网卡引起_001.png]]

 

Comments:

 

 

Closed email link：

<http://intranet.dell.com/services/CCC_TS/CCC_PLE_Enterprise_L2_Portal/coach/SitePages/Escalation%20Case.aspx?WikiPageMode=Edit&InitialTabId=Ribbon.EditingTools.CPEditTab&VisibilityContext=WSSWikiPage>

 

 

 

Best Regards

Ruyang Han

 

发件人: Lin, Yongliang 

发送时间: 2016年12月5日 16:27

收件人: Zhou, Kevin \<Kevin_Zhou@Dell.com\>; Han, Ruyang \<Ruyang_Han@Dell.com\>

抄送: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

主题: 答复: M820\|HBA ISSUE\|(PROS)\|SR: 939801213 

 

Dell - Internal Use - Confidential 

Ruyang:

 

Help check .

 

Thanks & Best Regards.

Yongliang Lin

Enterprise Product Engineer

Dell \| Enterprise Support Services

 

发件人: Zhou, Kevin 

发送时间: 2016年12月5日 16:24

收件人: CN XMN TS Server Escalation \<[CNXMNTSServerEscalation@DELL.com](mailto:CNXMNTSServerEscalation@DELL.com)\>

主题: M820\|HBA ISSUE\|(PROS)\|SR: 939801213 

 

Dell - Internal Use - Confidential 

Detail Symptom Descriptions

详细的故障现象描述:（请务必详细描述故障现象，例如诊断灯状态，显示器上的报错内容，软件报错信息或截图，故障时间点，频率(对于Unstable Case)，故障前情况等等。） 

客户报修服务器紫屏

1.重启服务器，系统可以正常访问，两周前也发生过一次。

2.客户反馈这台服务器与2S53962，4X53962，故障现象一样

之前两台服务器，针对系统紫屏，收集了日志，也请了VM分析判断，认为是HBA卡故障，当时更换了HBA解决

涉及批量问题，询求L2的确认

故障的时间点 :11/27

是否可以复现故障 :

如何复现故障 :

 

b.    Troubleshooting Steps

详细的诊断步骤:

 

 

 

维修记录: (单号/更换的部件/更换后的现象)

Bios/Driver/FW及存储控制器相关FW版本:

 

c.     Current status

客户公司名称:/业务影响:/升级的原因:/RM/TAM:吉祥人寿

 

d.     Must Collect Logs

已收集的日志(请上传至SR下，若无法收集，请注明无法收集的原因): 

常见日志类型参考(根据实际情况获取相应日志)：

服务器(参考)：DSET log/system log/SOS Report log/IDRAC log/Capture failure等;

存储(参考)：MD/EQL/NAS/CML/DR/DL log;

 

Best Regards

 

Kevin Zhou（周毅）

Enterprise Product Engineer

Dell \| Enterprise Support Services 

How am I doing?E-mail my manager at <Wei_Wang9@Dell.com>

24小时 服务热线： 800-858-0613或400-886-8616

戴尔技术支持官方网站：最新驱动程序下载或查询相关技术支持文档可直接访问[www.dell.com.cn/home](http://www.dell.com.cn/home)

 

 

已使用 OneNote 创建。
