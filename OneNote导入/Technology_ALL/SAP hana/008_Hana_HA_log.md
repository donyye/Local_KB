Hana_HA_log

2023年1月3日

9:58

系统日志收集： supportconfig 

\# 所有 node 都需要运行此命令收集，收集完成会提示日志存放路径。

 

集群日志收集：hb_report 

\# 在其中一个 node 收集就可以。

 

按照时间段收集：

hb_report -f \"2022/10/14 00:00\" -t \"2022/10/14 23:59\" /tmp/hb_report-\$(date +\"%Y%m%d-%H%M\")

 

 

<https://www.suse.com/zh-cn/support/kb/doc/?id=000017501>

 

 

 

收集日志：

s1:\~ \# hb_report 

WARNING: s1# could not figure out the log format of /var/log/pacemaker/pacemaker.log

INFO: s1# The report is saved in ./hb_report-2-02-04-2024.tar.bz2[     ]\<\-\-- 日志名字，在当前目录

INFO: s1# Report timespan: 04/01/24 21:10:00 - 04/02/24 09:10:20

INFO: s1# Thank you for taking time to create this report.

 

指定时间段收集日志：

s1:\~ \# hb_report -f \"2024/03/23 00:00\" -t \"2024/04/01 00:00\" /tmp/hb_report-\$(date +\"%Y%m%d-%H%M\")

WARNING: s1# could not figure out the log format of /var/log/pacemaker/pacemaker.log

tar: Skipping to next header

tar: Exiting with failure status due to previous errors

INFO: s1# The report is saved in /tmp/hb_report-20240402-0914.tar.bz2

INFO: s1# Report timespan: 03/23/24 00:00:00 - 04/01/24 00:00:00

INFO: s1# Thank you for taking time to create this report.

 

 

已使用 OneNote 创建。
