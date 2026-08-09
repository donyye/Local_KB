vsan 检查

2023年3月28日

15:23

 

》查看那个口是跑vsan网络，然后再ping节点间的联通性。

esxcfg-vmknic -l

vmkping -I vmk1 192.168.200.9

 

 

》所有节点都要show，看是否有问题，那个主节点。

[\[root@localhost:\~\] esxcli vsan cluster get  ]\-\--\> 或者 localcli vsan cluster get

Cluster Information

   Enabled: true

   Current Local Time: 2022-07-26T08:17:29Z

   Local Node UUID: 617b9736-87f7-c584-5bcb-00505689a23b

   Local Node Type: NORMAL

[   Local Node State: AGENT   ]\-\--\> 从节点，如果是主节点是 MASTER

   Local Node Health State: HEALTHY

   Sub-Cluster Master UUID: 617b9e14-8bb4-102c-37a6-00505689d43c

   Sub-Cluster Backup UUID: 617b9ac4-a309-4397-f6a5-0050568937c2

   Sub-Cluster UUID: 52557adc-dba7-0136-901e-6d2b9c417ea6

   Sub-Cluster Membership Entry Revision: 1

[   Sub-Cluster Member Count: 3    ]\--\> 可以看到此VSAN有三个节点(当前总共也是3节点)，如果有分区，这里也能看出来。

   Sub-Cluster Member UUIDs: 617b9e14-8bb4-102c-37a6-00505689d43c, 617b9ac4-a309-4397-f6a5-0050568937c2, 617b9736-87f7-c584-5bcb-00505689a23b

[   Sub-Cluster Member HostNames: localhost, localhost, localhost  ]\-\--\> 三个节点

   Sub-Cluster Membership UUID: 9f50da62-ce4e-feb2-c396-005056895df1

   Unicast Mode Enabled: true

[   Maintenance Mode State: OFF  ][ \-\--\> ]没有在维护模式下,如果显示ON

   Config Generation: 776d7cce-c2a7-41c9-80b6-df59eec58ec9 6 2022-07-22T07:44:39.970

   Mode: REGULAR

 

》在node1上看到连接了另外两个节点[   ]

\[root@localhost:\~\] esxcli vsan cluster unicastagent list

NodeUuid                              IsWitness  Supports Unicast  IP Address       Port  Iface Name  Cert Thumbprint                                              SubClusterUuid

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\--  \-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\--

617b9ac4-a309-4397-f6a5-0050568937c2          0              true  192.168.200.92  12321              10:2F:22:FF:DA:04:69:0D:0E:31:CD:DC:10:FC:F4:FB:F5:F7:2E:9B  52557adc-dba7-0136-901e-6d2b9c417ea6

617b9e14-8bb4-102c-37a6-00505689d43c          0              true  192.168.200.93  12321              72:34:AF:B7:8A:59:B1:72:99:86:A0:5B:4F:47:0B:26:A5:63:6B:46  52557adc-dba7-0136-901e-6d2b9c417ea6

 

 

》检查vsan健康状态

\[root@localhost:\~\] esxcli vsan health cluster list

Health Test Name                                    Status

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\--

Overall health                                      yellow (Cluster health issue)

Cluster                                             yellow

  Advanced vSAN configuration in sync               green

  vSAN daemon liveness                              green

  vSAN Disk Balance                                 green

  Resync operations throttling                      green

  Software version compatibility                    green

  Disk format version                               yellow

Network                                             green

  Hosts with connectivity issues                    green

  vSAN cluster partition                            green

  All hosts have a vSAN vmknic configured           green

  vSAN: Basic (unicast) connectivity check          green

  vSAN: MTU check (ping with large packet size)     green

  vMotion: Basic (unicast) connectivity check       green

  vMotion: MTU check (ping with large packet size)  green

  Network latency check                             green

Physical disk                                       green

  Operation health                                  green

  Disk capacity                                     green

  Congestion                                        green

  Component limit health                            green

  Component metadata health                         green

  Memory pools (heaps)                              green

  Memory pools (slabs)                              green

Data                                                green

  vSAN object health                                green

  vSAN object format health                         green

