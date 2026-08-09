\[RHEL9\]GFS-2

2023年12月28日

15:19

 

<https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/8/html-single/configuring_and_managing_high_availability_clusters/index#assembly_configuring-gfs2-in-a-cluster-configuring-and-managing-high-availability-clusters>

 

FYI:

<https://www.server-world.info/en/note?os=CentOS_Stream_9&p=pacemaker&f=7>

 

\# subscription-manager repos \--enable=rhel-9-for-x86_64-resilientstorage-rpms

 

[\[root@r61 \~\]# dnf install lvm2-lockd gfs2-utils dlm]

\[root@r62[ \~\]# dnf install lvm2-lockd gfs2-utils dlm]

 

[\[root@r61 \~\]# vim /etc/lvm/lvm.conf]

use_lvmlockd = 1

 

\[root@r62[ \~\]# vim /etc/lvm/lvm.conf]

use_lvmlockd = 1

 

 

[\[root@r61 \~\]# pcs property set no-quorum-policy=freeze]

 

[\[root@r61 \~\]# pcs resource create dlm \--group locking ocf:pacemaker:controld op monitor interval=30s on-fail=fence]

 

[\[root@r61 \~\]# pcs resource clone locking interleave=true]

 

[\[root@r61 \~\]# pcs resource create lvmlockd \--group locking ocf:heartbeat:lvmlockd op monitor interval=30s on-fail=fence]

 

[\[root@r61 \~\]# pcs status \--full]

Cluster name: nss-cluster

Status of pacemakerd: \'Pacemaker is running\' (last updated 2023-12-28 15:23:30 +08:00)

Cluster Summary:

  \* Stack: corosync

  \* Current DC: r61-ha (1) (version 2.1.4-5.el9_1.2-dc6eb4362e) - partition with quorum

  \* Last updated: Thu Dec 28 15:23:30 2023

  \* Last change:  Thu Dec 28 15:23:09 2023 by root via cibadmin on r61-ha

  \* 2 nodes configured

  \* 11 resource instances configured

 

Node List:

  \* Online: \[ r61-ha (1) r62-ha (2) \]

 

Full List of Resources:

  \* Resource Group: NSSHA:

    \* nfsdata1        (ocf:heartbeat:Filesystem):         Started r61-ha

    \* NFSDaemon        (ocf:heartbeat:nfsserver):         Started r61-ha

    \* Virtualip        (ocf:heartbeat:IPaddr2):         Started r61-ha

    \* NFSNotify        (ocf:heartbeat:nfsnotify):         Started r61-ha

    \* NFSExport2        (ocf:heartbeat:exportfs):         Started r61-ha

    \* NFSExport1        (ocf:heartbeat:exportfs):         Started r61-ha

  \* vmfence        (stonith:fence_vmware_rest):         Started r61-ha

  \* Clone Set: locking-clone \[locking\]:

    \* Resource Group: locking:0:

      \* dlm        (ocf:pacemaker:controld):         Started r61-ha

      \* lvmlockd        (ocf:heartbeat:lvmlockd):         Started r61-ha

    \* Resource Group: locking:1:

[      \* dlm        (ocf:pacemaker:controld):         Started r62-ha]

[      \* lvmlockd        (ocf:heartbeat:lvmlockd):         Started r62-ha]

 

Migration Summary:

 

Failed Fencing Actions:

  \* reboot of r62-ha failed: delegate=, client=pacemaker-controld.1237, origin=r61-ha, completed=\'2023-12-26 17:23:03 +08:00\'

 

Tickets:

 

PCSD Status:

  r61-ha: Online

  r62-ha: Online

 

Daemon Status:

  corosync: active/enabled

  pacemaker: active/enabled

  pcsd: active/enabled

\[root@r61 \~\]#

 

 

 

 

已使用 OneNote 创建。
