SUSE HA 心跳问题

2022年2月10日

9:16

案例 ST: GFWMTJ3 \
==================\
在查看日志之后，我们看到这两个服务器多次重新启动。这些都是心跳通信丢失造成的。

\# node1和node2不能同时看到对方。

2022-02-07T08:06:09.133275+08:00 sapprdhn01 [corosync\[3049\]:   \[TOTEM \] ]A processor failed, forming new configuration.

2022-02-07T08:06:09.131864+08:00 sapprdhn02 [corosync\[3019\]:   \[TOTEM \] ]A processor failed, forming new configuration.

 

\# node1检查SAP HANA复制状态失败。也可能是网络故障。

[2022-02-07T08:06:29.373844+08:00 sapprdhn01 SAPHana(rsc_SAPHana_PRD_HDB01)\[67749\]: ]WARNING: HANA_CALL timed out after 5 seconds running command \'systemReplicationStatus.py \--site=PRDSITE2\'

[2022-02-07T08:06:29.378304+08:00 sapprdhn01 SAPHana(rsc_SAPHana_PRD_HDB01)\[67749\]: INFO: DEC analyze_hana_sync_statusSRS systemReplicationStatus.py (to site \'PRDSITE2\')-\> 124]

\--\> suse的HA调用sap的command \'systemReplicationStatus.py \--site=PRDSITE2\' 而这个命令是sap的，用来检查当前SAP应用的replication状态，这个也报错，说明这个脚本也连接不到对面。它走网络要看配置。

\--\> 上述两个说明，心跳同时断开，而SAP hana同步也断开了，很有可能是网络出现了问题。

 

\# 但不幸的是，node1已经向IPMI发送了fencing命令以重新启动node2。

[2022-02-07T08:18:08.717610+08:00 sapprdhn01 corosync\[3083\]:   \[TOTEM \] ]A processor failed, forming new configuration.

[2022-02-07T08:18:14.719750+08:00 sapprdhn01 corosync\[3083\]:   \[TOTEM \] A new membership (10.10.10.126:17036) was formed. Members left: 2]

[2022-02-07T08:18:14.719922+08:00 sapprdhn01 corosync\[3083\]:   \[TOTEM \] ]Failed to receive the leave message. failed: 2

[2022-02-07T08:18:14.720011+08:00 sapprdhn01 corosync\[3083\]:   ][\[QUORUM\]][ Members\[1\]: 1]

 

[2022-02-07T08:18:08.379407+08:00 sapprdhn02 corosync\[3093\]:   \[TOTEM \] ]A processor failed, forming new configuration.

[2022-02-07T08:18:14.380886+08:00 sapprdhn02 corosync\[3093\]:   \[TOTEM \] A new membership (10.10.10.127:17036) was formed. Members left: 1]

[2022-02-07T08:18:14.381107+08:00 sapprdhn02 corosync\[3093\]:   \[TOTEM \] ]Failed to receive the leave message. failed: 1

[2022-02-07T08:18:14.381305+08:00 sapprdhn02 cib\[3353\]:   notice: Node sapprdhn01 ]state is now lost

[2022-02-07T08:18:14.381413+08:00 sapprdhn02 corosync\[3093\]:   ][\[QUORUM\]][ Members\[1\]: 2]

 

\# node2重启node1成功

[2022-02-07T08:18:14.457245+08:00 sapprdhn02 stonith-ng\[3354\]:   notice: ]Requesting peer fencing (reboot) targeting sapprdhn01

[2022-02-07T08:18:35.783128+08:00 sapprdhn02 stonith-ng\[3354\]:   notice: ]Operation \'reboot\'[ \[8651\] (call 2 from crmd.3358) for host \'sapprdhn01\' with device \'sapprdhn01-ipmilan\' returned: 0 (OK)]

 

服务器设置了 2 个用于心跳的环，每个环都使用 LACP 绑定。 我们没有从服务器端看到任何网络故障。 建议在此期间检查网络。 网络并没有完全关闭。 它只是在几秒钟（\> 30 秒）左右无法通信并恢复正常。 但是集群在没有心跳的情况下失败，超时时间如上所述。 

 

 

![Machine generated alternative text: e22-ø2-e7Te8: : 15.135B61+e8 e22-e2-e7Te8:e€ : 15.135158+e8 e22-ø2-e7Te8: : 15.135321+e8 sapprdhnel sapprdhnel sapprdhn01 sapprdhnel \[TOTEM \] Failed to receive corosync\[3e49\] : notice: Lost attribute writer attrd \[3277\] : corosync\[3Ø49\] : \[QUORUM\] •embers corosync\[3049\] : \[MAIN \] Completed service the leave message. failed. sapprdhne2 synchronization, ready to provide service. ](attachments/Technology_ALL_SUSE_analyze_017_SUSE%20HA%20心跳问题_001.png)

 

![Machine generated alternative text: 2e22-e2-e7Te8: es : 33.63e848+e8 : 35.587223+e8 2e22-ø2-e7Te8:e6 : 35.592703+e8 2e22-ø2-e7Te8: 06 : 35.592784+e8 sapprdhnel sapprdhnel sapprdhnel sapprdhnel corosync\[3049\] : corosync\[3e49\] : corosync\[3e49\] \[68697\]: INFO: RA - - end action monitor clone with \[TOTEM \] A new membership (10.1B.10.126 :17016) Was formed. joined: 2 \*embers \[QUORUM\] •nembers (MAIN \] Completed service synchronization, ready to provide sen\'ice. ](attachments/Technology_ALL_SUSE_analyze_017_SUSE%20HA%20心跳问题_002.png)

 

 

 

 

案例：ST: 7P8LTC3

==============

 

根据日志分析，我们可以看到prd02在特定时间未能连接到prd01的iDRAC。 集群注意到故障后，集群重新启动资源"rsc_ess4prd01_stonith"并成功。 

因此它将在集群状态中计为 1 个故障。 

需要使用命令"crm resource refresh rsc_ess4prd01_stonith ess4prd02"手动清理记录

 

[Jan 11 17:29:06 ess4prd02 external/ipmi(rsc_ess4prd01_stonith)\[58316\]: ]ERROR: error executing ipmitool: Error: Unable to establish IPMI v2 / RMCP+ session

[Jan 11 17:29:07 ess4prd02 stonith\[56369\]: external_status: \'ipmi status\' ]failed with rc 1

[Jan 11 17:29:07 ess4prd02 stonith\[56369\]: external/ipmi device not accessible.]

[Jan 11 17:29:18 ess4prd02 external/ipmi(rsc_ess4prd01_stonith)\[58946\]: ]ERROR: error executing ipmitool: Error: Unable to establish IPMI v2 / RMCP+ session

[Jan 11 17:29:19 ess4prd02 stonith\[58629\]: external_status: \'ipmi status\' ]failed with rc 1

[Jan 11 17:29:19 ess4prd02 stonith\[58629\]: external/ipmi device not accessible.]

[Jan 11 17:29:19 ess4prd02 pacemaker-attrd\[18384\]:  notice: Setting ][fail-count-rsc_ess4prd01_stonith#monitor_300000\[ess4prd02\]:] (unset) -\> 1

[Jan 11 17:29:19 ess4prd02 pacemaker-attrd\[18384\]:  notice: Setting last-failure-rsc_ess4prd01_stonith#monitor_300000\[ess4prd02\]: (unset) -\> 1673429359]

[Jan 11 17:29:19 ess4prd02 hacluster\[59070\]: PCMK-NOTIFY: hacluster: Resource operation \'monitor (300000)\' for \'rsc_ess4prd01_stonith\' on \'ess4prd02\': ]error

 

[Jan 11 17:29:19 ess4prd02 pacemaker-controld\[18386\]:  ]notice: Result of stop operation for rsc_ess4prd01_stonith on ess4prd02: ok

[Jan 11 17:29:20 ess4prd02 pacemaker-controld\[18386\]:  ]notice: Result of start operation for rsc_ess4prd01_stonith on ess4prd02: ok

[Jan 11 17:29:23 ess4prd02 pacemaker-controld\[18386\]:  ]notice: Result of monitor operation for rsc_ess4prd01_stonith on ess4prd02: ok

 

我们尝试检查 prd01 的 TSR 日志以查看 iDRAC 连接失败时发生了什么。 但不幸的是，1 月 11 日的日志被刷新了。 有多种可能的原因，例如 iDRAC 在那一刻被重置/重新启动或网络无法访问等。但是没有用于进一步隔离的 TSR 日志。 如果再次发生，请尽快抓取两个节点的supportconfig log和TSR log。

 

\-\-\-\--

Idrac 心跳设置默认是60s, 不建议修改加长，最佳实践就是用默认的就已经足够长了，再加会导致failover过慢等其他问题。

对于这个cluster的，长时间只发生了一次，且cluster自动修复的就没问题，不会有生产影响。如果经常发生，那就要检查是idrac还是网络有问题了，而不是去增加timeout

ha.txt (查看当前心跳时长)

\# /usr/sbin/crm configure show

......

primitive rsc_ess4prd01_stonith stonith:external/ipmi \\

        op monitor interval=300s timeout=60s on-fail=restart \\

        op start interval=0 timeout=60s \\

......

 

 

 

 

已使用 OneNote 创建。