Capacity utilization                                green

  Storage space                                     green

  Read cache reservations                           green

  Component                                         green

  What if the most consumed host fails              green

Performance service                                 green

  Stats DB object                                   green

  Stats primary election                            green

  Performance data collection                       green

  All hosts contributing stats                      green

  Stats DB object conflicts                         green

 

》vsan node一致性检查。

1）先检查node是否在维护模式，有就退出。

esxcli vsan cluster get[  \--\> ]查看

esxcli system maintenanceMode set -e false[  \--\> ]退出维护模式

 

如果上述无法退出维护模式，使用命令：

localcli vsan maintenancemode cancel

 

<https://kb.vmware.com/s/article/87350>

2）每个node都要运行

\[root@localhost:\~\] esxcfg-advcfg -g /VSAN/DOMPauseALLCCPs

Value of DOMPauseAllCCPs is 0[     \--]》默认正常是0

\[root@localhost:\~\] esxcfg-advcfg -g /VSAN/IgnoreclusterMemberListUpdates

Value of IgnoreClusterMemberListUpdates is 0[   \--]》默认正常是0

\[root@localhost:\~\]

 

如果不是0就改成 0

\[root@localhost:\~\] esxcfg-advcfg -s 0 /VSAN/DOMPauseALLCCPs

Value of DOMPauseAllCCPs is 0

\[root@localhost:\~\] esxcfg-advcfg -s 0 /VSAN/IgnoreclusterMemberListUpdates

Value of IgnoreClusterMemberListUpdates is 0

所有node都要一致

 

3）VC显示vsan集群已关闭，手动SSH到VC恢复

root@vc7 \[ \~ \]# cd /etc/vmware-vsan-health/

root@vc7 \[ /etc/vmware-vsan-health \]# cp config.conf config.conf.bak

root@vc7 \[ /etc/vmware-vsan-health \]# vim config.conf

\[PowerSystem\]

state_for_domain-c20 = vcVMPoweredOff[  ][ \--\> ]去掉这一行

 

重启服务

service-control \--restart vmware-vsan-health

  

===== vsan 网络方面的排查 ======[  ]

》列出所有虚拟交换机

esxcfg-vswitch -l

》列出所有端口组

esxcfg-vmknic -l

》查看网口是否up

esxcfg-nics -l

》ESXI上端口重启

esxcli network nic list

esxcli network nic down -n vmnic0

esxcli network nic up -n vmnic0

 

》用于捕获指定网络接口（vmnicX）上的以太网ARP数据包

pktcap-uw \--uplink vmnic1 \--capture UplinkRcvKernel \--ethtype 0x0806 \| tcpdump-uw -enr -

\...\...

Dumped 57 packet to console, dropped 0 packets.[  \--\>]将57个数据包转储到控制台，丢弃0个数据包。说明有ARP。

Done.

 

》用于列出指定网络接口或 ESXi 主机上所有网络接口的邻居信息。

esxcli network ip neighbor list

 

》查看你所有监听

esxcli network ip connection list \|grep -E \'902\|443\|903\|12321\'

 

》测试vsan节点连接端口

nc -vzu 10.10.40.92 12321

Connection to 10.10.40.92 12321 port \[udp/ssh\] succeeded!

 

》ping vsan网络节点是否都通信正常

vmkping -I vmk1 192.168.200.92

vmkping -I vmk1 192.168.200.92 -c 10

 

》ping 巨帧

vmkping -I vmk1 -s 9000 -d 192.168.200.92

 

》获取VMKX的IP地址

esxcli network ip interface ipv4 get

 

》显示所有 VMks 网络接口的地址解析协议 (ARP) 和邻居发现 (ND) 缓存中的已知网络邻居列表

在node1上运行的结果

esxcli network ip neighbor list

Neighbor        Mac Address        Vmknic     Expiry  State  Type

\-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\--  \-\-\-\-\-\-\-\--  \-\-\-\--  \-\-\--

\...\...

10.10.40.212    (incomplete)       vmk0       -1 sec         Invalid

192.168.200.92  00:50:56:6b:ea:4e  vmk1     1161 sec         Dynamic

