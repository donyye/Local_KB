答复: R730XD\|system unstable issue\|(psp)（SR:981093492）

2018年10月18日

17:42

  ------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   答复: R730XD\|system unstable issue\|(psp)（SR:981093492）
  From      Huang, Dongwei
  To        Lin, Yongliang; Jiang, Yunguang
  Cc        Deltamail_prod; Li, Zhiyong; Luo, Jeason; Lv, Richard; yunguang_jiang@dellcom; CN XMN TS ENT L2 SME; Dong, Peter
  Sent      2018年10月18日 14:10
  ------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Internal Use - Confidential

 

报错硬盘

[\[0:0:20:0\]   disk    ATA      INTEL SSDSC2BB01 0121  /dev/sdd intel 1.5Tb]硬盘非原配。

TSR日志中有硬盘离线的信息

2018-08-06 23:36:19      Drive 20 is installed in disk drive bay 1.

2018-08-06 23:24:09      Drive 20 is removed from disk drive bay 1.

下方是对应报错，阵列卡驱动显示等待阵列卡固件响应，有任务没有处理完，对方硬盘非Dell出厂原配，无PN无法核对固件是否有BUG。

如有需要可以升级阵列卡固件及驱动到最新。

[Oct  9 12:10:50 cld-unknown149632 kernel: \[5416906.873911\] megaraid_sas 0000:03:00.0: \[160\]waiting for 1 commands to complete for scsi0]

[Oct  9 12:10:55 cld-unknown149632 kernel: \[5416911.888993\] megaraid_sas 0000:03:00.0: \[165\]waiting for 1 commands to complete for scsi0]

[Oct  9 12:11:00 cld-unknown149632 kernel: \[5416916.904073\] megaraid_sas 0000:03:00.0: \[170\]waiting for 1 commands to complete for scsi0]

[Oct  9 12:11:05 cld-unknown149632 kernel: \[5416921.919154\] megaraid_sas 0000:03:00.0: \[175\]waiting for 1 commands to complete for scsi0]

Oct  9 12:11:10 cld-unknown149632 kernel: \[5416926.934232\] megaraid_sas 0000:03:00.0: pending commands remain after waiting, will reset adapter scsi0.

Oct  9 12:11:10 cld-unknown149632 kernel: \[5416926.934234\] megaraid_sas 0000:03:00.0: resetting fusion adapter scsi0.

Oct  9 12:11:16 cld-unknown149632 kernel: \[5416932.844435\] megasas: Waiting for FW to come to ready state

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416941.723724\] megasas: FW now in Ready state

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416941.723804\] megaraid_sas 0000:03:00.0: Current firmware supports maximum commands: 928       LDIO thershold: 237

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416942.403060\] megaraid_sas 0000:03:00.0: IOC INIT command return status SUCCESS for SCSI host 0

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416942.419044\] megaraid_sas 0000:03:00.0: firmware type     : Legacy(64 VD) firmware

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416942.419046\] megaraid_sas 0000:03:00.0: controller type   : iMR(0MB)

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416942.419048\] megaraid_sas 0000:03:00.0: Online Controller Reset(OCR)    : Enabled

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416942.419049\] megaraid_sas 0000:03:00.0: Secure JBOD support : No

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416942.419051\] megaraid_sas 0000:03:00.0: jbod sync map legacy/extended         : No/No

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416942.419053\] megaraid_sas 0000:03:00.0: NVMe passthru support     : No

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416942.451336\] megaraid_sas 0000:03:00.0: Jbod map is not supported megasas_setup_jbod_map 6619

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416942.451383\] megaraid_sas 0000:03:00.0: Interrupts are enabled and controller is OPERATIONAL for scsi:0

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416942.485524\] megaraid_sas 0000:03:00.0: Reset successful for scsi0.

Oct  9 12:11:25 cld-unknown149632 kernel: \[5416942.517801\] megaraid_sas 0000:03:00.0: 5472 (592373413s/0x0020/CRIT) - Controller encountered a fatal error and was reset

[Oct  9 12:38:09 cld-unknown149632 kernel: \[5418544.563046\] sd 0:0:20:0: task abort called for scmd(ffff88105ac659c0)]

Oct  9 12:38:09 cld-unknown149632 kernel: \[5418544.563053\] sd 0:0:20:0: CDB:

Oct  9 12:38:09 cld-unknown149632 kernel: \[5418544.563056\] Test Unit Ready: 00 00 00 00 00 00

Oct  9 12:38:09 cld-unknown149632 kernel: \[5418544.563066\] sd 0:0:20:0: task abort: FAILED scmd(ffff88105ac659c0)

Oct  9 12:38:09 cld-unknown149632 kernel: \[5418544.563195\] sd 0:0:20:0: target reset called for scmd(ffff88105ac659c0)

Oct  9 12:38:09 cld-unknown149632 kernel: \[5418544.563202\] sd 0:0:20:0: megasas: target reset FAILED!!

 

发件人: Lin, Yongliang 

发送时间: 2018年10月18日 9:49

收件人: Jiang, Yunguang; Huang, Dongwei

抄送: Deltamail_prod; Li, Zhiyong; Luo, Jeason; Lv, Richard; yunguang_jiang@dellcom; CN XMN TS ENT L2 SME; Dong, Peter

主题: 答复: R730XD\|system unstable issue\|(psp)（SR:981093492）

 

Internal Use - Confidential

 

Ho Dongwei:

 

Help it

 

Yongliang, Lin

Enterprise Product Engineer

Dell \| Enterprise Support Services 

 

发件人: Jiang, Yunguang 

发送时间: Thursday, October 18, 2018 9:36 AM

收件人: CN XMN TS Server Escalation

抄送: Deltamail_prod; Li, Zhiyong; Luo, Jeason; Lv, Richard; yunguang_jiang@dellcom

主题: R730XD\|system unstable issue\|(psp)（SR:981093492）

 

网易要求DELL帮忙分析死机原因 .

a. 操作系统版本：debian 8

b. 非OEM

c. 问题的简单描述:7HVPGQ2 死机两次，只能强制重启.

d. 需要解决什么问题:要求DELL帮忙检查 硬件及sosreport log分析原因 。

e. 用户的其他要求，比如跟进时间和跟进方式：没有具体要求时间，能帮忙找出原因即可，电话或邮件回馈均可。

f. 系统相关日志：提供了sosreport log 及报错截图（已经上传SR）

 

 

已使用 OneNote 创建。
