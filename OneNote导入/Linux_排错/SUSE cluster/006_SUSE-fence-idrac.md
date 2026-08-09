SUSE-fence-idrac

2024年5月28日

15:02

FYI: [http://www.voleg.info/suse-sap-ha-power.html](http://www.voleg.info/suse-sap-ha-power.html)

 

\# zypper install fence-agents

 

查看所有支持的fence模式

node1:\~ \# stonith -L

apcmaster

apcmastersnmp

apcsmart

baytech

cyclades

drac3

external/drac5

external/dracmc-telnet

external/ec2

external/hetzner

external/hmchttp

external/ibmrsa

external/ibmrsa-telnet

external/ipmi

external/ippower9258

external/kdumpcheck

external/libvirt

external/nut

external/rackpdu

external/riloe

external/sbd

external/vcenter

external/vmware

external/xen0

external/xen0-ha

ibmhmc

meatware

nw_rpc100s

rcd_serial

rhcs/cisco_ucs

rps10

suicide

wti_mpc

wti_nps

 

 

node1:\~ \# crm ra list stonith

![[SUSE cluster_006_SUSE-fence-idrac_001.png]]

 

添加 idrac 为 fence设备

idrac信息：

Node1:  10.10.101.175

Node2:  10.10.101.75

 

开始添加：[  ]要把node1与node2的idrac都添加进去

node1:\~ \# crm configure primitive fence-node1 stonith:fence_ipmilan params ipaddr=\"10.10.101.175\" login=root passwd=calvin lanplus=1 op monitor interval=60s

 

node1:\~ \# crm configure primitive fence-node2 stonith:fence_ipmilan params ipaddr=\"10.10.101.75\" login=root passwd=calvin lanplus=1 op monitor interval=60s

 

![[SUSE cluster_006_SUSE-fence-idrac_002.png]]

 

 

双节点集群是一种特殊的集群。尽管隔离配置正确，但它永远不会达到法定人数。因此，其默认策略应适应这一事实。

node1:\~ \# crm configure property no-quorum-policy=ignore

 

配置fence后需要打开 STONITH（或隔离）了：

node1:\~ \# crm configure property stonith-enabled=true

 

 

node1:\~ \# stonith_admin -l node1

fence-node2

fence-node1

2 fence devices found

 

node1:\~ \# stonith_admin -l node2

fence-node1

fence-node2

2 fence devices found

 

定义了一个当出现问题时所先试用fence设备，现用sbd，然后再用其它

node1:\~ \# crm configure fencing_topology stonith-sbd,fence-node1 fence-node2

 

删除这个定义

node1:\~ \# crm configure delete  fencing_topology

 

成功fence：

node1:\~ \# crm node fence node2.abc.com

Fencing node2.abc.com will shut down the node and migrate any resources that are running on it! Do you want to fence node2.abc.com (y/n)?

node1:\~ \#

 

 

》》查看fence 配置信息：

![[SUSE cluster_006_SUSE-fence-idrac_003.png]]

 

 

》》清除所有历史记录：

Operations 那些就是之前的历史记录

![[SUSE cluster_006_SUSE-fence-idrac_004.png]]

 

\# crm_resource \--cleanup

![[SUSE cluster_006_SUSE-fence-idrac_005.png]]

 

 

 

已使用 OneNote 创建。
