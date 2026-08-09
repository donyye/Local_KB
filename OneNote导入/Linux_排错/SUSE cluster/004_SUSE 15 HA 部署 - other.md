SUSE 15 HA 部署 - other

2024年4月10日

14:07

 

 

s1:\~ \# iscsiadm \--mode discoverydb \--type sendtargets \--portal 10.10.40.241 \--discover

10.10.40.241:3260,-1 iqn.rhel-1.example.com:10g

10.10.40.241:3260,-1 iqn.rhel-2.example.com:30g

10.10.40.241:3260,-1 iqn.suse-1.example.com:10g

10.10.40.241:3260,-1 iqn.suse-2.example.com:30g

10.10.40.241:3260,-1 iqn.test-1.example.com:18g

10.10.40.241:3260,-1 iqn.test-2.example.com:400g

 

 

 

s1:\~ \# iscsiadm \--mode node \--targetname iqn.suse-1.example.com:10g \--portal 10.10.40.241 \--login

Logging in to \[iface: default, target: iqn.suse-1.example.com:10g, portal: 10.10.40.241,3260\]

Login to \[iface: default, target: iqn.suse-1.example.com:10g, portal: 10.10.40.241,3260\] successful.

 

s1:\~ \# iscsiadm \--mode node \--targetname iqn.suse-2.example.com:30g \--portal 10.10.40.241 \--login

Logging in to \[iface: default, target: iqn.suse-2.example.com:30g, portal: 10.10.40.241,3260\]

Login to \[iface: default, target: iqn.suse-2.example.com:30g, portal: 10.10.40.241,3260\] successful.

 

s1:\~ \# iscsiadm \--mode node \--targetname iqn.suse-1.example.com:10g \--portal 10.10.40.241 -o update -n node.startup -v automatic

 

s1:\~ \# iscsiadm \--mode node \--targetname iqn.suse-2.example.com:30g \--portal 10.10.40.241 -o update -n node.startup -v automatic

 

s1:\~ \# echo softdog \> /etc/modules-load.d/watchdog.conf

s1:\~ \# systemctl restart systemd-modules-load

s1:\~ \#[  lsmod \| grep dog]

softdog                16384  0

 

如果不开启watchdog，配置集群的 SBD 时会出现下面报错。

\.....

Do you wish to use SBD (y/n)? y

ERROR: 3: cluster.init: Watchdog device must be configured in order to use SBD

 

 

s1:\~ \# zypper install -t pattern ha_sles

 

 

s1:\~ \# ha-cluster-init \--name SUClu15 -i eth0

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

  Multicast address (e.g.: 239.x.x.x) \[239.112.98.9\]

  Multicast port \[5405\]

  

Configure SBD:

  If you have shared storage, for example a SAN or iSCSI target,

  you can use it avoid split-brain scenarios by configuring SBD.

  This requires a 1 MB partition, accessible to all nodes in the

  cluster.  The device path must be persistent and consistent

  across all nodes in the cluster, so /dev/disk/by-id/\* devices

  are a good choice.  Note that all data on the partition you

  specify here will be destroyed.

 

Do you wish to use SBD (y/n)? n[  ][ ]\<\-\--先不安装SBD

WARNING: Not configuring SBD - STONITH will be disabled.

  Hawk cluster interface is now running. To see cluster status, open:

    <https://10.10.40.61:7630/>

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

  Virtual IP \[\]10.10.40.66

  Configuring virtual IP (10.10.40.66)\....done

  Done (log saved to /var/log/ha-cluster-bootstrap.log)

 

s1:\~ \# passwd hacluster 

New password:

BAD PASSWORD: it is based on a dictionary word

Retype new password:

passwd: password updated successfully

 

 

 

<https://10.10.40.66:7630>

![[SUSE cluster_004_SUSE 15 HA 部署 - other_001.png]]

 

 

 

 

Node 2 加入集群：

s2:\~ \# ha-cluster-join -i eth0

  Join This Node to Cluster:

  You will be asked for the IP address of an existing node, from which

  configuration will be copied.  If you have not already configured

  passwordless ssh between nodes, you will be prompted for the root

  password of the existing node.

 

[  IP address or hostname of existing node (e.g.: 192.168.1.1) \[\]10.10.40.61] [ \<\-\-- ]node1

  Retrieving SSH keys - This may prompt for root@10.10.40.61:

Password:

  One new SSH key installed

  Configuring csync2\...done

  Merging known_hosts

  Probing for new partitions\...done

  Hawk cluster interface is now running. To see cluster status, open:

    <https://10.10.40.62:7630/>

  Log in with username \'hacluster\', password \'linux\'

WARNING: You should change the hacluster password to something more secure!

  Waiting for cluster\.....done

  Reloading cluster configuration\...done

  Done (log saved to /var/log/ha-cluster-bootstrap.log)

s2:\~ \#

 

 

因为之前没有提前做SSH key 两个node信任，但是通过脚本安装可以自动做，但是需要使用一下。

s1:\~ \# ssh root@s2

The authenticity of host \'s2 (10.10.40.62)\' can\'t be established.

ECDSA key fingerprint is SHA256:0OEAUnhY6g9/bMI1aVGOZPf+mTKraR47yqBLktmPl7A.

Are you sure you want to continue connecting (yes/no/\[fingerprint\])? fingerprint

Please type \'yes\', \'no\' or the fingerprint: yes

Warning: Permanently added \'s2,10.10.40.62\' (ECDSA) to the list of known hosts.

Password:

Last login: Tue Apr  9 09:25:02 2024 from 10.10.40.9

 

 

 

 

 

已使用 OneNote 创建。
