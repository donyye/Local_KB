01: Ip link 配置网络

2023年7月11日

15:11

使用 ip link 配置 bond

ip link set dev em1 down

ip link set dev em2 down

ip link add bond0 type bond

ip link set bond0 type bond miimon 100 mode 1

ip link set em1 master bond0

ip link set em2 master bond0

ip link set bond0 up

 

ip link add link bond0 name bond0.18 type vlan id 18

ifconfig bond0.18 10.175.0.14 netmask 255.255.255.0 up

ip link set bond0.18 up

=======

如果要在bond0之上再加一层bridge 的话

ip link set dev em1 down

ip link set dev em2 down

ip link add bond0 type bond

ip link add bond0 type bond

ip link set bond0 type bond miimon 100 mode 1

ip link set em1 master bond0

ip link set em2 master bond0

ip link set bond0 up

ip link add br0 type bridge

ip link set dev br0 up

ip link set dev bond0 master br0

ip link ls

ip link add link br0 name br0.18 type vlan id 18

ifconfig br0.18 10.175.0.14 netmask 255.255.255.0 up

ifconfig br0.18

ip link ls

ip route add 0.0.0.0/0 via 10.175.0.254

 

已使用 OneNote 创建。