192.168.200.93  00:50:56:64:46:d9  vmk1     1161 sec         Dynamic

\...\...

 

===== vsan 运行状态和磁盘方面 ======[  ]

》命令列出所有 vSAN 对象的健康状况摘要。

esxcli vsan debug object health summary get

 

Object Health Status        Description

[     5                          Healthy                ]健康的

[     6                          Absent                ]缺席的

[     9                          Degrade                ]降级

[     10                        Reconfiguring        ]重新配置

 

 

》对vSAN存储进行检查         

\[root@localhost:\~\] localcli vsan storage list \|grep CMMDS

   In CMMDS: true

   In CMMDS: true

   In CMMDS: true

   In CMMDS: true

 

 

=================================

<https://support.lenovo.com/us/en/solutions/ht509737-the-esxi-shell-commands-and-log-files-needed-to-troubleshoot-object-issues-in-thinkagile-vx>

 

<https://support.lenovo.com/gb/zc/solutions/ht509717>

=================================

=============

node脱离vsan

esxcli vsan cluster leave

=============

 

 

=============

VSAN 各个节点连通性抓包

 

1.确认vsan网络走的那个vmk

\[root@localhost:\~\] esxcfg-vmknic -l

\...\...

vmk1       VMkernel-vsan                           IPv4      192.168.200.91                          255.255.255.0   192.168.200.255 00:50:56:61:d3:72 1500    65535     true    STATIC              defaultTcpipStack

\...\...    

 

2.开始抓包：

每个node都运行一下

》进来的流量

\[root@localhost:\~\] pktcap-uw \--vmk vmk1 \--capture PortInput -o - \| tcpdump-uw -enr -

The name of the vmk is vmk1.

The session capture point is PortInput.

pktcap: The output file is -.

pktcap: No server port specifed, select 27181 as the port.

pktcap: Local CID 2.

pktcap: Listen on port 27181.

pktcap: Main thread: 955338963776.

pktcap: Dump Thread: 955339499264.

pktcap: Recv Thread: 955340027648.

pktcap: Accept\...

pktcap: Vsock connection from port 1026 cid 2.

reading from file -, link-type EN10MB (Ethernet)

03:32:56.994927 00:50:56:61:d3:72 \> 00:50:56:6b:ea:4e, ethertype IPv4 (0x0800), length 282: 192.168.200.91.12321 \> 192.168.200.92.12321: UDP, length 240

03:32:56.994928 00:50:56:61:d3:72 \> 00:50:56:64:46:d9, ethertype IPv4 (0x0800), length 282: 192.168.200.91.12321 \> 192.168.200.93.12321: UDP, length 240

03:32:57.042676 00:50:56:61:d3:72 \> 00:50:56:64:46:d9, ethertype IPv4 (0x0800), length 354: 192.168.200.91.8182 \> 192.168.200.93.8182: UDP, length 312

03:32:57.042677 00:50:56:61:d3:72 \> 00:50:56:64:46:d9, ethertype IPv4 (0x0800), length 354: 192.168.200.91.44585 \> 192.168.200.93.8182: UDP, length 312

03:32:57.042678 00:50:56:61:d3:72 \> 00:50:56:6b:ea:4e, ethertype IPv4 (0x0800), length 354: 192.168.200.91.8182 \> 192.168.200.92.8182: UDP, length 312

03:32:57.042709 00:50:56:61:d3:72 \> 00:50:56:6b:ea:4e, ethertype IPv4 (0x0800), length 354: 192.168.200.91.44585 \> 192.168.200.92.8182: UDP, length 312

03:32:57.061346 00:50:56:61:d3:72 \> 00:50:56:6b:ea:4e, ethertype IPv4 (0x0800), length 4338: 192.168.200.91.37530 \> 192.168.200.92.2233: Flags \[P.\], seq 38475745:38480017, ack 396218533, win 4097, options \[nop,nop,TS val 1542415670 ecr 2818106953\], length 4272

