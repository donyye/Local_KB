[\[RHEL9\]]Environmental description

2023年11月14日

9:43

\
\
[\[root@r61 \~\]# cat /etc/hosts]

127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4

::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

10.10.40.191 r61

10.10.40.192 r62

192.168.200.191 r61-ha

192.168.200.192 r62-ha

 

 

[\[root@r61 \~\]# pcs status]

Cluster name: nss-cluster

Status of pacemakerd: \'Pacemaker is running\' (last updated 2023-11-14 09:47:14 +08:00)

Cluster Summary:

  \* Stack: corosync

  \* Current DC: r62-ha (version 2.1.4-5.el9_1.2-dc6eb4362e) - partition with quorum

  \* Last updated: Tue Nov 14 09:47:14 2023

  \* Last change:  Mon Sep 18 16:41:37 2023 by hacluster via crmd on r61-ha

  \* 2 nodes configured

  \* 7 resource instances configured

 

Node List:

  \* Online: \[ r61-ha r62-ha \]

 

Full List of Resources:

  \* Resource Group: NSSHA:

    \* nfsdata1        (ocf:heartbeat:Filesystem):         Started r62-ha

    \* NFSDaemon        (ocf:heartbeat:nfsserver):         Started r62-ha

    \* Virtualip        (ocf:heartbeat:IPaddr2):         Started r62-ha

    \* NFSNotify        (ocf:heartbeat:nfsnotify):         Started r62-ha

    \* NFSExport2        (ocf:heartbeat:exportfs):         Started r62-ha

    \* NFSExport1        (ocf:heartbeat:exportfs):         Started r62-ha

  \* vmfence        (stonith:fence_vmware_rest):         Started r62-ha

 

Daemon Status:

  corosync: active/enabled

  pacemaker: active/enabled

  pcsd: inactive/disabled

\[root@r61 \~\]#

 

 

 

已使用 OneNote 创建。
