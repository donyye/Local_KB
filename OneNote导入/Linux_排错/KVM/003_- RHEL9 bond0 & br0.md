\- RHEL9 bond0 & br0

2024年8月5日

14:21

FYI ：[https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/networking_guide/sec-vlan_on_bond_and_bridge_using_the_networkmanager_command_line_tool_nmcli#sec-VLAN_on_Bond_and_Bridge_Using_the_NetworkManager_Command_Line_Tool_nmcli](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/networking_guide/sec-vlan_on_bond_and_bridge_using_the_networkmanager_command_line_tool_nmcli#sec-VLAN_on_Bond_and_Bridge_Using_the_NetworkManager_Command_Line_Tool_nmcli)

 

 

RHEL9.4 \[成功配置[\]]

\
配置bond0：

\# nmcli connection add type bond con-name bond0 ifname bond0 bond.options \"mode=active-backup,miimon=100\" ipv4.method disabled ipv6.method ignore

 

\# nmcli con add type ethernet con-name Slave1-bond0 ifname eno8303 master bond0 slave-type bond

 

\# nmcli con add type ethernet con-name Slave2-bond0 ifname eno8403 master bond0 slave-type bond

 

添加桥接br0:

\# nmcli con add type bridge con-name bridge0 ifname br0 ipv4.method manual ipv4.addresses 10.10.41.156/16 autoconnect yes

 

\# nmcli connection modify bond0 master br0

 

查看：

![[KVM_003_- RHEL9 bond0 & br0_001.png]]

 

VM配置桥接：

![[KVM_003_- RHEL9 bond0 & br0_002.png]]

 

 

查看配置文件：

[\[root@localhost \~\]# cd] /etc/NetworkManager/system-connections

 

[\[root@localhost system-connections\]# ll]

total 40

-rw\-\-\-\-\-\--. 1 root root 230 Aug  7 14:31 bond0.nmconnection

-rw\-\-\-\-\-\--. 1 root root 217 Aug  7 14:30 bridge0.nmconnection

-rw\-\-\-\-\-\--. 1 root root 204 Aug  6 10:54 eno12399.nmconnection

-rw\-\-\-\-\-\--. 1 root root 204 Aug  6 10:54 eno12409.nmconnection

-rw\-\-\-\-\-\--. 1 root root 245 Aug  6 10:54 enp0s20f0u14u3.nmconnection

-rw\-\-\-\-\-\--. 1 root root 206 Aug  6 10:54 ens3f0np0.nmconnection

-rw\-\-\-\-\-\--. 1 root root 206 Aug  6 10:54 ens3f1np1.nmconnection

-rw\-\-\-\-\-\--. 1 root root 232 Aug  6 10:54 ibp152s0.nmconnection

-rw\-\-\-\-\-\--. 1 root root 194 Aug  7 14:23 Slave1-bond0.nmconnection

-rw\-\-\-\-\-\--. 1 root root 194 Aug  7 14:24 Slave2-bond0.nmconnection

 

[\[root@localhost system-connections\]# cat bond0.nmconnection ]

\[connection\]

id=bond0

uuid=22a0824e-4b02-474d-836c-5c7f8f6ba21b

type=bond

controller=br0

interface-name=bond0

master=br0

port-type=bridge

slave-type=bridge

timestamp=1723011763

 

\[bond\]

miimon=100

mode=active-backup

 

\[bridge-port\]

 

 

[\[root@localhost system-connections\]# cat bridge0.nmconnection ]

\[connection\]

id=bridge0

uuid=1feefa5d-fb64-449e-b9ac-69b25666eddb

type=bridge

interface-name=br0

 

\[ethernet\]

 

\[bridge\]

 

\[ipv4\]

address1=10.10.41.156/16

method=manual

 

\[ipv6\]

addr-gen-mode=default

method=auto

 

\[proxy\]

 

 

[\[root@localhost system-connections\]# cat Slave1-bond0.nmconnection ]

\[connection\]

id=Slave1-bond0

uuid=cac3023f-996b-4682-912c-6ce1cb57e39d

type=ethernet

controller=bond0

interface-name=eno8303

master=bond0

port-type=bond

slave-type=bond

 

\[ethernet\]

 

\[bond-port\]

 

 

[\[root@localhost system-connections\]# cat Slave2-bond0.nmconnection ]

\[connection\]

id=Slave2-bond0

uuid=2e7e56e5-79e3-49af-900f-01e6c66f85c0

type=ethernet

controller=bond0

interface-name=eno8403

master=bond0

port-type=bond

slave-type=bond

 

\[ethernet\]

 

\[bond-port\]

 

 

=========================================

使用 team：\
如果您使用 team 代替 bond：\
nmcli con add type team ifname team0\
nmcli con add type team-slave ifname eth0 master team0\
nmcli con add type team-slave ifname eth1 master team0\
nmcli con add type bridge ifname br0\
nmcli con mod team0 connection.master br0 connection.slave-type bridge

明确指定 port-type：\
确保 bond0 和 br0 的配置文件明确指定了适当的 port-type。例如：

nmcli con mod bond0 connection.port-type bridge-port\
nmcli con mod br0 connection.port-type bridge

然后重新尝试设置：

nmcli con mod bond0 connection.master br0 connection.slave-type bridge

通过这些步骤，您应该能够解决 RHEL 9.4 中使用 nmcli 配置网络连接时遇到的问题。

 

 

 

 

 

 

已使用 OneNote 创建。