03:32:57.061717 00:50:56:61:d3:72 \> 00:50:56:6b:ea:4e, ethertype IPv4 (0x0800), length 5894: 192.168.200.91.37530 \> 192.168.200.92.2233: Flags \[.\], seq 4272:10100, ack 1, win 4097, options \[nop,nop,TS val 1542415670 ecr 2818107008\], length 5828

\...\...

\-\-\-\-\-\-\-- 正常 \-\-\-\-\-\-\--

 

》出去的流量

\[root@localhost:\~\] pktcap-uw \--vmk vmk1 \--capture PortOutput -o - \| tcpdump-uw -enr -

The name of the vmk is vmk1.

The session capture point is PortOutput.

pktcap: The output file is -.

pktcap: No server port specifed, select 27888 as the port.

pktcap: Local CID 2.

pktcap: Listen on port 27888.

pktcap: Main thread: 255434672960.

pktcap: Dump Thread: 255435208448.

pktcap: Recv Thread: 255435736832.

pktcap: Accept\...

pktcap: Vsock connection from port 1028 cid 2.

reading from file -, link-type EN10MB (Ethernet)

03:42:06.383627 00:50:56:64:46:d9 \> 00:50:56:61:d3:72, ethertype IPv4 (0x0800), length 282: 192.168.200.93.12321 \> 192.168.200.91.12321: UDP, length 240

03:42:06.507364 00:50:56:85:84:47 \> ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 60: Request who-has 10.10.250.131 tell 10.10.250.70, length 46

03:42:06.619293 f0:4d:a2:da:0c:5b \> ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 60: Reply 10.10.6.2 is-at f0:4d:a2:da:0c:5b, length 46

03:42:06.893871 00:50:56:89:a2:3b \> ff:ff:ff:ff:ff:ff, ethertype ARP (0x0806), length 60: Request who-has 10.10.40.212 tell 10.10.40.91, length 46

03:42:06.894346 00:50:56:6b:ea:4e \> 00:50:56:61:d3:72, ethertype IPv4 (0x0800), length 103: 192.168.200.92.22186 \> 192.168.200.91.8182: Flags \[P.\], seq 2870682074:2870682111, ack 4291430055, win 130, options \[nop,nop,TS val 1968807228 ecr 2120455463\], length 37

03:42:06.894347 00:50:56:64:46:d9 \> 00:50:56:61:d3:72, ethertype IPv4 (0x0800), length 103: 192.168.200.93.49949 \> 192.168.200.91.8182: Flags \[P.\], seq 2070844952:2070844989, ack 337100753, win 130, options \[nop,nop,TS val 178995850 ecr 2751848539\], length 37

\...\...

\-\-\-\-\-\-\-- 正常 \-\-\-\-\-\-\--

 

\-\-\-\-\-\-\--一些命令\-\-\-\-\-\-\-\--

\# localcli system maintenanceMode get

Disabled[   \--\> ]说明没在维护模式下

 

如果主机已取消维护模式，但是命令看到还是在维护模式下，使用下面命令。（ESXI上）

\# localcli vsan maintenancemode cancel

VsanUtil: AffObj count: 0

 

\# esxcli vm process list 查看一下ESXI下是否有vm

 

\# esxcli vsan cluster leave 把这个node提出vsan，慎重使用。

 

 

===== rvc

\# rvc administrator@vsphere.local@localhost

\> cd 1

/localhost\> ls

0 Data_CCD (datacenter)

/localhost\> cd 0

/localhost/Data_CCD\> ls

0 storage/

1 computers \[host\]/

2 networks \[network\]/

3 datastores \[datastore\]/

4 vms \[vm\]/

/localhost/Data_CCD\> cd 1

/localhost/Data_CCD/computers\> ls

0 HA_HOST (cluster): cpu 37 GHz, memory 17 GB

/localhost/Data_CCD/computers\> cd 0

/localhost/Data_CCD/computers/HA_HOST\> vsan.support_information ./[  \--\> ]在线查看

\..... 有大量信息输出

 

检查虚拟机和 vSAN 对象是否有效且可访问。-r 是刷新状态

/localhost/Data_CCD/computers/HA_HOST\> vsan.check_state -r ./

 

 

 

已使用 OneNote 创建。
