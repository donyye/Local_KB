BSOD

Thursday, March 02, 2017

9:42 AM

  -------------------------------------- ------------------------------------------------------------------------------
  主题       RE: R530\|OS ISSUE\|PROS\|\|(PROS)\|SR:944609084
  发件人     Yin, Guoxun
  收件人     Lin, Yongliang; Long, Haibao; Hu, Will
  抄送       Zhang, Janice; CN XMN TS ENT L2 SME
  发送时间   Thursday, March 02, 2017 9:38 AM
  -------------------------------------- ------------------------------------------------------------------------------

 

All,

从Haibao之前提供的ST以及BSOD图片看，初步判断上4KN disk的NTFS compression问题导致的蓝屏，建议如下

1：尝试以下workaround恢复并进入系统，然后关闭NTFS compression.

Workaround:

1\) Boot to the recovery console (you should automatically get there after a few Blue Screen crashes)

2\) Launch command prompt

3\) Run the following command:

c:\\windows\\system32\\compact.exe /U c:\\windows\\system32\\drivers\\\*.sys

4\) Reboot

5\) As soon as you successfully boot, disable NTFS compression system-wide so that the CBS Scavenger does not reintroduce the issue again:

fsutil behavior set DisableCompression 1

6\) Reboot again (so the DisableCompression setting takes effect)

2：如果以上方案无效，OS启动前时候按F8进入带网络连接的安全模式，把dump通过U盘或者网络共享copy出来给我们分析

 

 

 

BR.

Guoxun.

From: Lin, Yongliang

Sent: 2017年3月1日 17:18

To: Long, Haibao \<Haibao_Long@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

Cc: Zhang, Janice \<Janice_Zhang@Dell.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

Subject: 答复: R530\|OS ISSUE\|PROS\|\|(PROS)\|SR:944609084

 

Dell - Internal Use - Confidential 

Hi guoxun:

 

Help check os .

 

Thanks & Best Regards.

Yongliang Lin

Enterprise Product Engineer

Dell \| Enterprise Support Services

 

发件人: Long, Haibao 

发送时间: 2017年3月1日 16:59

收件人: CN XMN TS Server Escalation \<[CNXMNTSServerEscalation@DELL.com](mailto:CNXMNTSServerEscalation@DELL.com)\>

抄送: Long, Haibao \<[Haibao_Long@Dell.com](mailto:Haibao_Long@Dell.com)\>; Zhang, Janice \<[Janice_Zhang@Dell.com](mailto:Janice_Zhang@Dell.com)\>

主题: R530\|OS ISSUE\|PROS\|\|(PROS)\|SR:944609084

 

Dell - Internal Use - Confidential 

a.     Detail Symptom Descriptions

详细的故障现象描述: 

故障的时间点 :

是否可以复现故障 :

如何复现故障 :

b.    Troubleshooting Steps

详细的诊断步骤:

1、用户R530新机器，OEM-WIN2012系统，安装Mcafee后加域退出，系统报错，会自动重启服务器，再次登录无法进入系统。

   客户希望DELL工程师上门解决问题，联系销售建议升级L2&RM共同处理。

2、根据用户提供的情况及图片，初步判断为杀毒软件导致的问题，建议用户重新安装系统，再加域，不要安装杀毒软件。

    客户抱怨为何不能先安装杀毒软件再加域，强烈要求我们快速解决。

3、SALES和AE表示这个客户很重要，建议升级L2给解决方案

4、客户拒绝提供日志。

 

 

维修记录: (单号/更换的部件/更换后的现象)

Bios/Driver/FW及存储控制器相关FW版本:

 

c.     Current status

客户公司名称:嘉里建设管理（上海）有限公司

业务影响:

升级的原因: Pending 在哪里？遇到了什么困难？需要（SERVER/OS/Network/Storage）SME L2帮忙做什么？

RM/TAM:

 

d.     Must Collect Logs

已收集的日志(请上传至SR下，若无法收集，请注明无法收集的原因): 

常见日志类型参考(根据实际情况获取相应日志)：

服务器(参考)：DSET log/system log/SOS Report log/IDRAC log/Capture failure等;

存储(参考)：MD/EQL/NAS/CML/DR/DL log;

 

Haibao Long \| 龙海宝

Enterprise Engineer

Enterprise Support Services

Dell \| Global Support and Deployment

如果您对我的服务有任何意见或建议,也可以联系我的经理   [Wei_Wang9@dell.com](mailto:Wei_Wang9@dell.com)  

[![[Technology_ALL_windows_case_032_BSOD_001.jpg]]](http://www.dell.com.cn/Support)

[Dell TechDirect](http://techdirect.dell.com/)\|在线报修门户网站:提供在线报修,自助部件派单以及报修事件在线管理

使用快速服务代码, 直达售后团队。[获取及了解快速服务代码信息请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)

[![[Technology_ALL_windows_case_032_BSOD_002.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Email/TagChange)

 

[![[Technology_ALL_windows_case_032_BSOD_003.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Chat/NonConsumerForm?svcTag=8JQMKY1&segment=BSD&chatQueueName=CN.ZH.TS.ENT.CCC&chatQueueClient=UNIFIEDCLIENT&chatQueueMaxWaitTime=1000000&chatQueueUnifiedChatClientTemp=Dell&chatQueueLanguageId=18&productModel=PModel&IsOLIMTag=False)

 

[![[Technology_ALL_windows_case_032_BSOD_004.jpg]]](http://www.dell.com/support/article/cn/zh/19/SLN298781)

 

 

已使用 OneNote 创建。
