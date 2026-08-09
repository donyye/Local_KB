RHEL8 thin lvm

2023年2月20日

17:29

RHEL8.7 上测试

 

[\[root@localhost \~\]# pvcreate /dev/sdb ]

  Physical volume \"/dev/sdb\" successfully created.

 

[\[root@localhost \~\]# vgcreate myvg /dev/sdb ]

  Volume group \"myvg\" successfully created

 

[\[root@localhost \~\]# vgdisplay ]

  \-\-- Volume group \-\--

  VG Name               myvg

  System ID             

  Format                lvm2

  Metadata Areas        1

  Metadata Sequence No  1

  VG Access             read/write

  VG Status             resizable

  MAX LV                0

  Cur LV                0

  Open LV               0

  Max PV                0

  Cur PV                1

  Act PV                1

  VG Size               \<10.00 GiB

  PE Size               4.00 MiB

  Total PE              2559

  Alloc PE / Size       0 / 0   

  Free  PE / Size       2559 / \<10.00 GiB

  VG UUID               Lhd3Ax-DrIn-eXTx-5Wps-X5Mp-2w7y-t9tf2N

 

使用所有的空间来创建精简池

[\[root@localhost \~\]# lvcreate -l +100%free \--thinpool data_pool myvg]

  Thin pool volume with chunk size 64.00 KiB can address at most \<15.88 TiB of data.

  Logical volume \"data_pool\" created.

 

 

创建了两个精简卷 Thin volume

但是实践上空间只有10G，而每个精简卷的大小时50G，超分配，有下面警告。

[\[root@localhost \~\]# lvcreate -V 50G \--thin -n thin_lv_data01 myvg/data_pool]

  WARNING: Sum of all thin volume sizes (50.00 GiB) exceeds the size of thin pool myvg/data_pool and the size of whole volume group (\<10.00 GiB).

  WARNING: You have not turned on protection against thin pools running out of space.

  WARNING: Set activation/thin_pool_autoextend_threshold below 100 to trigger automatic extension of thin pools before they get full.

  Logical volume \"thin_lv_data01\" created.

 

[\[root@localhost \~\]# lvcreate -V 50G \--thin -n thin_lv_data02 myvg/data_pool]

  WARNING: Sum of all thin volume sizes (100.00 GiB) exceeds the size of thin pool myvg/data_pool and the size of whole volume group (\<10.00 GiB).

  WARNING: You have not turned on protection against thin pools running out of space.

  WARNING: Set activation/thin_pool_autoextend_threshold below 100 to trigger automatic extension of thin pools before they get full.

  Logical volume \"thin_lv_data02\" created.

 

通过lvs命令可以看到两个精简卷的大小，和时间pool的大小。

![[Technology_ALL_Linux 问题收集_087_RHEL8 thin lvm_001.png]]

 

![[Technology_ALL_Linux 问题收集_087_RHEL8 thin lvm_002.png]]

 

 

配置 /etc/lvm/lvm.conf

thin_pool_autoextend_threshold = 70

thin_pool_autoextend_percent = 20

#这意味着，只要池使用率超过70％，它就会再扩展20％。

 

thin_pool_autoextend_threshold = 100

#设置为100将禁用自动扩展，默认设置表明该功能已被禁用。

 

 

 

 

》》测试1，手动添加一个10G的分区后来做thin pool的手动扩容

pvcreate myvg /dev/sdc1

vgextend myvg /dev/sdc1

 

手动扩容池

[\[root@localhost \~\]# lvextend -L +2G myvg/data_pool]

  Size of logical volume myvg/data_pool_tdata changed from 9.97 GiB (2553 extents) to 11.97 GiB (3065 extents).

  WARNING: Sum of all thin volume sizes (100.00 GiB) exceeds the size of thin pool myvg/data_pool and the size of whole volume group (19.99 GiB).

  Logical volume myvg/data_pool successfully resized.

 

手动扩容卷

[\[root@localhost \~\]# lvextend -L +2G myvg/thin_lv_data01]

  Size of logical volume myvg/thin_lv_data01 changed from 50.00 GiB (12800 extents) to 52.00 GiB (13312 extents).

  WARNING: Sum of all thin volume sizes (102.00 GiB) exceeds the size of thin pool myvg/data_pool and the size of whole volume group (19.99 GiB).

  Logical volume myvg/thin_lv_data01 successfully resized.

 

手动扩容后

![[Technology_ALL_Linux 问题收集_087_RHEL8 thin lvm_003.png]]

 

 

》》测试2，写入15G的数据

目前 lvm 总共时 20G 的空间，而 thin pool 分配是12G的空间

 

[\[root@localhost lvm_thin_test\]# dd if=/dev/zero of=/mnt/lvm_thin_test/15G.txt bs=1G count=15]

15+0 records in

15+0 records out

16106127360 bytes (16 GB, 15 GiB) copied, 40.1704 s, 401 MB/s

 

可以看到空间有自动的增加 LSize 从11.79g 到 17.24g。

Data% 是代表pool空间的使用情况，已经使用了87.15%

![[Technology_ALL_Linux 问题收集_087_RHEL8 thin lvm_004.png]]

 

挂载自动回收空间

\# mount -o discard /dev/myvg/thin_lv_data01 /mnt/lvm_thin_test/

 

手动对空间进行回收

\# fstrim /mnt/lvm_thin_test

![[Technology_ALL_Linux 问题收集_087_RHEL8 thin lvm_005.png]]

 

LSize是指逻辑卷的总容量。

Data%是指逻辑卷当前使用的空间占逻辑卷总容量的百分比。

Meta%是指逻辑卷中用于存储元数据的空间占逻辑卷总容量的百分比。

 

 

 

 

已使用 OneNote 创建。
