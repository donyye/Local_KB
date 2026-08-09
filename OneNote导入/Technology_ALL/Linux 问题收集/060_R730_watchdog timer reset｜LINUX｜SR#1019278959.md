[ R730\|watchdog timer reset]｜LINUX｜SR#1019278959

2020年3月18日

15:36

  ----------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject       答复: R730\|watchdog timer reset｜LINUX｜SR#1019278959
  From          Cao, Ting
  To            Lin, Yongliang; Lee, Jacky
  Cc            Tong, Leo; CN XMN TS ENT L2 SME; Dong, Peter; He, Julian; Zhao, Eddy
  Sent          2020年3月18日 15:20
  Attachments   \<\<Linux 7OMSA9.2.pdf\>\>
  ----------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Hi

查看TSR日志：

  --------------------- ------ ---------- ----------------------------------------------------------------
  2020-03-09 05:46:25   2500   ASR0001    The watchdog timer reset the system.
  2020-03-09 05:46:20   2499   UEFI0082   The system was reset due to a timeout from the watchdog timer.
  2020-03-09 05:46:20   2498   PST0089    A problem was detected during Power-On Self-Test (POST).
  --------------------- ------ ---------- ----------------------------------------------------------------

关于UEFI0082错误：由于看门狗定时器超时，系统被重置

 

Sosreport未收集完整，/var/log内容为空；建议重新收集

 

有查看当前服务器系统版本

Operating System

Name                        : Red Hat Enterprise Linux Server

Version                     : release 7.7 (Maipo) Kernel 3.10.0-1062.9.1.el7.x86_64 (x86_64)

System Time                 : Wed Mar 18 00:32:52 2020

System Bootup Time          : Mon Mar  9 03:50:45 2020

 

固件版本：

BIOS 2.11.0

CPLD        1.0.1

iDRAC & LC      2.70.70.70

Power Supplies 1 & 2      00.23.32

Integrated NIC 1      18.3.6

NICs in Slots 5, 7     18.3.6

Integrated RAID Controller 1  25.5.6.0009

Backplane 1 on Integrated RAID Controller 1 3.35

Disks 0 & 1 in Backplane 1 on Integrated RAID Controller 1  AS0B

Disks 2-9 in Backplane 1 on Integrated RAID Controller 1     NT31

 

系统有安装OMSA9.3 ;

omsa log显示：

Severity      : Critical

ID            : 1404

Date and Time : Mon Mar  9 03:52:47 2020

Category      : Instrumentation Service

Description   : Memory device status is critical

Memory device location: B3

Possible memory module event cause:Single bit warning error rate exceeded,Single bit failure error rate exceeded,Multi bit error encountered

 

Severity      : Non-Critical

ID            : 1014

Date and Time : Mon Mar  9 03:52:09 2020

Category      : Instrumentation Service

Description   : System software event:

Description: A problem was detected during Power-On Self-Test (POST).

Date and time of action: Mon Mar  9 00:46:20 2020

 

Severity      : Critical

ID            : 1006

Date and Time : Mon Mar  9 03:52:09 2020

Category      : Instrumentation Service

Description   : Automatic System Recovery (ASR) action was performed

Action performed was: Reboot

Date and time of action: Wed Apr 14 23:14:36 2156

 

查看B3内存发生多位错误；建议更换；

 

关于WDT导致系统reset操作

if OMSA's Automatic System Recovery (ASR) feature is enabled for the watchdog to issue a power action, DRAC will indeed reboot or power cycle the entire system. Therefore it is strongly recommended to leave the ASR feature disabled.

当前BIOS  WDT设置为disabled;建议在omsa界面禁用ASR

通过客户端远程登录omsa

 

![[Technology_ALL_Linux 问题收集_060_R730_watchdog timer reset｜LINUX｜SR#101927_001.jpg]]

 

发件人: Lin, Yongliang \<Yongliang_Lin@Dell.com\> 

发送时间: 2020年3月18日 10:50

收件人: Lee, Jacky; Cao, Ting

抄送: Tong, Leo; CN XMN TS ENT L2 SME; Dong, Peter; He, Julian; Zhao, Eddy

主题: RE: R730\|watchdog timer reset｜LINUX｜SR#1019278959

 

Hi Ting:

 

Help follow up ..

 

Thanks & Best Regards.

Yongliang Lin

Enterprise Product Engineer

Dell \| Enterprise Support Services

 

From: Lee, Jacky \<<Jacky_Lee1@Dell.com>\>

Sent: Wednesday, March 18, 2020 10:48 AM

To: CN XMN TS Server Escalation

Cc: Tong, Leo

Subject: R730\|watchdog timer reset｜LINUX｜SR#1019278959

 

Hi team

 

请协助分析Linux SOS report,谢谢

 

1.操作系统版本

Red Hat Enterprise Linux Server

2.OEM/非OEM？ 如是单独订单，请同时记录Order#。

非OEM,PSP

3.问题的简单描述

客户在机箱上发现硬件警告，TSR下发现有以下报错

  --------------------- ------ ---------- ----------------------------------------------------------------
  2020-03-09 05:46:25   2500   ASR0001    The watchdog timer reset the system.
  2020-03-09 05:46:20   2499   UEFI0082   The system was reset due to a timeout from the watchdog timer.
  --------------------- ------ ---------- ----------------------------------------------------------------

 

4．需要解决什么问题

分析故障原因

5.用户的其他要求，比如跟进时间和跟进方式

6.系统相关日志，涉及系统不稳定、性能问题，有条件请预先收集好vmsupport 或sosreport log

TSR,SOS report 均已上传。

[https://satc.dell.com/#/4MQH8N2/devices/4MQH8N2](https://satc.dell.com/#/4MQH8N2/devices/4MQH8N2) 

 

Best Regards,

Jacky Lee

Technical Support Engineer , Enterprise Tech Support

Dell Technologies \| Great China ISG Support Services

[Jacky_lee1@dell.com](mailto:Jacky_lee1@dell.com)

 

My manager is <Jonson_C@dell.com> Thanks!

 

Please consider the environment before printing this email.

 

Confidentiality Notice: This email message, including any attachments, is for the sole use of the intended recipient(s) and may contain confidential or proprietary information. Any unauthorized review, use, disclosure or distribution is prohibited. If you are not the intended recipient, immediately contact the sender by reply e-mail and destroy all copies of the original message.

 

 

已使用 OneNote 创建。
