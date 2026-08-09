RHCS

Thursday, April 24, 2014

1:45 PM

 

+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 故障陈述：\                                                                                                                                                                                                                                                                                                                                                                                                              |
| 2014-4-24（14:04:50）Cluster 里的node1被qdisk fence。\                                                                                                                                                                                                     |
| fence 时间发生在 Apr 21 14:04:50，是根据node2 qdisk日志找到的信息。 |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

node1:

qdisk.log

Apr 20 03:24:03 zjkf_kmsapp_1 kernel: imklog 4.6.2, log source = /proc/kmsg started.

Apr 20 03:24:03 zjkf_kmsapp_1 rsyslogd: \[origin software=\"rsyslogd\" swVersion=\"4.6.2\" x-pid=\"76319\" x-info=\"http://www.rsyslog.com\"\] (re)start

Apr 20 03:24:04 zjkf_kmsapp_1 rhsmd: This system is missing one or more valid entitlement certificates. Please run subscription-manager for more information.

Apr 21 03:21:02 zjkf_kmsapp_1 rhsmd: This system is missing one or more valid entitlement certificates. Please run subscription-manager for more information.

Apr 21 14:11:16 zjkf_kmsapp_1 kernel: imklog 4.6.2, log source = /proc/kmsg started.

Apr 21 14:11:16 zjkf_kmsapp_1 rsyslogd: \[origin software=\"rsyslogd\" swVersion=\"4.6.2\" x-pid=\"7689\" x-info=\"http://www.rsyslog.com\"\] (re)start

Apr 21 14:11:16 zjkf_kmsapp_1 kernel: Initializing cgroup subsys cpuset

Apr 21 14:11:16 zjkf_kmsapp_1 kernel: Initializing cgroup subsys cpu

Apr 21 14:11:16 zjkf_kmsapp_1 kernel: Linux version 2.6.32-220.4.2.el6.x86_64 (mockbuild@x86-003.build.bos.redhat.com) (gcc version 4.4.6 20110731 (Red Hat 4.4.6-3) (GCC) ) #1 SMP Mon Feb 6 16:39:28 EST 2012

#node1上没有看到被node2 fence 记录，莫名重启了。

 

node2:

qdisk.log

Apr 19 01:12:29 qdiskd qdisk cycle took more than 1 second to complete (1.830000)

Apr 21 14:04:49 qdiskd Assuming master role

Apr 21 14:04:49 qdiskd Writing eviction notice for node 1

Apr 21 14:04:50 qdiskd Node 1 evicted

 

message.log

Apr 20 03:25:05 zjkf_kmsapp_2 rhsmd: This system is missing one or more valid entitlement certificates. Please run subscription-manager for more information.

Apr 21 03:12:03 zjkf_kmsapp_2 rhsmd: This system is missing one or more valid entitlement certificates. Please run subscription-manager for more information.

Apr 21 13:07:01 zjkf_kmsapp_2 auditd\[7374\]: Audit daemon rotating log files

Apr 21 14:04:53 zjkf_kmsapp_2 kernel: dlm: closing connection to node 1

Apr 21 14:04:53 zjkf_kmsapp_2 kernel: GFS2: fsid=pkg_kmsapp:kmsfile.0: jid=1: Trying to acquire journal lock\...

Apr 21 14:04:53 zjkf_kmsapp_2 kernel: GFS2: fsid=pkg_kmsapp:kmsindex.0: jid=1: Trying to acquire journal lock\...

Apr 21 14:05:08 zjkf_kmsapp_2 kernel: GFS2: fsid=pkg_kmsapp:kmsfile.0: jid=1: Looking at journal\...

Apr 21 14:05:08 zjkf_kmsapp_2 kernel: GFS2: fsid=pkg_kmsapp:kmsfile.0: jid=1: Acquiring the transaction lock\...

Apr 21 14:05:08 zjkf_kmsapp_2 kernel: GFS2: fsid=pkg_kmsapp:kmsfile.0: jid=1: Replaying journal\...

Apr 21 14:05:08 zjkf_kmsapp_2 kernel: GFS2: fsid=pkg_kmsapp:kmsfile.0: jid=1: Replayed 1903 of 1990 blocks

Apr 21 14:05:08 zjkf_kmsapp_2 kernel: GFS2: fsid=pkg_kmsapp:kmsfile.0: jid=1: Found 45 revoke tags

Apr 21 14:05:08 zjkf_kmsapp_2 kernel: GFS2: fsid=pkg_kmsapp:kmsfile.0: jid=1: Journal replayed in 1s

Apr 21 14:05:08 zjkf_kmsapp_2 kernel: GFS2: fsid=pkg_kmsapp:kmsfile.0: jid=1: Done

 

 

 

 

+---------------------------------------------------------------------------------------------------------------------------------+
| cluster.conf                                                                                        |
|                                                                                                                                 |
| \<?xml version=\"1.0\"?\>                                                                                                       |
|                                                                                                                                 |
| \<cluster config_version=\"77\" name=\"pkg_kmsapp\"\>                                                                           |
|                                                                                                                                 |
| \<clusternodes\>                                                                                                                |
|                                                                                                                                 |
| \<clusternode name=\"zjkf_kmsapp_1\" nodeid=\"1\"\>                                                                             |
|                                                                                                                                 |
| \<fence\>                                                                                                                       |
|                                                                                                                                 |
| \<method name=\"Method1\"\>                                                                                                     |
|                                                                                                                                 |
| \<device name=\"fence1\"/\>                                                                                                     |
|                                                                                                                                 |
| \</method\>                                                                                                                     |
|                                                                                                                                 |
| \</fence\>                                                                                                                      |
|                                                                                                                                 |
| \</clusternode\>                                                                                                                |
|                                                                                                                                 |
| \<clusternode name=\"zjkf_kmsapp_2\" nodeid=\"2\"\>                                                                             |
|                                                                                                                                 |
| \<fence\>                                                                                                                       |
|                                                                                                                                 |
| \<method name=\"Method2\"\>                                                                                                     |
|                                                                                                                                 |
| \<device name=\"fence2\"/\>                                                                                                     |
|                                                                                                                                 |
| \</method\>                                                                                                                     |
|                                                                                                                                 |
| \</fence\>                                                                                                                      |
|                                                                                                                                 |
| \</clusternode\>                                                                                                                |
|                                                                                                                                 |
| \</clusternodes\>                                                                                                               |
|                                                                                                                                 |
| \<cman expected_votes=\"3\"/\>                                                                                                  |
|                                                                                                                                 |
| \<fencedevices\>                                                                                                                |
|                                                                                                                                 |
| \<fencedevice agent=\"fence_ipmilan\" auth=\"md5\" ipaddr=\"192.168.3.105\" login=\"root\" name=\"fence1\" passwd=\"\*\*\*\"/\> |
|                                                                                                                                 |
| \<fencedevice agent=\"fence_ipmilan\" auth=\"md5\" ipaddr=\"192.168.3.106\" login=\"root\" name=\"fence2\" passwd=\"\*\*\*\"/\> |
|                                                                                                                                 |
| \</fencedevices\>                                                                                                               |
|                                                                                                                                 |
| \<rm\>                                                                                                                          |
|                                                                                                                                 |
| \<failoverdomains\>                                                                                                             |
|                                                                                                                                 |
| \<failoverdomain name=\"kmsapp_fd\" nofailback=\"0\" ordered=\"0\" restricted=\"0\"\>                                           |
|                                                                                                                                 |
| \<failoverdomainnode name=\"zjkf_kmsapp_1\"/\>                                                                                  |
|                                                                                                                                 |
| \<failoverdomainnode name=\"zjkf_kmsapp_2\"/\>                                                                                  |
|                                                                                                                                 |
| \</failoverdomain\>                                                                                                             |
|                                                                                                                                 |
| \</failoverdomains\>                                                                                                            |
|                                                                                                                                 |
| \</rm\>                                                                                                                         |
|                                                                                                                                 |
| \<logging to_syslog=\"no\"\>                                                                                                    |
|                                                                                                                                 |
| \<logging_daemon debug=\"on\" name=\"rgmanager\"/\>                                                                             |
|                                                                                                                                 |
| \</logging\>                                                                                                                    |
|                                                                                                                                 |
| \<quorumd device=\"/dev/mapper/qdisk\"/\>                                                                                       |
|                                                                                                                                 |
| \</cluster\>                                                                                                                    |
+---------------------------------------------------------------------------------------------------------------------------------+

 

+------------------------------------------------------------------------------------------------------------------------------------------+
| 集群运作状态：\                                                                                                                   |
| Cluster Status for pkg_kmsapp @ Mon Apr 21 18:21:38 2014                                                   |
|                                                                                                                                          |
| Member Status: Quorate                                                                                                                   |
|                                                                                                                                          |
|                                                                                                                                          |
|                                                                                                                                          |
|  Member Name                             ID   Status |
|                                                                                                                                          |
|  \-\-\-\-\-- \-\-\--                             \-\-\-- \-\-\-\-\--             |
|                                                                                                                                          |
|  zjkf_kmsapp_1                               1 Online                            |
|                                                                                                                                          |
|  zjkf_kmsapp_2                               2 Online, Local                     |
|                                                                                                                                          |
|  /dev/mapper/qdisk                           0 Online, Quorum Disk               |
+------------------------------------------------------------------------------------------------------------------------------------------+

 

+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| cman_tool_nodes：\                                                                                                                                                                               |
| Node[  Sts   Inc   Joined               Name]            |
|                                                                                                                                                                                                      |
|    0   M      0   2014-03-19 00:30:40  /dev/mapper/qdisk |
|                                                                                                                                                                                                      |
|    1   M    280   2014-04-21 14:11:19  zjkf_kmsapp_1     |
|                                                                                                                                                                                                      |
|    2   M    268   2014-03-19 00:30:32  zjkf_kmsapp_2     |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

 

  ---
   
  ---

 

已使用 OneNote 创建。
