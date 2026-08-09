- RHEL9 team0 & br0

2024年8月8日

13:36

\
配置Team：[\
\[root@localhost \~\]# nmcli con add con-name team0 ifname team0 type team config \'{\"runner\":}\']

Connection \'team0\' (b91e4b33-8c3e-494f-9773-3289d4288caf) successfully added.

 

可以不设置IP

[\[root@localhost \~\]# nmcli con mod team0 ipv4.addresses \"10.10.41.152/16\" ipv4.method manual autoconnect yes]

 

Team slave

[\[root@localhost \~\]# nmcli con add con-name team0-port1 ifname ens35 type team-slave master team0 ]

Connection \'team0-port1\' (2c0f4b4f-d0e3-443f-889b-833d4c37de91) successfully added.

 

[\[root@localhost \~\]# nmcli con add con-name team0-port2 ifname ens36 type team-slave master team0 ]

Connection \'team0-port2\' (58c7464f-6e6f-4da4-8486-075fd7c2e2d6) successfully added.

 

启动team

[\[root@localhost \~\]# nmcli con up team0]

Connection successfully activated (master waiting for slaves) (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/7)

 

查看

![[KVM_004_- RHEL9 team0 & br0_001.png]]

 

 

添加桥接

[\[root@localhost \~\]# nmcli con add type bridge con-name bridge0 ifname br0 ipv4.method manual ipv4.addresses 10.10.41.153/16 autoconnect yes ]

Connection \'bridge0\' (3739bc6a-98a6-479a-99a7-be5bd5c0c045) successfully added.

 

桥接 team0 到 

[\[root@localhost \~\]# nmcli con modify team0 master br0]

 

查看

![[KVM_004_- RHEL9 team0 & br0_002.png]]

 

 

Vm 方面的设置：

![[KVM_004_- RHEL9 team0 & br0_003.png]]

 

 

已使用 OneNote 创建。
