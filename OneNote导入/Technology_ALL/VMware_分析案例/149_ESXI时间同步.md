ESXI时间同步

2022年11月22日

12:58

 

1. 检查端口

[\[root@localhost:\~\] nc -vz -u 10.10.40.7]4 123

Connection to 10.10.40.75 123 port \[udp/ntp\] succeeded!

 

2. 获取管理网络地址

\[root@localhost:\~\] esxcfg-vmknic -l

Interface  Port Group/DVPort/Opaque Network        IP Family IP Address                              Netmask         Broadcast       MAC Address       MTU     TSO MSS   Enabled Type                NetStack            

vmk0       Management Network                      IPv4      10.10.40.92                             255.255.0.0     10.10.255.255   00:50:56:89:37:c2 1500    65535     true    STATIC              defaultTcpipStack   

......

 

3. tcpdump 抓包

pktcap-uw \--vmk vmk0 \--dir 2 \--dstip 10.10.40.xx -o - \|tcpdump-uw -enr -

例子1，向windows AD同步，使用的NTPv3协议

![[Technology_ALL_VMware_分析案例_149_ESXI时间同步_001.png]]

 

例子2，向Linux同步，使用的是Ntpv4

![[Technology_ALL_VMware_分析案例_149_ESXI时间同步_002.png]]

 

 

4. 因为ESXI向Windows AD同步时间使用的V3 协议，所以需要手动配置。

\# vi /tmp/ntpconfig.txt

server 10.10.40.79 version 3

server 10.10.40.212 version 3

tos maxdist 30

 

\# esxcli system ntp set -f /tmp/ntpconfig.txt

\# esxcli system ntp set -e 1

\# esxcli system ntp config get

Command

\-\-\-\-\-\--

tos maxdist 30[  \--\> ]延时

restrict default nomodify notrap nopeer noquery

restrict 127.0.0.1

restrict -6 ::1

driftfile /etc/ntp.drift

logconfig +clockstatus +peerstatus +sysstatus +syncstatus

server 10.10.40.79

server 10.10.40.212

 

 

5. 使用命令检查

[\[root@localhost:\~\] esxcli system ntp test]

   Comments: Service analysis started on host: localhost, Test started at: 2022-11-22T04:57:21Z, Time Service is administratively enabled., Verifying NTP service., NTP server: 10.10.40.79 resolves IPv4: 10.10.40.79, Virtual NIC vmk0 : Admin: Up, IP Interface: vmk0 IPv4 Address: STATIC 10.10.40.91, IP Interface: vmk0 connected to Management Network on vSwitch0, IP Network Stack: defaultTcpipStack, Physical NIC vmnic0 : Admin: Up Oper: Up, Physical NIC vmnic3 : Admin: Up Oper: Up, Firewall Rule: ntpClient allows traffic on port: 123, ntpd is running, PID: 534855, Kernel clock type: ntp, NTP is in sync , Peering with: 10.10.40.79, Statum: 4, Accuracy to within: 39.598000 msecs, Polling every: 10 secs, Network delay round trip: 26.816000 msecs, Difference from remote clock: 0.778472 msecs, Service analysis completed.

[   ]Timeinsync: true[  ]\--\> 说明是同步的

\# 有出现过时间偏移大而导致了没这么快可以同步，需要等待比较长的时间。

 

 

 # ntpd -p[   ]也可以初步验证一下。 

![[Technology_ALL_VMware_分析案例_149_ESXI时间同步_003.jpg]]

 

 

![[Technology_ALL_VMware_分析案例_149_ESXI时间同步_004.jpg]]

 

reach 0 是代表没传输,其它值都说明有传输

如果添加两个NTP地址是会轮巡两个NTP server来修正时间。

 

 

 

 

已使用 OneNote 创建。
