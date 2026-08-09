VCenter提示报错

Thursday, March 10, 2016

1:29 PM

  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------
  主题       RE: ST：2MKDC52，SR：926491923\|VMware系统报错
  发件人     Yin, Guoxun
  收件人     Li, Jiangxiong; Liao, Jone
  抄送       CN XMN TS ENT L2 SME
  发送时间   Thursday, March 10, 2016 12:58 PM
  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------

 

Vmware KB：

<https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2076267>

 

Hi Jone,

下面这条消息是提示"aa.60060e8010348710057f134100000000" 这个Lun I/O平均完成时间增加了而已，折合时间到微秒也才23.2ms，完全属于正常范围，而且存储的响应时间会受I/O类型，大小的影响而变化，只要不是长期处于较高延迟的情况下且存储性能符合实际能力那么下面的消息完全可以忽略。

 

设备警告 naa.60060e8010348710057f134100000000 性能降低。I/O滞后时间已从平均值447微秒增加到23224微秒。

客户在VCenter获取这个报错，客户想定位具体是哪台虚机的问题。

 

 

From: Li, Jiangxiong

Sent: 2016年3月10日 12:52

To: Liao, Jone \<Jone_Liao@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

Cc: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

Subject: RE: ST：2MKDC52，SR：926491923\|VMware系统报错

 

Dell - Internal Use - Confidential 

Guoxun

Please help on this case

 

 

Li Jiangxiong

 

 

From: Liao, Jone

Sent: 2016年3月10日 12:20

To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

Subject: ST：2MKDC52，SR：926491923\|VMware系统报错

 

Dell - Internal Use - Confidential 

•           Detail Symptom Descriptions

详细的故障现象描述:VMware系统，VCenter提示报错，客户想定位问题

 

故障问题发生时间段:3.9,17:33

 

•         Troubleshooting Steps

详细的诊断步骤:

客户随机购买VMware系统，使用ESXi5.1系统，

当前VCenter出现报错。

客户提供报错：

设备警告 naa.60060e8010348710057f134100000000 性能降低。I/O滞后时间已从平均值447微秒增加到23224微秒。

客户在VCenter获取这个报错，客户想定位具体是哪台虚机的问题。

 

维修记录: (单号/更换的部件/更换后的现象)

 

Bios/Driver/FW及存储控制器相关FW版本:

 

•         Current status

客户公司名称:

业务影响:暂无

升级的原因:VMware系统，需要L2帮助

RM/TAM:

 

•         Must Collect Logs

已收集的日志(请上传至SR下):

 

 

Best Regards

Jone_Liao

 

 

已使用 OneNote 创建。
