ESXI抓vm的包

2023年4月10日

14:02

RHEL7

IP ： 10.10.40.171

Switch port:16772191   \|  

MC: 00:50:56:ad:7e:0e  \| 00:50:56:89:cb:fc

 

pktcap-uw \--switchport 167772191 \--capture VnicTx \--ethtype 0x0806 -o - \| tcpdump-uw -enr -

pktcap-uw \--uplink vmnic2 \--capture PortOutput \--enhtype 0x0806 -o - \| tcpdump-uw -enr -

pktcap-uw \--uplink vmnic2 \--capture UplinkSndKernel \--enhtype 0x0806 -o - \| tcpdump-uw -enr -

pktcap-uw \--uplink vmnic2 \--capture UplinkRcvKernal \--enhtype 0x0806 -o - \| tcpdump-uw -enr -

 

pktcap-uw \--switchport 167772191 \--capture VnicTx \--ethtype 0x0806 -o - \| tcpdump-uw -enr -

pktcap-uw \--uplink vmnic2 \--capture PortOutput \--srcmac 00:50:56:ad:7e:0e -o - \| tcpdump-uw -enr -

pktcap-uw \--uplink vmnic2 \--capture UplinkSndKernel \--srcmac 00:50:56:ad:7e:0e -o - \| tcpdump-uw -enr -

 

 

 

 

============

arping -I eth0

 

tcpdump -i eth1 -nn arp

 

ip neigh   \## check current ARP tables

 

ping -I eth1 -M do -s 8972 x.x.x.x  \## \"-M  do\": prohibit fragmentation

 

 

 

 

rp_filter = 0

 

firewall-cmd \--add-service=http

firewall-cmd \--list-all

 

nft insert rule inet firewalld fileter_FORWARD iifname  accept

 

echo \> /dev/tcp/x.x.x.x/80 && echo OK

 

已使用 OneNote 创建。
