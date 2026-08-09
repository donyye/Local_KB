\[RHEL8\]GFS-2

2024年2月19日

11:20

FYI:\
[https://www.server-world.info/en/note?os=CentOS_Stream_8&p=pacemaker&f=7](https://www.server-world.info/en/note?os=CentOS_Stream_8&p=pacemaker&f=7)

\
\[root@ha01 \~\]# yum install lvm2-lockd gfs2-utils dlm

 

\[root@ha01[ \~\]# vim /etc/lvm/lvm.conf]

use_lvmlockd = 1

 

\[root@ha02[ \~\]# vim /etc/lvm/lvm.conf]

use_lvmlockd = 1

 

[\# set \[no-quorum-policy=freeze\] on GFS2  -\> ]设置仲裁法定数规则

[\[root@ha01 \~\]# pcs property set no-quorum-policy=freeze]

\# 此配置的意思是当失去法定仲裁，Pacemaker 将冻结所有资源组并停止在集群中进行任何切换。这是出于安全性考虑，以防止节点之间发生不同步的情况。

 

\# create controld resource[   -\> ]创建一个控制资源

[\# \[dlm\] ]⇒ any name you like

[\# \[\--group\] ]⇒ any group name

[\[root@ha01 \~\]# pcs resource create dlm \--group locking ocf:pacemaker:controld op monitor interval=30s on-fail=fence]

\# 这个 PCS 命令用于创建一个 DLM（分布式锁管理器）资源，并将其归入 locking 组中。这个资源将使用 Pacemaker 提供的 OCF（Open Cluster Framework）脚本 ocf:pacemaker:controld 进行配置。

\# op monitor interval=30s on-fail=fence： 配置资源监控操作。它会定期（每 30 秒）检查资源的状态，如果监控失败，则执行 fence 操作

 

\# create clone of \[locking\] to activate it on all nodes in cluster

创建[ \[locking\] ]的克隆以在集群中的所有节点上激活它

[\[root@ha01 \~\]# pcs resource clone locking interleave=true]

 

\# create lvmlockd resource -\> 创建lvmlockd资源

[\# \[lvmlockdd\] ]⇒ any name

[\# \[\--group\] ]⇒ the same group with controld resource

[\[root@ha01 \~\]# pcs resource create lvmlockd \--group locking ocf:heartbeat:lvmlockd op monitor interval=30s on-fail=fence]

\#  LVM Lock Manager Daemon（lvmlockd）资源，并将其归入 locking 组中。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

上面几步，主要是配置用于共享存储的锁定服务

 

\# verify status

[\[root@ha01 \~\]# pcs status \--full]

Cluster name: MyHR89C

Cluster Summary:

  \* Stack: corosync (Pacemaker is running)

  \* Current DC: ha02.ddcab.com (2) (version 2.1.6-9.1.el8_9-6fdc9deea29) - partition with quorum

  \* Last updated: Mon Feb 19 17:04:53 2024 on ha01.ddcab.com

  \* Last change:  Mon Feb 19 17:04:43 2024 by root via cibadmin on ha01.ddcab.com

  \* 2 nodes configured

  \* 4 resource instances configured

 

Node List:

  \* Node ha01.ddcab.com (1): online, feature set 3.17.4

  \* Node ha02.ddcab.com (2): online, feature set 3.17.4

 

Full List of Resources:

  \* Clone Set: locking-clone \[locking\]:

    \* Resource Group: locking:0:

      \* dlm        (ocf::pacemaker:controld):         Stopped

      \* lvmlockd        (ocf::heartbeat:lvmlockd):         Stopped

    \* Resource Group: locking:1:

      \* dlm        (ocf::pacemaker:controld):         Stopped

      \* lvmlockd        (ocf::heartbeat:lvmlockd):         Stopped

 

Migration Summary:

  \* Node: ha01.ddcab.com (1):

    \* dlm: migration-threshold=1000000 fail-count=1000000 last-failure=\'Mon Feb 19 17:04:13 2024\'

 

Failed Resource Actions:

  \* dlm_start_0 on ha01.ddcab.com \'not configured\' (6): call=6, status=\'complete\', last-rc-change=\'Mon Feb 19 17:04:13 2024\', queued=0ms, exec=53ms

 

Tickets:

 

PCSD Status:

  ha01.ddcab.com: Online

  ha02.ddcab.com: Online

 

Daemon Status:

  corosync: active/enabled

  pacemaker: active/enabled

  pcsd: active/enabled

 

不用设置

\# systemctl list-unit-files \|grep dlm

\[root@ha01[ \~\]#] systemctl start dlm.service 

\[root@ha01[ \~\]#] systemctl status lvmlockd.service

 

\# OK if all \[Started\]

[\[root@ha01 \~\]# pcs status \--full]

Cluster name: MyHR89C

Cluster Summary:

  \* Stack: corosync (Pacemaker is running)

  \* Current DC: ha02.ddcab.com (2) (version 2.1.6-9.1.el8_9-6fdc9deea29) - partition with quorum

  \* Last updated: Tue Feb 20 08:56:35 2024 on ha01.ddcab.com

  \* Last change:  Tue Feb 20 08:33:39 2024 by root via cibadmin on ha01.ddcab.com

  \* 2 nodes configured

  \* 5 resource instances configured

 

Node List:

  \* Node ha01.ddcab.com (1): online, feature set 3.17.4

  \* Node ha02.ddcab.com (2): online, feature set 3.17.4

 

Full List of Resources:

  \* Clone Set: locking-clone \[locking\]:

    \* Resource Group: locking:0:

      \* dlm        (ocf::pacemaker:controld):         Started ha01.ddcab.com

      \* lvmlockd        (ocf::heartbeat:lvmlockd):         Started ha01.ddcab.com

    \* Resource Group: locking:1:

      \* dlm        (ocf::pacemaker:controld):         Started ha02.ddcab.com

      \* lvmlockd        (ocf::heartbeat:lvmlockd):         Started ha02.ddcab.com

  \* vmfence        (stonith:fence_vmware_rest):         Started ha02.ddcab.com

 

Migration Summary:

 

Failed Fencing Actions:

  \* reboot of ha02.ddcab.com failed: client=pacemaker-controld.43106, origin=ha01.ddcab.com, completed=\'2024-02-20 08:36:07.530926 +08:00\'

 

Tickets:

 

PCSD Status:

  ha01.ddcab.com: Online

  ha02.ddcab.com: Online

 

Daemon Status:

  corosync: active/enabled

  pacemaker: active/enabled

  pcsd: active/enabled

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\# set LVM

[\[root@ha01 \~\]# parted \--script /dev/sdd \"mklabel gpt\"]

[\[root@ha01 \~\]# parted \--script /dev/sdd \"mkpart primary 0% 100%\" ]

[\[root@ha01 \~\]# parted \--script /dev/sdd \"set 1 lvm on\"]

 

\# create physical volume

[\[root@ha01 \~\]# pvcreate /dev/sdd1 ]

  Physical volume \"/dev/sdd1\" successfully created.

 

\# create chared volume

[\[root@ha01 \~\]# vgcreate \--shared vg_gfs2 /dev/sdd1 ]

  Volume group \"vg_gfs2\" successfully created

  VG vg_gfs2 starting dlm lockspace

  Starting locking.  Waiting until locks are ready\...

\-\-\-\-\-\-\-\--node2\-\-\-\-\-\-\-\-\-\--

[\[root@ha02 \~\]# vgs]

  VG      #PV #LV #SN Attr   VSize    VFree   

  vg_gfs2   1   0   0 wz\--ns \<500.00g \<500.00g

 

[\[root@ha02 \~\]# vgchange \--lock-start vg_gfs2]

  Starting locking.  Waiting until locks are ready...

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\# create logical volume

[\[root@ha01 \~\]# lvcreate -l 100%FREE -n lv_gfs2 vg_gfs2]

  Logical volume \"lv_gfs2\" created.

 

\# format with GFS2

[\[root@ha01 \~\]# mkfs.gfs2 -j2 -p lock_dlm -t MyHR89C:gfs2-01 /dev/vg_gfs2/lv_gfs2 ]

Warning: Block size 4096 is inefficient because it is less than the physical block size (16384).

/dev/vg_gfs2/lv_gfs2 is a symbolic link to /dev/dm-0

This will destroy any data on /dev/dm-0

Are you sure you want to proceed? \[y/n\] y

Discarding device contents (may take a while on large devices): Done

Adding journals: Done

Building resource groups: Done       

Creating quota file: Done

Writing superblock and syncing: Done

Device:                    /dev/vg_gfs2/lv_gfs2

Block size:                4096

Device size:               500.00 GB (131070976 blocks)

Filesystem size:           500.00 GB (131070973 blocks)

Journals:                  2

Journal size:              128MB

Resource groups:           2001

Locking protocol:          \"lock_dlm\"

Lock table:                \"MyHR89C:gfs2-01\"

UUID:                      396dc6b1-6556-4b21-8762-bbcf81dedfda

 

\# create LVM-activate resource[  \--\> ]创建 LVM 激活资源

[\# \[shared_lv\] ]⇒ any name

[\# \[\--group\] ]⇒ any group name

[\[root@ha01 \~\]# pcs resource create shared_lv ocf:heartbeat:LVM-activate lvname=lv_gfs2 vgname=vg_gfs2 activation_mode=shared vg_access_mode=lvmlockd \--group shared_vg]

 

\# create clone of \[LVM-activate\]

[\[root@ha01 \~\]# pcs resource clone shared_vg interleave=true]

\# 这个命令克隆了名为 shared_vg 的资源，使用 interleave=true 选项表示该资源可以在多个节点上同时运行。

 

\# set start order as \[locking\] → \[shared_vg\]

[\[root@ha01 \~\]# pcs constraint order start locking-clone then shared_vg-clone]

Adding locking-clone shared_vg-clone (kind: Mandatory) (Options: first-action=start then-action=start)

\# 这个设置定义了启动顺序的约束条件，确保在启动 shared_vg-clone 之前，locking-clone 已经启动。

 

\# set that \[shared_vg\] and \[locking\] start on a same node

[\[root@ha01 \~\]# pcs constraint colocation add shared_vg-clone with locking-clone]

\# 这个命令定义了资源 colocation 约束，确保 shared_vg-clone 和 locking-clone 在同一节点上运行。这是为了确保逻辑卷和锁定服务在同一节点上可用。

 

\# create Filesystem resource

[\# \[shared_fs\] ]⇒ any name

[\# \[device\] ]⇒ device with GFS2 formatted

[\# \[directory\] ]⇒ any directory you\'d like to mount GFS2 filesystem

[\# \[\--group\] ]⇒ the same group with LVM-activate resource

[\[root@ha01 \~\]# pcs resource create shared_fs ocf:heartbeat:Filesystem device=\"/dev/vg_gfs2/lv_gfs2\" directory=\"/var/ftp/GFS2\" fstype=\"gfs2\" options=noatime op monitor interval=10s on-fail=fence \--group shared_vg]

\# 这个命令创建了名为 shared_fs 的资源，用于管理 GFS2 文件系统。它使用指定的 LVM 逻辑卷，指定的目录和文件系统类型。options=noatime 是文件系统的挂载选项，op monitor interval=10s 配置监视操作，每隔10秒执行一次，on-fail=fence 指定在监视失败时执行 fence 操作。

 

 

[\[root@ha01 \~\]# pcs status \--full]

Cluster name: MyHR89C

Cluster Summary:

  \* Stack: corosync (Pacemaker is running)

  \* Current DC: ha01.ddcab.com (1) (version 2.1.6-9.1.el8_9-6fdc9deea29) - partition with quorum

  \* Last updated: Tue Feb 20 14:16:00 2024 on ha01.ddcab.com

  \* Last change:  Tue Feb 20 13:29:45 2024 by root via cibadmin on ha01.ddcab.com

  \* 2 nodes configured

  \* 9 resource instances configured

 

Node List:

  \* Node ha01.ddcab.com (1): online, feature set 3.17.4

  \* Node ha02.ddcab.com (2): online, feature set 3.17.4

 

Full List of Resources:

  \* Clone Set: locking-clone \[locking\]:

    \* Resource Group: locking:0:

      \* dlm        (ocf::pacemaker:controld):         Started ha01.ddcab.com

      \* lvmlockd        (ocf::heartbeat:lvmlockd):         Started ha01.ddcab.com

    \* Resource Group: locking:1:

      \* dlm        (ocf::pacemaker:controld):         Started ha02.ddcab.com

      \* lvmlockd        (ocf::heartbeat:lvmlockd):         Started ha02.ddcab.com

  \* vmfence        (stonith:fence_vmware_rest):         Started ha01.ddcab.com

  \* Clone Set: shared_vg-clone \[shared_vg\]:

    \* Resource Group: shared_vg:0:

      \* shared_lv        (ocf::heartbeat:LVM-activate):         Started ha01.ddcab.com

      \* shared_fs        (ocf::heartbeat:Filesystem):         Started ha01.ddcab.com

    \* Resource Group: shared_vg:1:

      \* shared_lv        (ocf::heartbeat:LVM-activate):         Started ha02.ddcab.com

      \* shared_fs        (ocf::heartbeat:Filesystem):         Started ha02.ddcab.com

 

Migration Summary:

 

Tickets:

 

PCSD Status:

  ha01.ddcab.com: Online

  ha02.ddcab.com: Online

 

Daemon Status:

  corosync: active/enabled

  pacemaker: active/enabled

  pcsd: active/enabled

\# 如果有问题可以两个 node都重启一下

 

[\[root@ha01 \~\]#] df -hT /var/ftp/GFS2/

Filesystem                  Type  Size  Used Avail Use% Mounted on

/dev/mapper/vg_gfs2-lv_gfs2 gfs2  500G  259M  500G   1% /var/ftp/GFS2

 

[\[root@ha02 \~\]#] df -hT /var/ftp/GFS2/

Filesystem                  Type  Size  Used Avail Use% Mounted on

/dev/mapper/vg_gfs2-lv_gfs2 gfs2  500G  259M  500G   1% /var/ftp/GFS2

 

[\[root@ha01 \~\]#] lsblk

NAME                MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT

sda                   8:0    0   60G  0 disk

├─sda1[                8:1    0  200M  0 part /boot/efi]

├─sda2[                8:2    0    4G  0 part \[SWAP\]]

└─sda3                8:3    0 55.8G  0 part /

sdb                   8:16   0  150G  0 disk

└─sdb1                8:17   0 87.6G  0 part

sdc                   8:32   0   30G  0 disk

sdd                   8:48   0  500G  0 disk

└─sdd1                8:49   0  500G  0 part

  └─vg_gfs2-lv_gfs2 253:0    0  500G  0 lvm  /var/ftp/GFS2

sr0                  11:0    1 12.6G  0 rom  

\# node2 也是相同的输出

 

已经是自动有挂载了，可以向里面写入文件。

[\[root@ha01 \~\]#] mount \|grep gfs2

/dev/mapper/vg_gfs2-lv_gfs2 on /var/ftp/GFS2 type gfs2 (rw,noatime)

 

[\[root@ha02 \~\]#] mount \|grep gfs2

/dev/mapper/vg_gfs2-lv_gfs2 on /var/ftp/GFS2 type gfs2 (rw,noatime)

 

 

 

已使用 OneNote 创建。
