关于12G/13G机器CPU IERR和MCE的问题处理步骤

Wednesday, July 15, 2015

10:49 AM

- ::: 
    -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       关于12G/13G机器CPU IERR和MCE的问题处理步骤
    发件人     Lian, Wenxiang
    收件人        CN XMN EEC HK
    抄送       CN XMN TS ENT L2 SME
    发送时间   Wednesday, July 15, 2015 10:45 AM
    -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

  Team,

   

  CPU MCE and IERR的case相对比较复杂，如果没有客户强烈要求派单测试的情况下，不建议派单处理，务必做以下步骤进行观察。

   

  步骤如下：

   

  1.  升级BIOS和idrac with lifecycle到最新版本；
  2.  关闭如下BIOS的4个选项（C1E, C STATES Monitor/Mwait， memory patrol scrub）：

  ![[Technology_ALL_案例分析[重要]_031_关于12G_13G机器CPU IERR和MCE的问题处理步骤_001.png]]

   

  3，收集DSET日志确认报警信息，参考KB QNA43687进行判断处理。

  ![[Technology_ALL_案例分析[重要]_031_关于12G_13G机器CPU IERR和MCE的问题处理步骤_002.png]]

   

  Here\'s method to identify which DIMM has problem.

  The last two byte of 2nd OEM diagnostic raw data (CX00).

  X Indicate which channel got problem during memory patrol scrub.

   

  For CPU#1 MCE,

  0 ==A1,A5,A9

  1 ==A2,A6,A10

  2 ==A3,A7,A11

  3 ==A4,A8,A12

   

  For CPU#2 MCE,

  0 ==B1,B5,B9

  1 ==B2,B6,B10

  2 ==B3,B7,B11

  3 ==B4,B8,B12

   

   

  4，如果以上你无法判断处理问题，请升级到SME进行分析处理。

   

  Thanks & Regards,

   

  Wenxiang Lian

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

   

   

  From: Lian, Wenxiang

  Sent: Tuesday, July 14, 2015 17:03

  To: Li, Jiangxiong; Lu, Hanhui; Wang, Andy King

  Cc: CN XMN TS ENT L2 SME; W, Robin

  Subject: RE: 4VLMW22\|913425762\|R720\|CPU ISSUE\|PROS\|需要L2 帮忙confirm cpu报错的原因

   

  Dell - Internal Use - Confidential 

  Hanhui,

   

  参考KB: QNA43687

   

  你可以确认目前是A1 memory需要注意，请关闭monitor/Mwait 和Memory Patrol scrub选项。

  Here\'s method to identify which DIMM has problem.

  The last two byte of 2nd OEM diagnostic raw data (CX00).

  X Indicate which channel got problem during memory patrol scrub.

   

  For CPU#1 MCE,

  0 ==A1,A5,A9

  1 ==A2,A6,A10

  2 ==A3,A7,A11

  3 ==A4,A8,A12

   

  For CPU#2 MCE,

  0 ==B1,B5,B9

  1 ==B2,B6,B10

  2 ==B3,B7,B11

  3 ==B4,B8,B12

   

   

  ![[Technology_ALL_案例分析[重要]_031_关于12G_13G机器CPU IERR和MCE的问题处理步骤_002.png]]

   

  ![[Technology_ALL_案例分析[重要]_031_关于12G_13G机器CPU IERR和MCE的问题处理步骤_001.png]]

   

   

  Thanks & Regards,

   

  Wenxiang Lian

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

   

   

  From: Li, Jiangxiong

  Sent: Tuesday, July 14, 2015 15:30

  To: Lu, Hanhui; Wang, Andy King

  Cc: CN XMN TS ENT L2 SME

  Subject: RE: 4VLMW22\|913425762\|R720\|CPU ISSUE\|PROS\|需要L2 帮忙confirm cpu报错的原因

   

  Dell - Internal Use - Confidential 

  Andy

  Please help on this case

  Jiangxiong

   

   

  \-\-\-\--Original Message\-\-\-\--

  From: <No_reply@dell.com> \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)\]

  Sent: 2015年7月14日 15:28

  To: Lu, Hanhui

  Cc: CN XMN TS Server Escalation

  Subject: 4VLMW22\|913425762\|R720\|CPU ISSUE\|PROS\|需要L2 帮忙confirm cpu报错的原因

   

  • Detail Symptom Descriptions

  详细的故障现象描述:

  从三月份开始频繁报错 CPU 1 has an internal error (IERR).

  故障问题发生时间段:

  三月份开始

  • Troubleshooting Steps

  详细的诊断步骤:

  CPU 1 has an internal error (IERR).

  2\*e2603 v2/2\*8g memory

  Operating System Name Red Hat Enterprise Linux Server release 6.5 (Santiago) Version 2.6.32-431.el6.x86_64 #1 SMP Sun Nov 10 22:19:54 EST 2013 x86_64 x86_64 x86_64 GNU/Linux

  四台R720座集群只有这台有问题，对比正常的日志 配置与内核都一样

  1.7.07上门更换cpu+主板，并且把原来cpu2换到cpu1槽位，带过去的cpu换到cpu2槽位

  7.10又继续CPU 1 has an internal error (IERR).

  出现的几率很随机，一天一次或者一天两三次

   

   

  维修记录: (单号/更换的部件/更换后的现象)

  7.07安排主板+cpu进行更换，并且对调了cpu的位置

  Bios/Driver/FW及存储控制器相关FW版本:

  BIOS:2.5.2

  DRAC:1.67.67

  C1E CSTATE都关闭

  • Current status

  客户公司名称:

  业务影响:出问题的时候不会死机，但是有报错 客户想知道原因

  升级的原因:固件均为最新版本，cpu 主板也更换过，dset日志除了cpu 报错无其他报错，需要L2 帮忙找出原因

  RM/TAM:

   

  • Must Collect Logs

  已收集的日志(请上传至SR下):

  SOSREPORT/DSET

 

已使用 OneNote 创建。
