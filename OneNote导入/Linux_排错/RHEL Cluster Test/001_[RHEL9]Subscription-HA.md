\[RHEL9\]Subscription-HA

2023年12月28日

13:55

\
RHEL9.X\
High Availability Packages

[\[root@localhost \~\]# subscription-manager repos \--enable=rhel-9-for-x86_64-highavailability-rpms]

Repository \'rhel-9-for-x86_64-highavailability-rpms\' is enabled for this system.

![[RHEL Cluster Test_001_[RHEL9]Subscription-HA_001.png]]

 

 

GFS2、lvm2-cluster(clvmd)、cmirror、ctdb 等

[\[root@localhost \~\]# subscription-manager repos \--enable=rhel-9-for-x86_64-resilientstorage-rpms]

Repository \'rhel-9-for-x86_64-resilientstorage-rpms\' is enabled for this system.

![[RHEL Cluster Test_001_[RHEL9]Subscription-HA_002.png]]

 

 

 

RHEL8.X

subscription-manager repos \--enable=rhel-8-for-x86_64-highavailability-rpms

 

 

 

 

已使用 OneNote 创建。
