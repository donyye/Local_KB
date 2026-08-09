RHCS display

2017年9月4日

10:45

\<\<RHCS2.pdf\>\>

![Machine generated alternative text: M610 node1 RHEL 6.5 10.10.100.10 M610 node2 RHEL 6.5 10.10.100.20 M915 node3 RHEL 6.5 10.10.100.30 node1.examle.com Iscsi ininiator ricci agent ,ClusterA node2.example.com Iscsi ininiator ricci agent ,ClusterA node3.example.com IscsiTarget Server luci server \[root@node1 \~\]# yum install ricci httpd y \[root@node1 \~\]# echo redhat \|passwd \--stdin ricci \[root@node1 \~\]# /etc/init.d/ricci start;chkconfig ricci on Node2 \[root@node3 \~\]# yum install luci y \[root@node3 \~\]# chkconfig luci on \[root@node3 \~\]# /etc/init.d/luci start luci ](attachments/Linux-TEDP-2018_2025_Day6.RHCS%20display_001_RHCS%20display_001.png)

 

已使用 OneNote 创建。
