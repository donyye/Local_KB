vm到vm的抓包

2024年2月26日

14:41

抓包

环境：

ESXI7 ： 10.10.40.92

VM：10.10.40.122

跳板机：10.10.40.9

 

1）通过ESXI 上抓取指定的vm 端口的所有发向特定 IP 地址的网络流量

2）通过跳板机，使用 wireshark 来抓取 vm主机的所有流量。

 

1. 检查vm上所使用的端口号

net-stats -l

![[VMware-排错_ESXI-网络方面_004_vm到vm的抓包_001.png]]

 

 

2. 通过命令在ESXI上发起收集包的命令

\# 这条命令的作用是在 VMware 环境中，从指定的虚拟交换机端口捕获所有发往特定 IP 地址（10.10.40.9）的网络流量，并将这些数据包保存到指定的文件中

pktcap-uw \--switchport 100663335 \--capture VnicTx \--ip 10.10.40.9 -o /vmfs/volumes/FreeNAS_ISCSI/vm-vnicTx.pacp & pktcap-uw \--switchport 100663335 \--capture VnicRx \--ip 10.10.40.9 -o /vmfs/volumes/FreeNAS_ISCSI/vm-vnicRx.pacp &

命令分段解释说明：\
\> pktcap-uw：这是执行网络数据包捕获的命令行工具。

\> \--switchport 100663335 ：这个选项指定了要捕获流量的虚拟交换机上的特定端口号。在这个例子中，端口号是 100663335 。这意味着捕获工具将会监听和捕获通过这个端口的所有网络流量。

\> \--capture VnicTx：这个选项指示 pktcap-uw 工具捕获从虚拟机（VM）到物理网络的传输（Tx）流量。这通常包括虚拟机发出的所有出站数据包。

\> \--capture VnicRx：它会捕获所有进入虚拟机的网络流量，无论这些数据包是从物理网络进入虚拟机，还是从其他虚拟机通过网络交换机。

\> \--ip 10.10.40.9：这个选项用于过滤捕获的数据包，只包括目标 IP 地址为 10.10.40.9 的数据包。这可以帮助缩小捕获范围，只关注特定 IP 地址的流量。

\> -o /vmfs/volumes/datastore1/vm-vnicTx.pacp：这个选项指定了捕获数据包的输出文件的路径和文件名。在这个例子中，捕获的数据包将被保存到 /vmfs/volumes/datastore1 这个数据存储位置，文件名为 vm-vnicTx.pacp。.pacp 是 VMware 网络数据包捕获文件的扩展名。

 

 

停止

kill \$(lsof \|grep pktcap-uw \|awk \'\'\| sort -u)

lsof \|grep pktcap-uw \|awk \'\'\| sort -u

![[VMware-排错_ESXI-网络方面_004_vm到vm的抓包_002.png]]

 

 

 

3. 在跳板机器上通过wireshark来抓包看vm的情况

Test-RH2 的IP 地址是[  10.10.40.122]

![[VMware-排错_ESXI-网络方面_004_vm到vm的抓包_003.png]]

 

 

 

 

 

 

 

 

 

 

已使用 OneNote 创建。
