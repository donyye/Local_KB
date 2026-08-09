\[RHEL8\]fences-SBD

2024年5月16日

16:11

适用于 RHEL9

======SBD======

\[root@rh88 \~\]# sbd -d /dev/sdb1 -4 20 -1 10 create

Initializing device /dev/sdb1

Creating version 2.1 header on device 3 (uuid: adfd7fe3-dd58-48b8-9cc3-ab222639a811)

Initializing 255 slots on device 3

Device /dev/sdb1 is initialized.

Did you check sbd service down on all nodes before? If not do so now and restart afterwards.

 

创建后正常两个node都能看到

\[root@rh88 \~\]# sbd -d /dev/sdb1 dump

==Dumping header on disk /dev/sdb1

Header version     : 2.1

UUID               : adfd7fe3-dd58-48b8-9cc3-ab222639a811

Number of slots    : 255

Sector size        : 512

Timeout (watchdog) : 10

Timeout (allocate) : 2

Timeout (loop)     : 1

Timeout (msgwait)  : 20

==Header on disk /dev/sdb1 is dumped

 

两个节点都确保 softdog 被加载

\[root@rh88 \~\]# modprobe softdog

\[root@rh88 \~\]# lsmod \|grep softdog

softdog                16384  0

 

两节点都要

\[root@rh88 \~\]# echo \'softdog\' \> /etc/modules-load.d/softdog.conf

 

两节点都要

修改sbd配置文件 "/etc/sysconfig/sbd"

[\[root@rh88 \~\]# grep -Ev \'\^\\s\*#\|\^\\s\*\$\' /etc/sysconfig/sbd]

SBD_DEVICE=\"/dev/sdb1\"[   \--\> ]修改成 sbd 的盘

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

[\[root@rh88 \~\]# systemctl enable sbd]

Created symlink /etc/systemd/system/corosync.service.requires/sbd.service → /usr/lib/systemd/system/sbd.service.

Created symlink /etc/systemd/system/pacemaker.service.requires/sbd.service → /usr/lib/systemd/system/sbd.service.

Created symlink /etc/systemd/system/dlm.service.requires/sbd.service → /usr/lib/systemd/system/sbd.service.

Unit /usr/lib/systemd/system/sbd.service is added as a dependency to a non-existent unit dlm.service.

\[root@rh88 \~\]#

 

完成后重启一下集群服务

\[root@rh88 \~\]# pcs cluster stop \--all

rh88.ha.com: Stopping Cluster (pacemaker)\...

rh89.ha.com: Stopping Cluster (pacemaker)\...

rh89.ha.com: Stopping Cluster (corosync)\...

rh88.ha.com: Stopping Cluster (corosync)\...

 

\[root@rh88 \~\]# pcs cluster start \--all

rh88.ha.com: Starting Cluster\...

rh89.ha.com: Starting Cluster\...

 

 

![[RHEL Cluster Test_012_[RHEL8]fences-SBD_001.png]]

 

 

![[RHEL Cluster Test_012_[RHEL8]fences-SBD_002.png]]

 

 

[\[root@rh88 \~\]# sbd -d /dev/sdb1 list]

0        rh89.ha.com        clear        

1        rh88.ha.com        clear        

 

[\[root@rh88 \~\]# fence_sbd \--devices=/dev/sdb1 -n rh89.ha.com -o status]

2024-04-02 13:32:46,991 WARNING: power timeout needs to be                 

greater then sbd message timeout

Status: ON

 

[\[root@rh88 \~\]# fence_sbd \--devices=/dev/sdb1 -n rh89.ha.com -o reboot]

2024-04-02 13:33:44,646 WARNING: power timeout needs to be                 

greater then sbd message timeout

2024-04-02 13:34:04,837 ERROR: Connection timed out

 

 

创建 SBD fence

[\[root@rh88 \~\]# pcs stonith create fence-sbd fence_sbd devices=\"/dev/sdb1\" power_timeout=20]

![[RHEL Cluster Test_012_[RHEL8]fences-SBD_003.png]]

 

 

[\[root@rh88 \~\]# pcs stonith fence rh89.ha.com]

Node: rh89.ha.com fenced [   \<\-- ]成功被fence

 

[\[root@rh88 \~\]# pcs stonith history ]

reboot of rh89.ha.com successful: delegate=rh88.ha.com, client=stonith_admin.2492282, origin=rh88.ha.com, completed=\'2024-04-02 13:40:35 +08:00\'

reboot of rh89.ha.com failed: delegate=, client=stonith_admin.2490095, origin=rh88.ha.com, completed=\'2024-04-02 13:36:20 +08:00\' (a later attempt succeeded)

2 events found

 

 

清除fence记录

[\[root@rh88 \~\]# pcs stonith history cleanup ]

cleaning up fencing-history for node \*

 

 

已使用 OneNote 创建。
