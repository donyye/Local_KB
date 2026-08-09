RHEL8-HA-build

2023年12月26日

13:24

RHEL 8.4 \-- 4.18.0-305.el8.x86_64

 

\# cat /etc/hosts

127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4

::1         localhost localhost.localdomain localhost6 localhost6.localdomain6

10.10.40.181    rh8a.example.com    rh8a

10.10.40.182    rh8b.example.com    rh8b

 

[\[root@rh8a \~\]#  yum install -y  pacemaker  pcs fence-agents-all vsftpd]

[\[root@rh8b \~\]# yum install -y  pacemaker  pcs fence-agents-all vsftpd]

 

[\[root@rh8a \~\]# systemctl start pcsd.service ]

[\[root@rh8a \~\]# systemctl enable pcsd.service ]

[\[root@rh8a \~\]# passwd hacluster]

Changing password for user hacluster.

New password: redhat

BAD PASSWORD: The password is shorter than 8 characters

Retype new password: redhat

passwd: all authentication tokens updated successfully.

[\[root@rh8b \~\]# systemctl start pcsd.service]

[\[root@rh8b \~\]# systemctl enable pcsd.service ]

[\[root@rh8b \~\]# passwd hacluster ]

Changing password for user hacluster.

New password: redhat

BAD PASSWORD: The password is shorter than 8 characters

Retype new password: redhat

passwd: all authentication tokens updated successfully.

集群各节点之间进行认证，认证配置和conf文件生成

[\[root@rh8a \~\]# pcs host auth rh8a.example.com -u hacluster -p redhat]

rh8a.example.com: Authorized

[\[root@rh8a \~\]# pcs host auth rh8b.example.com -u hacluster -p redhat]

rh8b.example.com: Authorized

\# 合并命令 pcs host auth rh8a.example.com rh8b.example.com -u hacluster -p redhat

[\[root@rh8a \~\]# pcs cluster setup MyCluster rh8a.example.com rh8b.example.com]

No addresses specified for host \'rh8a.example.com\', using \'rh8a.example.com\'

No addresses specified for host \'rh8b.example.com\', using \'rh8b.example.com\'

Destroying cluster on hosts: \'rh8a.example.com\', \'rh8b.example.com\'\...

rh8a.example.com: Successfully destroyed cluster

rh8b.example.com: Successfully destroyed cluster

Requesting remove \'pcsd settings\' from \'rh8a.example.com\', \'rh8b.example.com\'

rh8a.example.com: successful removal of the file \'pcsd settings\'

rh8b.example.com: successful removal of the file \'pcsd settings\'

Sending \'corosync authkey\', \'pacemaker authkey\' to \'rh8a.example.com\', \'rh8b.example.com\'

rh8a.example.com: successful distribution of the file \'corosync authkey\'

rh8a.example.com: successful distribution of the file \'pacemaker authkey\'

rh8b.example.com: successful distribution of the file \'corosync authkey\'

rh8b.example.com: successful distribution of the file \'pacemaker authkey\'

Sending \'corosync.conf\' to \'rh8a.example.com\', \'rh8b.example.com\'

rh8a.example.com: successful distribution of the file \'corosync.conf\'

rh8b.example.com: successful distribution of the file \'corosync.conf\'

Cluster has been successfully set up.

 

# \--start 选项，该选项将在群集的两个节点上启动群集服务。这里没添加。

[\[root@rh8a \~\]# pcs cluster start \--all]

rh8a.example.com: Starting Cluster\...

rh8b.example.com: Starting Cluster\...

[\[root@rh8a \~\]# pcs cluster enable \--all]

rh8a.example.com: Starting Cluster\...

rh8b.example.com: Starting Cluster\...

 

[\[root@rh8a \~\]# pcs cluster status]

Cluster Status:

 Cluster Summary:

   \* Stack: corosync

   \* Current DC: rh8a.example.com (version 2.1.2-4.el8_6.2-ada5c3b36e2) - partition with quorum

   \* Last updated: Fri Jul  1 12:03:35 2022

   \* Last change:  Fri Jul  1 12:03:09 2022 by hacluster via crmd on rh8a.example.com

   \* 2 nodes configured

   \* 0 resource instances configured

 Node List:

   \* Online: \[ rh8a.example.com rh8b.example.com \]

 

PCSD Status:

  rh8b.example.com: Online

  rh8a.example.com: Online

 

没有 Fencing设备时，禁用STONITH 组件功能

在 stonith-enabled="false" 的情况下，分布式锁管理器 (DLM) 等资源以及依赖DLM 的所有服务（例如 cLVM2、GFS2 和 OCFS2）都将无法启动。

 

[\[root@rh8a \~\]# pcs property set stonith-enabled=false]

[\[root@rh8a \~\]# pcs property config ]

Cluster Properties:

 cluster-infrastructure: corosync

 cluster-name: MyCluster

 dc-version: 2.1.2-4.el8_6.2-ada5c3b36e2

 have-watchdog: false

 stonith-enabled: false

\# 在生产环境里需要设置成"true"，意思是使用fenc设备。如果设置是"false",那么有fenc设备也不会使用。默认是true。

 

添加资源\--文件系统

[\[root@rh8a \~\]#] pcs resource create vsftpd_fs ocf:heartbeat:Filesystem device=\"/dev/disk/by-id/scsi-SFreeNAS_iSCSI_Disk_005056a0225a01-part1\" directory=\"/var/ftp\" fstype=\"xfs\" \--group vsftpd

\# 如果删除使用 pcs resource delete vsftpd_fs

 

添加资源\--VIP

[\[root@rh8a \~\]# pcs resource create vsftp_vip ocf:heartbeat:IPaddr2 ip=10.10.40.188 cidr_netmask=16 \--group vsftpd]

 

添加资源\--vsftpd服务

[\[root@rh8a \~\]# pcs resource create vsftpd_ser service:vsftpd \--group vsftpd]

 

两个node都需要

systemctl enable corosync; systemctl start corosync

systemctl enable pacemaker; systemctl start pacemaker [ ]

 

[\[root@rh8a \~\]# pcs status]

Cluster name: MyCluster

Cluster Summary:

  \* Stack: corosync

  \* Current DC: rh8a.example.com (version 2.1.2-4.el8_6.2-ada5c3b36e2) - partition with quorum

  \* Last updated: Fri Jul  1 16:59:25 2022

  \* Last change:  Fri Jul  1 16:47:41 2022 by root via cibadmin on rh8a.example.com

  \* 2 nodes configured

  \* 3 resource instances configured

Node List:

  \* Online: \[ rh8a.example.com rh8b.example.com \]

Full List of Resources:

  \* Resource Group: vsftpd:

    \* vsftp_vip    (ocf::heartbeat:IPaddr2):     Started rh8a.example.com

    \* vsftpd_ser    (service:vsftpd):     Started rh8a.example.com

    \* vsftpd_fs    (ocf::heartbeat:Filesystem):     Started rh8a.example.com

Daemon Status:

  corosync: active/enabled

  pacemaker: active/enabled

  pcsd: active/enabled

 

检验集群配置信息：

[\[root@rh8a \~\]# crm_verify -L -V]

没有输出说明集群配置没问题

 

RHEL8 vsftp 默认是不允许匿名用户访问的需要修改配置。

\# vim /etc/vsftpd/vsftpd.conf

anonymous_enable=yse 默认是NO，重启服务就可以了。

FYI： [https://blog.csdn.net/Howei\_\_/article/details/104398783](https://blog.csdn.net/Howei__/article/details/104398783)

![[Linux-TEDP-2018_2025_SEDP-2023_011_RHEL8-HA-build_001.png]]

 

 

 

 

 

=====有遇到的错误=====

1. 不知道为什么，使用 fence_vmware_soa 的方式添加VMwar_fence就是不成功会报错。

\# pcs stonith create vmfence fence_vmware_soap pcmk_monitor_timeout=120s pcmk_host_map=\"rh8a.example.com:vm-node1,rh8b.example.com:vm-node2\"  ipaddr=10.10.40.250 ssl=1 login=administrator@vsphere.local passwd=Pxxxxxx

#  [https://access.redhat.com/solutions/917813](https://access.redhat.com/solutions/917813) 通过这个KB也没解决

![[Linux-TEDP-2018_2025_SEDP-2023_011_RHEL8-HA-build_002.png]]

 

2. 清除fencing失败的操作

<https://www.golinuxcloud.com/cleanup-failed-actions-pcs-status-cluster/>

可以看有 fencing 失败的记录：

Failed Fencing Actions:

  \* reboot of rh8a.example.com failed: delegate=rh8b.example.com, client=pacemaker-controld.11331, origin=rh8b.example.com, last-failed=\'2022-07-06 11:54:24 +08:00\'

![[Linux-TEDP-2018_2025_SEDP-2023_011_RHEL8-HA-build_003.png]]

 

 通过 "pcs stonith history show" 能看到更多的记录 

![[Linux-TEDP-2018_2025_SEDP-2023_011_RHEL8-HA-build_004.png]]

 

使用下面方式就可以清除节点的fencing记录：

[\[root@rh8b \~\]# pcs stonith history cleanup rh8a.example.com]

cleaning up fencing-history for node rh8a.example.com

3. 错误资源的删除

![[Linux-TEDP-2018_2025_SEDP-2023_011_RHEL8-HA-build_005.png]]

 

\# pcs stonith delete VirtualIP

![[Linux-TEDP-2018_2025_SEDP-2023_011_RHEL8-HA-build_006.png]]

 

删除完成后

![[Linux-TEDP-2018_2025_SEDP-2023_011_RHEL8-HA-build_007.png]]

 

一开始使用下面命令删除没用

[\[root@localhost \~\]# pcs resource cleanup VirtualIP]

Cleaned up VirtualIP on node2.example.com

Cleaned up VirtualIP on node1.example.com

Waiting for 2 replies from the CRMd.. OK

 

=====集群属性和选项=====

<https://access.redhat.com/documentation/zh-cn/red_hat_enterprise_linux/7/html-single/high_availability_add-on_reference/index#s1-clusterproperties-HAAR>

除了本表格中描述的属性外，还有一些由集群软件公开的集群属性。对于这些属性，建议您不要修改其默认值。

 

+-----------------------+-----------------------+------------------------------------------------------------------+
| no-quorum-policy      | stop                  | 当集群没有仲裁（quorum）时该做什么。允许的值：                   |
|                       |                       |                                                                  |
|                       |                       | \* ignore - 继续所有资源管理                                     |
|                       |                       |                                                                  |
|                       |                       | \* freeze - 继续管理资源，但不会从受影响分区以外的节点中恢复资源 |
|                       |                       |                                                                  |
|                       |                       | \* stop - 停止受影响集群分区中的所有资源                         |
|                       |                       |                                                                  |
|                       |                       | \* suicide - 隔离受影响集群分区中的所有节点                      |
+-----------------------+-----------------------+------------------------------------------------------------------+

\# pcs property set no-quorum-policy=stop

 

 

 

已使用 OneNote 创建。
