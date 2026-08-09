06: iscsi-share

2023年9月11日

12:17

发现 iscsi 共享\
iscsiadm -m discovery -t sendtargets -p 172.17.33.3

 

挂载iscsi共享

iscsiadm -m node -T iqn.2011-linux-disk1 -p 172.17.33.3 -l

 

解除挂载iscsi共享

iscsiadm -m node -T qn.2011-linux-disk1 -p 172.17.33.3 -u

 

彻底删除已连接的iSCSI存储设备：

service iscsi stop

rm -rf /var/lib/iscsi/nodes/\*

rm -rf /var/lib/iscsi/send_targets/\*

service iscsi start

 

修改访问时的超时时间，登录和连接时间。

\[root@node1 \~\]# sed -i \'/time/s/=.\*\$/= 2/\' /etc/iscsi/iscsid.conf

\[root@node2 \~\]# sed -i \'/time/s/=.\*\$/= 2/\' /etc/iscsi/iscsid.conf

\[root@node3 \~\]# sed -i \'/time/s/=.\*\$/= 2/\' /etc/iscsi/iscsid.conf

 

查看已发现的 iscsi

\# iscsiadm -m node

 

 

使用fstab挂载iscsi时需要加上 "\_netdev"。

 

已使用 OneNote 创建。
