Multipath-targetd

2023年12月14日

17:22

Storage share

 

LVM:

\# pvcreate /dev/sdb 

\# vgcreate vg0 /dev/sdb

\# lvcreate -L 30G -n lv0 vg0

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\# lvcreate -l +100%FREE -n lv0 vg0 使用所有空间

 

Targetd:

\# dnf -y install targetcli

 

\# targetcli /backstores/block create disk0 dev=/dev/mapper/vg0-lv0

\# targetcli /iscsi create iqn.2023-12.com.example.rhel888:disk1

\# targetcli /iscsi /iqn.2023-12.com.example.rhel888:disk1/tpg1/luns create /backstores/disk0

\# targetcli /iscsi/iqn.2023-12.com.example.rhel888:disk1/tpg1/portals create 0.0.0.0

 

\# [ targetcli /iscsi/iqn.2023-12.com.example.rhel888:disk1/tpg1/acls create iqn.1994-05.com.redhat:903185d62cee]

这里需要注意的是acls里添加的是需要连接上你这台机器的 iqn，而不是你自己的。

 

\# targetcli saveconfig

\# systemctl enable target

\# systemctl start target

 

\# targetcli ls

 

Iscsi:

\# iscsiadm -m discovery -t sendtargets -p 10.10.40.89

\# iscsiadm -m node -T iqn.2023-12.com.example.rhel888:disk1 -p 10.10.40.89 -l

 

解除绑定

iscsiadm -m node -T iqn.2023-12.com.example.rhel888:disk1 -p 10.10.40.89 -u

 

\# ls /var/lib/iscsi/send_targets/

10.10.40.89,3260

\# ls /var/lib/iscsi/nodes/

iqn.2023-12.com.example.rhel888:disk1

 

 

 

已使用 OneNote 创建。
