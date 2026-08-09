====== 1 =====

[[R960] [IC: 195519914 ] [PC: 195518191 ] [ST: GHMZL24 ]]
TS:Li, Jianxing | IM:N/A |SAM:N/A

[OS: Oracle Linux Server 8.6[ ] \Kernel: 5.4.17-2136.307.3.1.el8uek.x86_64 [] [OEM: N]]

--- 描述 ---

Internal;

Escalation Reason: 技术原因;

Product Type: 服务器;

Product Model: R960;

TSR Log;

日志上传位置: SFDC Related;

Detail Symptom Descriptions(故障现象): 8月1日的时候业务有出现问题，当时网络有异常，存储的数据任务有堆积;

Troubleshooting Steps(详细诊断步骤):

故障现象：系统相关问题；

诊断过程：

8月1日的时候业务有出现问题，当时网络有异常.客户的技术人员调试后，目前到现场没有出现问题

确认之前异常是存储的数据任务有堆积，客户不太了解改善过程，大概是刷掉进程的任务堆积后，就正常使用

客户暂时不做升级固件操作，目前系统正常运作中

客户希望dell协助分析下网卡硬件和网络软件，客户希望二线工程师远程检测下服务器系统，确认上之前出现故障原因

确认服务器安装RH8.6

商议收集tsr日志和系统日志协助分析；

;

Current status(当前状态和要求): 目前服务器系统正常使用中;\

8月1日  凌晨8：29分50秒

====== 2 =====

The logs show that the system is Oracle Linux, which is not in our support list. With the advice given, but rejected.\
----------------------------\
Analyzing:

The feedback system reported an abnormality on the network on Aug 1st and

they wanted to know the reason.

Solution:

Suggest provide more details information and OS log.

Next step:

Waiting for update.

====== 3 =====

Resolution：

 

Root Cause: 

 

 

----Remark----

rac1

Aug  1 08:16:28 rs-24cmmdb-rac01 systemd-logind[5861]: New session 76734 of user oracle.

Aug  1 08:16:28 rs-24cmmdb-rac01 systemd[1]: Started Session 76734 of user oracle.

Aug  1 08:16:38 rs-24cmmdb-rac01 esfdaemon[2876994]: 0

Aug  1 08:16:53 rs-24cmmdb-rac01 systemd-logind[5861]: New session 76735 of user oracle.

Aug  1 08:16:53 rs-24cmmdb-rac01 systemd[1]: Started Session 76735 of user oracle.

Aug  1 08:16:53 rs-24cmmdb-rac01 systemd-logind[5861]: Session 76735 logged out. Waiting for processes to exit.

Aug  1 08:16:53 rs-24cmmdb-rac01 systemd[1]: session-76735.scope: Succeeded.

Aug  1 08:16:53 rs-24cmmdb-rac01 systemd-logind[5861]: Removed session 76735.

Aug  1 08:18:04 rs-24cmmdb-rac01 systemd[1]: session-c67036.scope: Succeeded.

Aug  1 08:18:07 rs-24cmmdb-rac01 su[2879918]: (to oracle) root on none

Aug  1 08:18:07 rs-24cmmdb-rac01 systemd[1]: Started Session c67057 of user oracle.

Aug[  1 08:18:07 rs-24cmmdb-rac01 cdmoracleproc[2879953]: Unabled to add log. \Error: 访问文件 /var/log/AnyBackup/app.log 被拒。 (/var/log/AnyBackup/app.log)（错误提供者：POSIX System，错误值：13，错误位置：fileoutput.cpp:318）[] [log info: 08-01 08:18:07.386564 cdmoracleproc[2879953][2879953](info)(upgradecache ncModuleDepend.h:1015): file does not exist:(/anybackup/AnyBackupClient/ClientService/AggregateApp/locale/zh_CN/upgradecache.mo).]]

Aug[  1 08:18:07 rs-24cmmdb-rac01 cdmoracleproc[2879953]: Unabled to add log. \Error: 访问文件 /var/log/AnyBackup/app.log 被拒。 (/var/log/AnyBackup/app.log)（错误提供者：POSIX System，错误值：13，错误位置：fileoutput.cpp:318）[] [log info: 08-01 08:18:07.387561 cdmoracleproc[2879953][2879953](info)(upgradedameng ncModuleDepend.h:1015): file does not exist:(/anybackup/AnyBackupClient/ClientService/AggregateApp/locale/zh_CN/upgradedameng.mo).]]

Aug[  1 08:18:07 rs-24cmmdb-rac01 cdmoracleproc[2879953]: Unabled to add log. \Error: 访问文件 /var/log/AnyBackup/app.log 被拒。 (/var/log/AnyBackup/app.log)（错误提供者：POSIX System，错误值：13，错误位置：fileoutput.cpp:318）[] [log info: 08-01 08:18:07.388316 cdmoracleproc[2879953][2879953](info)(upgradedbpipe ncModuleDepend.h:1015): file does not exist:(/anybackup/AnyBackupClient/ClientService/AggregateApp/locale/zh_CN/upgradedbpipe.mo).]]

Aug[  1 08:18:07 rs-24cmmdb-rac01 cdmoracleproc[2879953]: Unabled to add log. \Error: 访问文件 /var/log/AnyBackup/app.log 被拒。 (/var/log/AnyBackup/app.log)（错误提供者：POSIX System，错误值：13，错误位置：fileoutput.cpp:318）[] [log info: 08-01 08:18:07.388723 cdmoracleproc[2879953][2879953](info)(upgradedbscript ncModuleDepend.h:1015): file does not exist:(/anybackup/AnyBackupClient/ClientService/AggregateApp/locale/zh_CN/upgradedbscript.mo).]]

Aug[  1 08:18:07 rs-24cmmdb-rac01 cdmoracleproc[2879953]: Unabled to add log. \Error: 访问文件 /var/log/AnyBackup/app.log 被拒。 (/var/log/AnyBackup/app.log)（错误提供者：POSIX System，错误值：13，错误位置：fileoutput.cpp:318）[] [log info: 08-01 08:18:07.389836 cdmoracleproc[2879953][2879953](info)(upgradegauss ncModuleDepend.h:1015): file does not exist:(/anybackup/AnyBackupClient/ClientService/AggregateApp/locale/zh_CN/upgradegauss.mo).]]

Aug[  1 08:18:07 rs-24cmmdb-rac01 cdmoracleproc[2879953]: Unabled to add log. \Error: 访问文件 /var/log/AnyBackup/app.log 被拒。 (/var/log/AnyBackup/app.log)（错误提供者：POSIX System，错误值：13，错误位置：fileoutput.cpp:318）[] [log info: 08-01 08:18:07.390216 cdmoracleproc[2879953][2879953](info)(upgradehadoop ncModuleDepend.h:1015): file does not exist:(/anybackup/AnyBackupClient/ClientService/AggregateApp/locale/zh_CN/upgradehadoop.mo).]]

 

 

Rac2
