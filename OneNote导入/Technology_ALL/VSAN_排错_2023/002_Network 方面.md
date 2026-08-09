Network 方面

2022年9月19日

15:59

》列出所有虚拟交换机

esxcfg-vswitch -l

》列出所有端口组

esxcfg-vmknic -l

》查看网口是否up

esxcfg-nics -l

 

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

 

 

 

 

<https://support.lenovo.com/us/en/solutions/ht509737-the-esxi-shell-commands-and-log-files-needed-to-troubleshoot-object-issues-in-thinkagile-vx>

 

<https://support.lenovo.com/gb/zc/solutions/ht509717>

 

 

已使用 OneNote 创建。
