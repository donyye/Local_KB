Windows 不稳定

Wednesday, July 22, 2015

8:30 AM

- ::: 
    -------------------------------------- ---------------------------------------------------------------------------------
    主题       RE: R320\|system unstable issue\|pros\|SR:914049814
    发件人     Yin, Guoxun
    收件人     Fang, Yubin
    抄送       CN XMN TS ENT L2 SME; Li, Jiangxiong
    发送时间   Wednesday, July 22, 2015 8:00 AM
    -------------------------------------- ---------------------------------------------------------------------------------
  :::

   

  Yubin

   

  从软硬件日志记录的情况看，能供我们判断问题的内容并不多。

  硬件方面确实没有报错，软件方面并未有会引起意外关机的异常记录，可以排除人为从主机端强制关机行为。且非正常关机的记录没有规律，呈现完全的随机性。

  综合目前在日志中看到的情况，请按顺序做以下安排，并务必反馈每一项是否执行和成功执行，以备后续跟踪此case

   

  1.  请按用户重启进入安全模式，以管理员登录，运行命令sfc /scannow 对系统关键文件做扫描检查，如果有损坏，按照提示修复
  2.  将windows dump设置为kernel memory dump
  3.  派单安排更换主板，电源模块，电源后续模块/线缆等，具体备件你查一下，将整个电源链路全部更换
  4.  更换主板后，将BIOS/IDRAC更新到最新
  5.  设置电源管理为最大性能，关闭C1E/CSTSTS，
  6.  完成以上所有操作后，通知我webex远程设置电源事件的event tracing

   

   

  From: Li, Jiangxiong

  Sent: 2015年7月21日 14:59

  To: Yin, Guoxun

  Cc: Fang, Yubin; CN XMN TS ENT L2 SME

  Subject: RE: R320\|system unstable issue\|pros\|SR:914049814

   

  Dell - Internal Use - Confidential 

  Guoxun

  Please help on this case

  Jiangxiong

   

   

  \-\-\-\--Original Message\-\-\-\--

  From: <No_reply@dell.com> \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)\]

  Sent: 2015年7月21日 14:46

  Cc: CN XMN TS Server Escalation; Fang, Yubin

  Subject: R320\|system unstable issue\|pros\|SR:914049814

   

  Detail Symptom Descriptions

  R320 OEM WIN2012系统

  6月份开始机器每天自动重启

   

  Troubleshooting Steps

   

  客户提供了DSET，无硬件报错 

  DSET下系统启动时间有多条Abnormal Shutdown记录 对应时间的HW LOG显示OEM software event.

  S110阵列卡无tty log

  DSET内系统记录 软件记录无明显报错信息

   

   

  Current status

   

   

  Must Collect Logs

  DSET,MPS report 已经发给guoxun

 

已使用 OneNote 创建。
