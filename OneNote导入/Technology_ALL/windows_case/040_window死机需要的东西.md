window死机需要的东西

2017年10月9日

10:59

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       RE: R720\|System unstable issue\|PROS\|SR:954755480
  发件人     Yin, Guoxun
  收件人     Lin, Yongliang; Lin, Bert; Zhou, Vincent
  抄送       Guo, Quanming; Dong, Peter; CN XMN TS ENT L2 SME
  发送时间   2017年10月9日 10:57
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Hi Bert, Vincent,

解释和建议如下:

 

1.为何其中一台虚机会死机.

死机的原因需要分析死机时候的memory dump来获得，没有拿到DUMP分析的话是目前无法判断原因的。

死机模式表现有多种情况，处理方法如下：

对于常见的蓝屏，请务必进入控制面板---》系统\-\--》高级---》启动和故障恢复-à将Dump模式设置为complete memory dump来确保死机的时候能够获得完整的memory dump来提供给我们分析死机原因，出现蓝屏之后请务必等待屏幕提示DUMP完成了再重启，

该设置如之下图片所示

![[Technology_ALL_windows_case_040_window死机需要的东西_001.png]]

 

对于一般性的系统内部卡死，请系统内部设置打开注册表位置HKEY_LOCAL_MACHINE\\System\\CurrentControlSet\\Services\\kbdhid\\Parameters，在其下创建一个名字为CrashOnCtrlScroll的REG_DWORD值并将数值设置为1，必须重启才能让该设置生效，

如果发生系统内部卡死，比如能Ping通但是无法RDP进入，console也无显示或者显示冻结，可以按住键盘右边的CTRL键盘不放，然后按下SCROLL lock键，促使windows 系统crash并且自动产生memory dump， 请务必等待屏幕提示DUMP完成了再重启，不然可能会让DUMP文件不完整从而无法分析原因。

 

 

![[Technology_ALL_windows_case_040_window死机需要的东西_002.png]]

 

对于系统完全卡死，Ping都不通，并且上述的办法无效的情况下，请联系我们，由我们远程协助尝试生成Windows系统的DUMP来分析原因。

 

 

2.虚拟机异常死机后重启：为何卷里的文件系统有变化？

 

强制重启或者死机重启会导致加载到内存中的文件系统临时数据和文件临时数据无法正常回写，从而有可能会导致文件或者文件系统错误，从而导致数据错误/异常甚至丢失。

必须通过合理的备份来避免这种情况。

 

 

 

BR.

Guoxun。

From: Lin, Yongliang

Sent: Monday, October 9, 2017 9:17 AM

To: Lin, Bert \<Bert_Lin@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

Cc: Guo, Quanming \<quanming_guo@Dell.com\>; Dong, Peter \<Peter_Dong@dell.com\>; Zhou, Vincent \<Vincent_Zhou@dell.com\>; CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

Subject: RE: R720\|System unstable issue\|PROS\|SR:954755480

 

Hi guoxun:

 

Help it.

 

 

Yongliang, Lin

Enterprise Product Engineer

Dell \| Enterprise Support Services 

 

From: Lin, Bert

Sent: Friday, October 6, 2017 7:05 PM

To: CN XMN TS Server Escalation \<<CNXMNTSServerEscalation@DELL.com>\>

Cc: Guo, Quanming \<<quanming_guo@Dell.com>\>; Dong, Peter \<<Peter_Dong@dell.com>\>; Zhou, Vincent \<<Vincent_Zhou@dell.com>\>

Subject: R720\|System unstable issue\|PROS\|SR:954755480

 

Dear L2

 

Pls help thanks

 

 

.     Detail Symptom Descriptions

详细的故障现象描述:

RM/pv ts 来电要求直接call L2 确认：

OEM 2008R2上的一台虚机异常死机 

客户直接重启了物理机

（故障时间点和重启原因还在等客户邮件回复 .  刚call back客户. 客户着急. 要求先收集日志再说）

 

b.    Troubleshooting Steps

详细的诊断步骤:

按L2建议  让客户收集 

故障虚机VM support log 

物理机：OEM 2008R2 MPS report log

Dset

 

维修记录: (单号/更换的部件/更换后的现象)

Bios/Driver/FW及存储控制器相关FW版本:

 

c.     Current status

客户公司名称: 昆明船舶设备集团有限公司

业务影响: 客户急着要删除卷恢复业务  

升级的原因: 客户要root cause:

                                     1.为何其中一台虚机会死机.

                                     2.虚拟机异常死机后重启：为何卷里的文件系统有变化？

 

RM/TAM: RM: zhou vincent要求直接 Concall L2

存储TS已升级RM.  SR：954760936

 

d.     Must Collect Logs

已收集的日志(请上传至SR下，若无法收集，请注明无法收集的原因): 

常见日志类型参考(根据实际情况获取相应日志)：

服务器(参考)：DSET log/system log/SOS Report log/IDRAC log/Capture failure等;

存储(参考)：MD/EQL/NAS/CML/DR/DL log;

 

 

 

 

 

 

 

Bert Lin

Enterprise Engineer, Great China Customer Support Services

Dell EMC \| Support and Deployment Services

[bert_lin@dell.com](mailto:Yani_Wang@dell.com)

如果您对我的服务有任何意见或建议,也可以联系我的经理 [richa_zeng@dell.com](mailto:richa_zeng@dell.com) 

 

 

已使用 OneNote 创建。
