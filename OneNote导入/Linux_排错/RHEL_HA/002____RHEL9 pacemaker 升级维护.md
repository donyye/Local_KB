\|\_\_RHEL9 pacemaker 升级维护

2023年5月19日

19:46

 

案例：

客户两个node的 RHEL9 HA，需要升级 pcsd ,然后更新过程。根据RHEL KB 

<https://access.redhat.com/articles/2059253>

 

关闭第node1:

\# pcs node standby r61-ha

\# pcs cluster stop r61-ha

\# pcs cluster disable r61-ha

 

 

![[RHEL_HA_002____RHEL9 pacemaker 升级维护_001.png]]

 

![[RHEL_HA_002____RHEL9 pacemaker 升级维护_002.png]]

 

\# yum clean all

\# yum makecache

 

![[RHEL_HA_002____RHEL9 pacemaker 升级维护_003.png]]

 

 

恢复：

\# pcs cluster enable r61-ha

\# pcs cluster start r61-ha

\# pcs node unstandby r61-ha

![[RHEL_HA_002____RHEL9 pacemaker 升级维护_004.png]]

 

 

 

关闭第node2

Node2 关闭

![[RHEL_HA_002____RHEL9 pacemaker 升级维护_005.png]]

 

 

更新 yum

![[RHEL_HA_002____RHEL9 pacemaker 升级维护_006.png]]

 

更新 pcs

![[RHEL_HA_002____RHEL9 pacemaker 升级维护_007.png]]

 

开启集群

![[RHEL_HA_002____RHEL9 pacemaker 升级维护_008.png]]

 

 

 

已使用 OneNote 创建。
