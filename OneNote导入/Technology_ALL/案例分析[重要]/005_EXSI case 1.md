EXSI case 1

Friday, June 20, 2014

9:23 AM

客户发现6月15日上午十点刀片出现无法连接的情况，虚拟服务也断掉，客户直接重启了刀片，发现刀片重启时有报错，但不记得具体什么报错。目前客户将服务迁移到其他机器，到现在4天时间没有出现问题。

指导客户收集Dset日志及ESXi日志，没有发现硬件问题，Esxi中有timeout错误及Host down信息。客户是加入的Cluster及共享存储连接。

客户今天需要一个答复及工程师上门检查，已经给客户说明硬件检查都正常，但客户在未查出真正原因前不接受这个解释。

请今天下班前给客户回复下。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\
该主机管理地址为192.168.108.23，推测为双节点的HA集群，另外一个节点管理地址为192.168.108.25，在6月15日凌晨1点左右开始出现管理网络连接丢失。

2014-06-14T20:33:00.566Z \[FFF6BB90 verbose \'Cluster\' opID=SWI-b1d2bb4e\] \[ClusterManagerImpl::IsBadIP\] 192.168.108.25 is bad ip

2014-06-14T20:33:01.528Z \[499C4B90 info \'Message\'\] Destroying connection

2014-06-14T20:33:01.566Z \[FFF6BB90 verbose \'Cluster\' opID=SWI-b1d2bb4e\] \[ClusterManagerImpl::IsBadIP\] 192.168.108.25 is bad ip

2014-06-14T20:33:02.566Z \[FFF6BB90 verbose \'Cluster\' opID=SWI-b1d2bb4e\] \[ClusterManagerImpl::IsBadIP\] 192.168.108.25 is bad ip

2014-06-14T20:33:03.528Z \[49983B90 info \'Message\'\] Destroying connection

2014-06-14T20:33:03.566Z \[FFF6BB90 verbose \'Cluster\' opID=SWI-b1d2bb4e\] \[ClusterManagerImpl::IsBadIP\] 192.168.108.25 is bad ip

2014-06-14T20:33:04.567Z \[FFF6BB90 verbose \'Cluster\' opID=SWI-b1d2bb4e\] \[ClusterManagerImpl::IsBadIP\] 192.168.108.25 is bad ip

2014-06-14T20:33:04.586Z \[FFFEDB90 warning \'Cluster\' opID=SWI-3f7e1da5\] \[HostPing::Ping\] sendto\[ipv4\] 192.168.108.254: Host is down

 

该客户的production network跟管理网络在同一个Switch上，所以推测当时随着管理网络连接一起丢失，所以客户说"虚拟服务中断"

Switch Name      Num Ports   Used Ports  Configured Ports  MTU     Uplinks  

vSwitch0         128         4           128               1500    vmnic0,vmnic1

 

  PortGroup Name        VLAN ID  Used Ports  Uplinks  

  VLAN5                 5        0           vmnic0,vmnic1

  VLAN15                15       0           vmnic0,vmnic1

  Production Network    2        0           vmnic0,vmnic1

  VM Network            19       0           vmnic0,vmnic1

  Management Network    19       1           vmnic0,vmnic1

 

查询日志发现客户使用的网卡为5720

000:005:00.0 Ethernet controller Network controller: Broadcom Corporation NetXtreme BCM5720 Gigabit Ethernet \[vmnic0\]

         Class 0200: 14e4:165f

000:005:00.1 Ethernet controller Network controller: Broadcom Corporation NetXtreme BCM5720 Gigabit Ethernet \[vmnic1\]

         Class 0200: 14e4:165f

 

该网卡所使用的驱动为目前已经存在bug的版本，会导致网络闪断，只有重启才能恢复

Broadcom_bootbank_net-tg3_3.123b.v50.1-1OEM.500.0.0.472560:

   Name: net-tg3

   Version: 3.123b.v50.1-1OEM.500.0.0.472560

 

基于以上情况，建议如下

1：客户所使用的ESXi当前版本为5.0U1， 过于陈旧，建议至少升级至5.0 U3

 

2：升级5720驱动至3.136h

<https://my.vmware.com/web/vmware/details?downloadGroup=DT-ESXI5X-BROADCOM-TG3-3136HV501&productId=229>

 

3：检查交换机历史记录，确认以上情况及时间点符合日志中表现的情况。

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

已使用 OneNote 创建。
