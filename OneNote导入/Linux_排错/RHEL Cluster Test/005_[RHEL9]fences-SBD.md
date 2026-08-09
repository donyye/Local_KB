[\[RHEL9\]fences-]SBD

2024年5月16日

16:31

 

\[root@node1[ \~\]#] sbd -d /dev/disk/by-id/scsi-SLIO-ORG_disk1_9bd1538c-0b5a-4707-819e-1f5ff9828945 -4 20 -1 10 create

 

[\[root@node2 \~\]# sbd -d /dev/disk/by-id/scsi-SLIO-ORG_disk1_9bd1538c-0b5a-4707-819e-1f5ff9828945 -4 20 -1 10 create]

 

[\[root@node1 \~\]# sbd -d /dev/disk/by-id/scsi-SLIO-ORG_disk1_9bd1538c-0b5a-4707-819e-1f5ff9828945 dump]

==Dumping header on disk /dev/disk/by-id/scsi-SLIO-ORG_disk1_9bd1538c-0b5a-4707-819e-1f5ff9828945

Header version     : 2.1

UUID               : f0ba7909-8731-46ba-b277-12cd6c65e3c7

Number of slots    : 255

Sector size        : 512

Timeout (watchdog) : 10

Timeout (allocate) : 2

Timeout (loop)     : 1

Timeout (msgwait)  : 20

==Header on disk /dev/disk/by-id/scsi-SLIO-ORG_disk1_9bd1538c-0b5a-4707-819e-1f5ff9828945 is dumped

 

 

两个节点都确保 softdog 被加载

[\[root@node1 \~\]# modprobe softdog]

[\[root@node1 \~\]# lsmod \|grep softdog]

softdog                16384  0

 

两节点都要

[\[root@node1 \~\]#  echo \'softdog\' \> /etc/modules-load.d/softdog.conf]

 

 

两节点都要

修改sbd配置文件 "/etc/sysconfig/sbd"

[\[root@node1 \~\]# grep -Ev \'\^\\s\*#\|\^\\s\*\$\' /etc/sysconfig/sbd]

SBD_DEVICE=\"/dev/disk/by-id/scsi-SLIO-ORG_disk1_9bd1538c-0b5a-4707-819e-1f5ff9828945\"

[  \--\> ]修改成 做sbd 的盘 的by-id 地址

SBD_PACEMAKER=yes

SBD_STARTMODE=always

SBD_DELAY_START=no

SBD_WATCHDOG_DEV=/dev/watchdog

SBD_WATCHDOG_TIMEOUT=5

SBD_TIMEOUT_ACTION=flush,reboot

SBD_MOVE_TO_ROOT_CGROUP=auto

SBD_SYNC_RESOURCE_STARTUP=yes

SBD_OPTS=\"-W\"[       \--\> ]改成这样

\# 其它不变

 

两节点都要

[\[root@node1 \~\]# systemctl enable sbd]

Created symlink /etc/systemd/system/corosync.service.requires/sbd.service → /usr/lib/systemd/system/sbd.service.

Created symlink /etc/systemd/system/pacemaker.service.requires/sbd.service → /usr/lib/systemd/system/sbd.service.

Created symlink /etc/systemd/system/dlm.service.requires/sbd.service → /usr/lib/systemd/system/sbd.service.

Unit /usr/lib/systemd/system/sbd.service is added as a dependency to a non-existent unit dlm.service.

 

 

完成后重启一下集群服务

[\[root@node1 \~\]#  pcs cluster stop \--all]

rh88.ha.com: Stopping Cluster (pacemaker)\...

rh89.ha.com: Stopping Cluster (pacemaker)\...

rh89.ha.com: Stopping Cluster (corosync)\...

rh88.ha.com: Stopping Cluster (corosync)\...

 

[\[root@node1 \~\]#  pcs cluster start \--all]

rh88.ha.com: Starting Cluster\...

rh89.ha.com: Starting Cluster\...

 

![[RHEL Cluster Test_005_[RHEL9]fences-SBD_001.png]]

 

 

![[RHEL Cluster Test_005_[RHEL9]fences-SBD_002.png]]

 

 

[\[root@node1 \~\]# sbd -d /dev/disk/by-id/scsi-SLIO-ORG_disk1_9bd1538c-0b5a-4707-819e-1f5ff9828945 list]

0        localhost.localdomain        clear        

1        localhost.localdomain        clear

 

 

[\[root@node1 \~\]# pcs stonith create fence-sbd fence_sbd devices=\"/dev/disk/by-id/scsi-SLIO-ORG_disk1_9bd1538c-0b5a-4707-819e-1f5ff9828945\" power_timeout=20]

![[RHEL Cluster Test_005_[RHEL9]fences-SBD_003.png]]

 

 

[\[root@node1 \~\]# pcs stonith fence node2.abc.com]

Node: node2.abc.com fenced[   ][ \<\-- ]成功被fence

 

![[RHEL Cluster Test_005_[RHEL9]fences-SBD_004.png]]

 

清除fence记录

[\[root@node1 \~\]# pcs stonith history cleanup ]

cleaning up fencing-history for node \*

 

 

已使用 OneNote 创建。
