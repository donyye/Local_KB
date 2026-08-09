\[suse\]network.txt

Wednesday, October 28, 2015

2:05 PM

#==\[ Verification \]=================================#

\# rpm -V sysconfig-0.71.61-0.11.12

\--

#==\[ Command \]======================================#

\# /sbin/chkconfig network \--list

\--

#==\[ Command \]======================================#

\# /etc/init.d/network status

\--

#==\[ Command \]======================================#

\# /sbin/chkconfig nscd \--list

\--

#==\[ Command \]======================================#

\# /etc/init.d/nscd status

\--

#==\[ Command \]======================================#

\# /sbin/ifconfig -a

\--

#==\[ Command \]======================================#

\# /sbin/ip addr

\--

#==\[ Command \]======================================#

\# /sbin/ip route

\--

#==\[ Command \]======================================#

\# /sbin/ip -s link

\--

#==\[ Command \]======================================#

\# /usr/sbin/hwinfo \--netcard

\--

#==\[ Configuration File \]===========================#

\# /proc/sys/net/ipv4/ip_forward

\--

#==\[ Configuration File \]===========================#

\# /etc/HOSTNAME

\--

#==\[ Configuration File \]===========================#

\# /etc/services

\--

#==\[ Command \]======================================#

\# /bin/hostname

\--

#==\[ Command \]======================================#

\# /bin/ping -n -c1 -W1 127.0.0.1

\--

#==\[ Command \]======================================#

\# /bin/ping -n -c1 -W1 127.0.0.2

\--

#==\[ Command \]======================================#

\# /bin/ping -n -c1 -W1 192.168.100.120

\--

#==\[ Command \]======================================#

\# /bin/ping -n -c1 -W1 172.20.101.35

\--

#==\[ Command \]======================================#

\# /bin/ping -n -c1 -W1 172.20.101.253

\--

#==\[ Command \]======================================#

\# /bin/ping -n -c1 -W1 172.20.20.2

\--

#==\[ Command \]======================================#

\# /bin/ping -n -c1 -W1 172.20.20.10

\--

#==\[ Command \]======================================#

\# /sbin/route

\--

#==\[ Command \]======================================#

\# /sbin/route -n

\--

#==\[ Command \]======================================#

\# /bin/netstat -as

\--

#==\[ Command \]======================================#

\# /bin/netstat -nlp

\--

#==\[ Command \]======================================#

\# /bin/netstat -nr

\--

#==\[ Command \]======================================#

\# /bin/netstat -i

\--

#==\[ Command \]======================================#

\# /sbin/arp -v

\--

#==\[ Command \]======================================#

\# /sbin/brctl show

\--

#==\[ Command \]======================================#

\# /sbin/brctl showmacs bridge

\--

#==\[ Command \]======================================#

\# /sbin/ethtool em1

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -k em1

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -i em1

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -S em1

\--

#==\[ Command \]======================================#

\# mii-tool -v em1

\--

#==\[ Command \]======================================#

\# /sbin/ethtool em2

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -k em2

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -i em2

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -S em2

\--

#==\[ Command \]======================================#

\# mii-tool -v em2

\--

#==\[ Command \]======================================#

\# /sbin/ethtool em3

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -k em3

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -i em3

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -S em3

\--

#==\[ Command \]======================================#

\# mii-tool -v em3

\--

#==\[ Command \]======================================#

\# /sbin/ethtool em4

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -k em4

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -i em4

\--

#==\[ Command \]======================================#

\# /sbin/ethtool -S em4

\--

#==\[ Command \]======================================#

\# mii-tool -v em4

\--

#==\[ Command \]======================================#

\# /usr/sbin/nscd -g

\--

#==\[ Configuration File \]===========================#

\# /etc/hosts

\--

#==\[ Configuration File \]===========================#

\# /etc/host.conf

\--

#==\[ Configuration File \]===========================#

\# /etc/resolv.conf

\--

#==\[ Configuration File \]===========================#

\# /etc/nsswitch.conf

\--

#==\[ Configuration File \]===========================#

\# /etc/nscd.conf

\--

#==\[ Configuration File \]===========================#

\# /etc/hosts.allow

\--

#==\[ Configuration File \]===========================#

\# /etc/hosts.deny

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/SuSEfirewall2

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/personal-firewall - File not found

\--

#==\[ Command \]======================================#

\# iptables

\--

#==\[ Command \]======================================#

\# iptables

\--

#==\[ Command \]======================================#

\# iptables

\--

#==\[ Command \]======================================#

\# iptables

\--

#==\[ Command \]======================================#

\# ip6tables

\--

#==\[ Command \]======================================#

\# ip6tables

\--

#==\[ Command \]======================================#

\# ip6tables

\--

#==\[ Command \]======================================#

\# ip6tables

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/proxy

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/network/ifroute-lo

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/network/ifcfg-lo

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/network/config

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/network/ifcfg.template

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/network/ifcfg-em1

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/network/ifcfg-em4

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/network/dhcp

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/network/routes

\--

#==\[ Log File \]=====================================#

\# /var/log/nscd.log - File not found

\--

#==\[ Log File \]=====================================#

\# /var/log/NetworkManager - Last 500 Lines

 

已使用 OneNote 创建。
