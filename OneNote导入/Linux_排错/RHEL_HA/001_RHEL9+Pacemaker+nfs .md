RHEL9+Pacemaker+nfs 

2023年5月22日

15:35

 

 

\[root@node1[\~\]# ]yum install -y pcs fence-agents-all

\[root@node2[\~\]# ]yum install -y pcs fence-agents-all

 

\[root@node1[\~\]# ][ ]systemctl enable pcsd

\[root@node1[\~\]# ][ ]systemctl start pcsd

 

\[root@node2[\~\]# ][ ]systemctl enable pcsd

\[root@node2[\~\]# ][ ]systemctl start pcsd

 

 

\[root@node1[\~\]#] echo \"hacluster\" \| passwd \--stdin hacluster

\[root@node1[\~\]#] pcs host auth rh9a.haha.com rh9b.haha.com -u hacluster -p hacluster

\[root@node1[\~\]#] pcs cluster setup nfs_cluster rh9a.haha.com rh9b.haha.com 

\[root@node1[\~\]#] pcs cluster start \--all

\[root@node1[\~\]#] pcs cluster enable \--all

 

 

\[root@node1[\~\]# ]mkdir /nfsshare

\[root@node2[\~\]# ]mkdir /nfsshare

 

没有fences时要设置false，否则后面服务起不来

\[root@node1[\~\]# ][ ]pcs property set stonith-enabled=false

\# 这个设置的作用

<https://access.redhat.com/solutions/2476841>

\# 为了使集群环境获得对集群类型问题的支持，必须将支持 stonith 的值设置为 true，以便围栏能够正常工作。这是一个支持需求，因为如果没有一个可用的围栏配置，将导致集群中出现意外的行为。\
查看 pcs property config

 

查看 iscsi share

\[root@node1[\~\]# ]lsblk 

NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS

sda      8:0    0   30G  0 disk

├─sda1[   8:1    0  600M  0 part /boot/efi]

├─sda2[   8:2    0    1G  0 part /boot]

├─sda3[   8:3    0    2G  0 part \[SWAP\]]

└─sda4   8:4    0 26.4G  0 part /

sdb      8:16   0   50G  0 disk

sdc[      8:32   0   10G  0 disk ][    \# iscsi-share disk]

sdd[      8:48   0   30G  0 disk ][    \# iscsi-share disk]

sr0     11:0    1  8.4G  0 rom  

 

 

\[root@node1[\~\]#] ll /dev/disk/by-id/

total 0

lrwxrwxrwx. 1 root root 9 May 19 16:34 ata-VMware_Virtual_SATA_CDRW_Drive_00000000000000000001 -\> ../../sr0

lrwxrwxrwx. 1 root root 9 May 19 17:19 scsi-1FreeNAS_iSCSI_Disk_005056a0a47900 -\> ../../sdc

lrwxrwxrwx. 1 root root 9 May 19 17:19 scsi-1FreeNAS_iSCSI_Disk_005056a0a47901 -\> ../../sdd

lrwxrwxrwx. 1 root root 9 May 19 17:19 scsi-36589cfc00000055a49e852683556a60b -\> ../../sdd

lrwxrwxrwx. 1 root root 9 May 19 17:19 scsi-36589cfc000000872dc465bba19820517 -\> ../../sdc

lrwxrwxrwx. 1 root root 9 May 19 17:19 scsi-SFreeNAS_iSCSI_Disk_005056a0a47900 -\> ../../sdc

lrwxrwxrwx. 1 root root 9 May 19 17:19 scsi-SFreeNAS_iSCSI_Disk_005056a0a47901 -\> ../../sdd

lrwxrwxrwx. 1 root root 9 May 19 17:19 wwn-0x6589cfc00000055a49e852683556a60b -\> ../../sdd

lrwxrwxrwx. 1 root root 9 May 19 17:19 wwn-0x6589cfc000000872dc465bba19820517 -\> ../../sdc

 

下面操作都在 node1 上完成

添加 iscsi fence

\# pcs stonith create disk_fencing fence_scsi pcmk_host_list=\"rh9a.haha.com rh9b.haha.com\" pcmk_monitor_action=\"metadata\" pcmk_reboot_action=\"off\" devices=\"/dev/disk/by-id/scsi-SFreeNAS_iSCSI_Disk_005056a0a47900\" meta provides=\"unfencing\"

 

\# pcs status

\...\...

Full List of Resources:

  \* disk_fencing        (stonith:fence_scsi):         Started rh9a.haha.com

  \...\...

 

 

 

添加 NFS 的资源:

Filesystem resource

nfsserver resource

IPaddr2 floating IP address resource

exportfs resource

 

Filesystem resource:

\# pcs resource create nfsshare ocf:heartbeat:Filesystem device=/dev/disk/by-id/scsi-SFreeNAS_iSCSI_Disk_005056a0a47901-part1 directory=/nfsshare fstype=xfs \--group nfsgroup

 

\# pcs status

\...\...

Full List of Resources:

  \* disk_fencing        (stonith:fence_scsi):         Started rh9a.haha.com

  \* Resource Group: nfsgroup:

    \* nfsshare        (ocf:heartbeat:Filesystem):         Started rh9b.haha.com

\...\...

 

 

nfsserver resource:

\# pcs resource create nfsd ocf:heartbeat:nfsserver nfs_shared_infodir=/nfsshare/nfsinfo \--group nfsgroup

 

\# pcs status

\...\...

Full List of Resources:

  \* disk_fencing        (stonith:fence_scsi):         Started rh9a.haha.com

  \* Resource Group: nfsgroup:

    \* nfsshare        (ocf:heartbeat:Filesystem):         Started rh9b.haha.com

   [ \* nfsd        (ocf:heartbeat:nfsserver):         Started rh9b.haha.com]

\...\...

 

 

IPaddr2 floating IP address resourc:

\# pcs resource create nfs_vip ocf:heartbeat:IPaddr2 ip=10.10.40.111 cidr_netmask=16 \--group nfsgroup

 

\# pcs status

\...\...

Full List of Resources:

  \* disk_fencing        (stonith:fence_scsi):         Started rh9a.haha.com

  \* Resource Group: nfsgroup:

    \* nfsshare        (ocf:heartbeat:Filesystem):         Started rh9a.haha.com

    \* nfsd        (ocf:heartbeat:nfsserver):         Started rh9a.haha.com

  [  \* nfs_vip        (ocf:heartbeat:IPaddr2):         Started rh9a.haha.com]

\...\...

 

 

\# pcs resource create nfs_notify ocf:heartbeat:nfsnotify source_host=10.10.40.111 \--group nfsgroup

 

exportfs resource:

\[root@node1 \~\]# mkdir -p /nfsshare/share01/share02

\[root@node2[ \~\]# mkdir -p /nfsshare/share01/share02]

 

\# pcs resource create nfs_root ocf:heartbeat:exportfs clientspec=10.10.40.0/255.255.0.0 options=rw,sync,no_root_squash directory=/nfsshare/share01 fsid=0 \--group nfsgroup

 

\# pcs status

\...\...

Full List of Resources:

  \* disk_fencing        (stonith:fence_scsi):         Started rh9a.haha.com

  \* Resource Group: nfsgroup:

    \* nfsshare        (ocf:heartbeat:Filesystem):         Started rh9a.haha.com

    \* nfsd        (ocf:heartbeat:nfsserver):         Started rh9a.haha.com (Monitoring)

  [  \* nfs_notify        (ocf:heartbeat:nfsnotify):         Starting rh9a.haha.com]

\...\...

 

 

\# pcs resource create nfs_share02 ocf:heartbeat:exportfs clientspec=10.10.40.0/255.255.0.0 options=rw,sync,no_root_squash directory=/nfsshare/share01/share02 fsid=1 \--group nfsgroup

 

\* nfs_share02        (ocf:heartbeat:exportfs):         Started rh9b.haha.com

 

 

[\[root@localhost \~\]#] showmount -e

Export list for localhost.localdomain:

/nfsshare/share01         10.10.40.0/255.255.0.0

/nfsshare/share01/share02 10.10.40.0/255.255.0.0

 

 

client：

mount -t nfs4 10.10.40.111:share02 /data1

 

cat /etc/fstable:

10.10.40.111:share02[  /data1  nfs rw,sync,v4.2  0 0]

 

\[root@master \~\]# df -hT /data1/

Filesystem            Type  Size  Used Avail Use% Mounted on

10.10.40.111:/share02 nfs4   30G  247M   30G   1% /data1

 

 

 

====================

\[root@localhost \~\]# pcs stonith

  \* disk_fencing        (stonith:fence_scsi):         Started rh9a.haha.com

 

 

 

已使用 OneNote 创建。
