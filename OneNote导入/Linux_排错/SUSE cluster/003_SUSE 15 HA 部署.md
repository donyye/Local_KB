SUSE 15 HA 部署

2024年4月2日

11:11

SEL15 sp3

FYI：https://documentation.suse.com/zh-cn/sle-ha/15-SP1/single-html/SLE-HA-install-quick/#art-sleha-install-quick

通过上面SUSE KB 亲自验证

一、前期工作

1\. 设置好hosts，两个节点

10.10.40.81     su15a.ddoonnyy.com      su15a

10.10.40.82     su15b.ddoonnyy.com      su15b

2\. 设置好NTP，两个节点

 cat /etc/chrony.conf

server 10.10.40.212

3\. 使用 SBD 作为屏蔽机制

如果有 SAN（储存区域网络）等共享储存，可以使用它们来避免节点分裂的情况。要实现此目的，请配置 SBD 作为节点屏蔽机制。SBD 使用检查包支持和 external/sbd STONITH 资源代理。

使用 ha-cluster-init 设置第一个节点期间，您可以决定是否要使用 SBD。如果要使用，需要输入共享存储设备的路径。默认情况下，ha-cluster-init 将在设备上自动建立小型分区，供 SBD 使用。

要使用 SBD，必须符合以下要求：

群集中的所有节点上，共享存储设备的路径都必须永久且一致。使用稳定的设备名称，如 /dev/disk/by-id/dm-uuid-part1-mpath-abcedf12345。

SBD 设备不得使用基于主机的 RAID、LVM2，也不能位于 DRBD\* 实例上。

4\. 开启软狗

Softdog 驱动程序假设至少有一个 CPU 仍然在运行。如果所有 CPU 均已阻塞，则 softdog 驱动程序中应该重引导系统的代码永远都不会执行。相反地，即使所有 CPU 均已阻塞，硬件检查包也仍然会继续工作。

\# echo softdog \> /etc/modules-load.d/watchdog.conf

\# systemctl restart systemd-modules-load

\# lsmod \| grep dog

softdog                16384  1

5\. 挂载iscsi

\# zypper in open-iscsi\
iscsiadm \--mode discoverydb \--type sendtargets \--portal 192.168.200.xxx \--discover

iscsiadm \--mode node \--targetname iqn.suse-1.example.com:10g \--portal 192.168.200.xxx:3260 \--login

iscsiadm \--mode node \--targetname iqn.suse-1.example.com:10g \--portal 192.168.200.xxx -o update -n node.startup -v automatic

最后一步iscsi开机自动挂加载很总要，否则重启系统就没了。

 

二、两个node安装HA软件

zypper install -t pattern ha_sles

 

三、使用 ha-cluster-init 脚本设置第一个节点

su15a:\~ \# ha-cluster-init \--name amsterdam -i eth0

  Generating SSH key

  Configuring csync2

  Generating csync2 shared key (this may take a while)\...done

  csync2 checking files\...done

  

Configure Corosync:

  This will configure the cluster messaging layer.  You will need

  to specify a network address over which to communicate (default

  is eth0\'s network, but you can use the network address of any

  active interface).

  Network address to bind to (e.g.: 192.168.1.0) \[10.10.0.0\]

  Multicast address (e.g.: 239.x.x.x) \[239.110.143.59\]

  Multicast port \[5405\]

  

Configure SBD:

  If you have shared storage, for example a SAN or iSCSI target,

  you can use it avoid split-brain scenarios by configuring SBD.

  This requires a 1 MB partition, accessible to all nodes in the

  cluster.  The device path must be persistent and consistent

  across all nodes in the cluster, so /dev/disk/by-id/\* devices

  are a good choice.  Note that all data on the partition you

  specify here will be destroyed.

Do you wish to use SBD (y/n)? y

  Path to storage device (e.g. /dev/disk/by-id/\...), or \"none\" for diskless sbd, use \";\" as separator for multi path \[\]/dev/disk/by-path/pci-0000\\:03\\:00.0-scsi-0\\:0\\:2\\:0

/dev/disk/by-path/pci-0000\\:03\\:00.0-scsi-0\\:0\\:2\\:0 doesn\'t look like a block device   \--\> 输入SBD地址有错误

  Path to storage device (e.g. /dev/disk/by-id/\...), or \"none\" for diskless sbd, use \";\" as separator for multi path \[\]/dev/disk/by-id/scsi-1FreeNAS_iSCSI_Disk_0050568930f800-part1

