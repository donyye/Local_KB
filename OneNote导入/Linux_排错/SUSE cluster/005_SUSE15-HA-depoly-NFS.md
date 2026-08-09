SUSE15-HA-depoly-NFS

2023年11月7日

14:36

Zypper 安装 NFS\
s1:\~ \# zypper install nfs-kernel-server nfs-client 

s2:\~ \# zypper install nfs-kernel-server nfs-client 

 

设置所有服务器开机自启 NFS 服务

s1:\~ \# systemctl enable rpcbind

Created symlink /etc/systemd/system/multi-user.target.wants/rpcbind.service → /usr/lib/systemd/system/rpcbind.service.

Created symlink /etc/systemd/system/sockets.target.wants/rpcbind.socket → /usr/lib/systemd/system/rpcbind.socket.

 

s1:\~ \# systemctl enable nfsserver

Created symlink /etc/systemd/system/multi-user.target.wants/nfsserver.service → /usr/lib/systemd/system/nfsserver.service.

 

 

 所有服务器启动 NFS 服务

s1:\~ \# systemctl start rpcbind

s1:\~ \# systemctl start nfsserver

 

 

 

s1:\~ \# crm configure primitive nfs_service ocf:heartbeat:nfsserver params nfs_shared_infodir=\"/dev/disk/by-id/scsi-SFreeNAS_iSCSI_Disk_005056a0a47903-part1\" op start timeout=\"60s\" interval=\"0\" op stop timeout=\"120s\" interval=\"0\" op monitor timeout=\"60s\" interval=\"10s\"

 

 

s1:\~ \# crm configure primitive nfs_vip ocf:heartbeat:IPaddr2 params ip=\"10.10.40.167\" cidr_netmask=\"255.255.0.0\" nic=\"eth0\" op monitor interval=\"30s\"

 

停止后再删除

s1:\~ \# crm resource stop nfs_vip

s1:\~ \# crm configure delete nfs_vip

 

 

 

=======

s1:\~ \# crm configure property no-quorum-policy=ignore

s1:\~ \#

s1:\~ \# crm status

Cluster Summary:

  \* Stack: corosync

  \* Current DC: s1 (version 2.0.3+20200511.2b248d828-1.10-2.0.3+20200511.2b248d828) - partition with quorum

  \* Last updated: Tue Nov  7 16:13:15 2023

  \* Last change:  Tue Nov  7 16:13:10 2023 by root via cibadmin on s1

  \* 2 nodes configured

  \* 2 resource instances configured

 

Node List:

  \* Online: \[ s1 s2 \]

 

Full List of Resources:

  \* admin-ip        (ocf::heartbeat:IPaddr2):         Started s1

  \* stonith-sbd        (stonith:external/sbd):         Started s1

 

s1:\~ \#

 

 

s1:\~ \# crm cluster status

Name: amsterdam

 

Services:

corosync[         active/running/enabled]

pacemaker[        active/running/enabled]

 

Printing ring status.

Local node ID 168437821

RING ID 0

id        = 10.10.40.61

status        = ring 0 active with no faults

 

 

一些命令

  -------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------
  启动集                                                                                                                                                   crm cluster start
  停止某个资                                                                                                                                               crm resource stop stonith-sbd
  查看所有资源                                                                                                                                             crm resource list
  一直刷新cluster状态信息，包括资源   crm_mon -o -r
  编辑HA配置文件                      crm configure edit
  -------------------------------------------------------------------------------------------------------------------------------------------------------- -------------------------------

 

已使用 OneNote 创建。
