[\[RHEL8\] ]Building steps

2024年2月19日

11:18

RHEL8+Pacemaker+vfstp

 

包安装

\[root@ha01 \~\]# yum install pacemaker  pcs fence-agents-all vsftpd

\[root@ha02[ \~\]# yum install pacemaker  pcs fence-agents-all vsftpd]

 

启动pcsd服务

\[root@ha01 \~\]# systemctl start pcsd.service

\[root@ha01 \~\]# systemctl enable pcsd.service

\[root@ha01 \~\]# passwd hacluster

 

\[root@ha02 \~\]# systemctl start pcsd.service

\[root@ha02 \~\]# systemctl enable pcsd.service

\[root@ha02[ \~\]# passwd hacluster]

 

组建集群

[\[root@ha01 \~\]#] pcs host auth ha01.ddcab.com ha02.ddcab.com -u hacluster -p redhat

ha01.ddcab.com: Authorized

ha02.ddcab.com: Authorized

 

创建集群

[\[root@ha01 \~\]#] pcs cluster setup MyHR89C ha01.ddcab.com ha02.ddcab.com

No addresses specified for host \'ha01.ddcab.com\', using \'ha01.ddcab.com\'

No addresses specified for host \'ha02.ddcab.com\', using \'ha02.ddcab.com\'

Destroying cluster on hosts: \'ha01.ddcab.com\', \'ha02.ddcab.com\'\...

ha01.ddcab.com: Successfully destroyed cluster

ha02.ddcab.com: Successfully destroyed cluster

Requesting remove \'pcsd settings\' from \'ha01.ddcab.com\', \'ha02.ddcab.com\'

ha01.ddcab.com: successful removal of the file \'pcsd settings\'

ha02.ddcab.com: successful removal of the file \'pcsd settings\'

Sending \'corosync authkey\', \'pacemaker authkey\' to \'ha01.ddcab.com\', \'ha02.ddcab.com\'

ha01.ddcab.com: successful distribution of the file \'corosync authkey\'

ha01.ddcab.com: successful distribution of the file \'pacemaker authkey\'

ha02.ddcab.com: successful distribution of the file \'corosync authkey\'

ha02.ddcab.com: successful distribution of the file \'pacemaker authkey\'

Sending \'corosync.conf\' to \'ha01.ddcab.com\', \'ha02.ddcab.com\'

ha01.ddcab.com: successful distribution of the file \'corosync.conf\'

ha02.ddcab.com: successful distribution of the file \'corosync.conf\'

Cluster has been successfully set up.

 

启动cluster服务

[\[root@ha01 \~\]#] pcs cluster start \--all

ha01.ddcab.com: Starting Cluster\...

ha02.ddcab.com: Starting Cluster\...

 

[\[root@ha01 \~\]#] pcs cluster enable \--all

ha01.ddcab.com: Cluster Enabled

ha02.ddcab.com: Cluster Enabled

 

查看状态

[\[root@ha01 \~\]#] pcs status

Cluster name: MyHR89C

 

WARNINGS:

No stonith devices and stonith-enabled is not false

 

Cluster Summary:

  \* Stack: corosync (Pacemaker is running)

  \* Current DC: ha02.ddcab.com (version 2.1.6-9.1.el8_9-6fdc9deea29) - partition with quorum

  \* Last updated: Mon Feb 19 11:33:48 2024 on ha01.ddcab.com

  \* Last change:  Mon Feb 19 11:33:05 2024 by hacluster via crmd on ha02.ddcab.com

  \* 2 nodes configured

  \* 0 resource instances configured

 

Node List:

  \* Online: \[ ha01.ddcab.com ha02.ddcab.com \]

 

Full List of Resources:

  \* No resources

 

Daemon Status:

  corosync: active/enabled

  pacemaker: active/enabled

  pcsd: active/enabled

 

 

没有 Fencing设备时，禁用STONITH 组件功能

在 stonith-enabled="false" 的情况下，分布式锁管理器 (DLM) 等资源以及依赖DLM 的所有服务（例如 cLVM2、GFS2 和 OCFS2）都将无法启动。

 

[\[root@ha01 \~\]#] pcs property set stonith-enabled=false

 

\[root@ha01 \~\]# pcs property config

Cluster Properties:

 cluster-infrastructure: corosync

 cluster-name: MyHR89C

 dc-version: 2.1.6-9.1.el8_9-6fdc9deea29

 have-watchdog: false

 stonith-enabled: false

 

\# 在生产环境里需要设置成"true"，意思是使用fenc设备。如果设置是"false",那么有fenc设备也不会使用。默认是true。

 

 

添加资源\--文件系统

[\[root@rh8a \~\]#] pcs resource create vsftpd_fs ocf:heartbeat:Filesystem device=\"/dev/disk/by-id/scsi-SFreeNAS_iSCSI_Disk_005056a0225a01-part1\" directory=\"/var/ftp\" fstype=\"xfs\" \--group vsftpd

\# 如果删除使用 pcs resource delete vsftpd_fs

 

添加资源\--VIP

\[root@rh8a \~\]# pcs resource create vsftp_vip ocf:heartbeat:IPaddr2 ip=10.10.40.188 cidr_netmask=16 \--group vsftpd

 

添加资源\--vsftpd服务

\[root@rh8a \~\]# pcs resource create vsftpd_ser service:vsftpd \--group vsftpd

 

 

 

已使用 OneNote 创建。