WARNING: All data on /dev/disk/by-id/scsi-1FreeNAS_iSCSI_Disk_0050568930f800-part1 will be destroyed!

Are you sure you wish to use this device (y/n)? y

  Initializing SBD\...\...done

  Hawk cluster interface is now running. To see cluster status, open:

    <https://10.10.40.81:7630/>

  Log in with username \'hacluster\', password \'linux\'

WARNING: You should change the hacluster password to something more secure!

  Waiting for cluster\...\...\...\.....done

  Loading initial cluster configuration

  

Configure Administration IP Address:

  Optionally configure an administration virtual IP

  address. The purpose of this IP address is to

  provide a single IP that can be used to interact

  with the cluster, rather than using the IP address

  of any specific cluster node.

Do you wish to configure a virtual IP address (y/n)? y

  Virtual IP \[\]10.10.40.86

  Configuring virtual IP (10.10.40.86)\....done

  Done (log saved to /var/log/ha-cluster-bootstrap.log)

su15a:\~ # 

新命令： crm cluster init \--name xxx -i eth0

\#当 ha-cluster-init  时不加 -i eth0 有可能会报下面错误。

ERROR: cluster.join: Failed to determine default network interface

错误：cluster.init：无法确定默认网络接口

 

四、登录Hawk Web管理界面

[https://HAWKSERVER:7630/](https://HAWKSERVER:7630/)   这里使用的是之前设置的VIP地址加端口

![[SUSE cluster_003_SUSE 15 HA 部署_001.png]]

重新设置登陆密码：

su15a:\~ \# passwd hacluster

New password: 

BAD PASSWORD: it is based on a dictionary word

Retype new password: 

passwd: password updated successfully

设置完成后使用用户：hacluster 密码：redhat123 登录

 

![[SUSE cluster_003_SUSE 15 HA 部署_002.png]]

当然，可以在点击用户那里切换成中文

因为之前没有提前做SSH key 两个node信任，但是通过脚本安装可以自动做，但是需要使用一下。

如下：

su15a:\~ \# ssh root@su15b

The authenticity of host \'su15b (10.10.40.82)\' can\'t be established.

ECDSA key fingerprint is SHA256:RXtCeKS8R5bUcyn5YOQNsYUkAjkHdntUn37mYhCFDHw.

[Are you sure you want to continue connecting (yes/no/\[fingerprint\])?] fingerprint

Please type \'yes\', \'no\' or the fingerprint: yes

Warning: Permanently added \'su15b,10.10.40.82\' (ECDSA) to the list of known hosts.

Last login: Fri Sep 17 08:55:01 2021 from 10.10.40.9

su15b:\~ # 

 

五、添加第二个node

su15b:\~ \# ha-cluster-join -i eth0

  Join This Node to Cluster:

  You will be asked for the IP address of an existing node, from which

  configuration will be copied.  If you have not already configured

  passwordless ssh between nodes, you will be prompted for the root

  password of the existing node.

[  IP address or hostname of existing node (e.g.: 192.168.1.1) \[\]]10.10.40.81 \<\-\-- noide IP

  Retrieving SSH keys - This may prompt for root@10.10.40.81:

Password: 

  One new SSH key installed

  Configuring csync2\...done

  Merging known_hosts

  Probing for new partitions\...done

  Got SBD configuration

  Hawk cluster interface is now running. To see cluster status, open:

    <https://10.10.40.82:7630/>

  Log in with username \'hacluster\', password \'linux\'

WARNING: You should change the hacluster password to something more secure!

  Waiting for cluster\.....done

  Reloading cluster configuration\...done

  Done (log saved to /var/log/ha-cluster-bootstrap.log)

su15b:\~ # 

如果尚未配置两台计算机之间的无密码 SSH 访问，系统将提示您输入现有节点的 root 密码。

登录到指定节点后，脚本将复制 Corosync 配置、配置 SSH 和 Csync2，并使当前计算机作为新群集节点联机。除此之外，还将启动 Hawk2 所需的服务。

这时可以看到多了一个node

 

![[SUSE cluster_003_SUSE 15 HA 部署_003.png]]

 

六、测试

1\. VIP测试

想ping vip ，然后把su15a 切换到 standby状态，看vip是否会断开

![[SUSE cluster_003_SUSE 15 HA 部署_004.png]]

测试结果：

点击 切换su15a到standby状态，这时过了一会，web需要重新刷新访问，但是vip一直还是可以ping通。

另外回复su15a后 vip资源与其它资源没有切回这个node上面。

 

 

已使用 OneNote 创建。
