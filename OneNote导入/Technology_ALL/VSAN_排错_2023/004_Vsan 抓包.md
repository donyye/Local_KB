Vsan 抓包

2022年9月19日

16:02

1.确认vsan网络走的那个vmk

\[root@localhost:\~\] esxcfg-vmknic -l

\...\... 

vmk1       VMkernel-vsan                           IPv4      192.168.200.91                          255.255.255.0   192.168.200.255 00:50:56:61:d3:72 1500    65535     true    STATIC              defaultTcpipStack

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

[03:42:06.894346 00:50:56:6b:ea:4e \> 00:50:56:61:d3:72, ethertype IPv4 (0x0800), length 103: 192.168.200.92.22186 \> 192.168.200.91.8182: Flags \[P.\], seq 2870682074:2870682111, ack 4291430055, win 130, options \[nop,nop,TS val 1968807228 ecr 2120455463\], length 37]

03:42:06.894347 00:50:56:64:46:d9 \> 00:50:56:61:d3:72, ethertype IPv4 (0x0800), length 103: 192.168.200.93.49949 \> 192.168.200.91.8182: Flags \[P.\], seq 2070844952:2070844989, ack 337100753, win 130, options \[nop,nop,TS val 178995850 ecr 2751848539\], length 37

\...\...

\-\-\-\-\-\-\-- 正常 \-\-\-\-\-\-\--

3.查看vmk1端口情况

查看node3(10.10.40.93)的vmk1的 MAC地址和MTU

![[Technology_ALL_VSAN_排错_2023_004_Vsan 抓包_001.jpg]]

在node2(10.10.40.92) 上运行 PortOutput 可以看到收到 node3包，而MAX地址是正确的，说明正常。

03:51:52.563286 00:50:56:64:46:d9[ \> 00:50:56:6b:ea:4e, ethertype IPv4 (0x0800), length 234: 192.168.200.93.22602 \> 192.168.200.92.2233: Flags \[P.\], seq 168:336, ack 1321, win 4097, options \[nop,nop,TS val 190493955 ecr 1096736499\], length 168]

 

 

 

已使用 OneNote 创建。
