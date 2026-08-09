安装HIT会导致多出infiniband进程

1:43 PM

  -------------------------------------- ----------------------------------------------------------------------------------------------------------
  主题       CASE CLOSE: SR:SR:913971085[  HIT install problem]
  发件人     Wu, Xiaotao
  收件人     Li3, David
  抄送       Ye, Dony; CN XMN TS Server Coach; CN XMN GSD TS ESG MGMT
  发送时间   Tuesday, July 21, 2015 1:38 PM
  -------------------------------------- ----------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Hello David:

  我将关闭这个RA，以下是CASE总结

 

故障现象：

在安装HIT过程中，客户发现会添加INFINIBAND 监控进程。

 

解释：

经过L4确认，该为正常情况，第一次安装会加载该监控模块。

 

建议:

在升级前拿到足够的信息，便于L2理解问题

 

Wu Xiaotao

ProSupport Master Engineer

Dell \| Enterprise Support Services

Office + 86 592 818 8779

How am I doing? Email my manager (<John_Ohare@dell.com>) with any feedback.

 

From: Li, Jiangxiong

Sent: Friday, July 17, 2015 3:06 PM

To: Ye, Dony; Wu, Xiaotao

Cc: Li3, David; CN XMN TS ENT L2 SME; CN XMN TS Storage Escalation

Subject: RE: R920\|System unstable issue\|(PROS)\|SR:913971085

 

Dell - Internal Use - Confidential 

Xiaotao and dony

Please help on this case

 

Jiangxiong

 

 

\-\-\-\--Original Message\-\-\-\--

From: <No_reply@dell.com> \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)\]

Sent: 2015年7月17日 10:24

Cc: CN XMN TS Server Escalation; Li3, David

Subject: R920\|System unstable issue\|(PROS)\|SR:913971085

 

详细的故障现象描述:

TAM Yang,Wei 来电反映安装HIT 软件后，系统出现大量关于infiniband 的进程

root 56206 0.0 0.0 0 0 ? S May21 0:00 \[infiniband/0\]

故障问题发生时间段:安装HIT 后

详细的诊断步骤:

卸载HIT，不装HIT 就不会有

抓取SOS report

维修记录: (单号/更换的部件/更换后的现象)

Bios/Driver/FW及存储控制器相关FW版本:

客户公司名称:/业务影响:/升级的原因:/RM/TAM:

上海期货交易所

TAM yang, wei

已收集的日志(请上传至SR下):

 

已使用 OneNote 创建。
