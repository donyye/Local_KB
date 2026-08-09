RE: ST:HRLFZX1  SR:906099113 SC8000\|SCOS UPDATE\|PROS SSN:25484

2015年1月19日

8:51

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       RE: ST:HRLFZX1[  SR:906099113 SC8000\|SCOS UPDATE\|PROS SSN:25484]
  发件人     Lin, Ken
  收件人     Lv, Owen; Wu, Jungle
  抄送       CN XMN TS Server Escalation; CN XMN GSD TS ESG MGMT; CN XMN TS Server Coach
  发送时间   2015年1月18日 18:08
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

更正下问题描述:

EDT升级固件导致有一个控制器无法启动.

 

问题处理:

EDT反馈串口没有输出, IP无法ping通, iDRAC无法连接.

 

连接显示器, 报以下错误:

![[Technology_ALL_未分类知识库_035_RE_ ST_HRLFZX1  SR_906099113 SC8000_SCOS_001.png]]

 

重插Slot7的cache card, 无果.

 

安排更换cache card, 故障解决. 

 

分享点: SC30, SC40, SC8000都有VGA接口可以增加troubleshooting的排错机会. 目前只有SC4020没有VGA口.

 

Ken Lin

ProSupport Master Engineer

Dell \| Enterprise Support Services

Office + 86 592 818 5605

Mobile +86 18965185084

How am I doing? Email my manager (<John_Ohare@dell.com>) with any feedback.

 

 

 

\-\-\-\--Original Message\-\-\-\--

From: No_reply@dell.com \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)\]

Sent: Sunday, January 18, 2015 5:45 PM

To: CN XMN TS Server Escalation

Cc: Lv, Owen; Lin, Ken

Subject: ST:HRLFZX1 SR:906099113 SC8000\|SCOS UPDATE\|PROS SSN:25484

 

Detail Symptom Descriptions:EDT 升级固件宕机，客户在线业务。

Troubleshooting Setups:目前EDT 确认当前控制器升级后重启过程宕机，由于在线业务concall L2 ken lin确认。

Current status: Pending 目前EDT 确认当前控制器升级后重启过程宕机，由于在线业务concall L2_lin 确认。

Must Collect Logs:EDT 会直接发送报错信息给L2 ken_lin

 

已使用 OneNote 创建。
