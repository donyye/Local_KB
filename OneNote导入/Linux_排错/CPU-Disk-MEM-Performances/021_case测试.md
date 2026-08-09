case测试

2024年4月19日

11:03

dd if=/dev/urandom of=Test_data bs=1M count=60960

 

echo 3 \> /proc/sys/vm/drop_caches; /usr/bin/cp -arf Test_data /mnt/nvme01/

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

#!/bin/bash

 

while true

do

    /usr/bin/cp -arf /mnt/data/Test_data /mnt/nvme01/

    sleep 1

    echo 3 \> /proc/sys/vm/drop_caches

[    sleep 6  \# ]可以调整循环之间的时间间隔

done

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

iostat -zxmd 1

 

![[CPU-Disk-MEM-Performances_021_case测试_001.png]]

 

![[CPU-Disk-MEM-Performances_021_case测试_002.png]]

 

 

 

关闭大页：

\[root@localhost \~\]# echo never \> /sys/kernel/mm/transparent_hugepage/enabled

\[root@localhost \~\]# cat /sys/kernel/mm/transparent_hugepage/enabled

always madvise \[never\]

 

\[root@localhost \~\]# echo never \> /sys/kernel/mm/transparent_hugepage/defrag

\[root@localhost \~\]# cat /sys/kernel/mm/transparent_hugepage/defrag

always madvise \[never\]

 

默认是 都是 always

 

![[CPU-Disk-MEM-Performances_021_case测试_003.png]]

 

 

 

 

 

 

 

 

 

RHEL8.8

 

![[CPU-Disk-MEM-Performances_021_case测试_004.png]]

 

 

 

![[CPU-Disk-MEM-Performances_021_case测试_005.png]]

 

 

 

\[root@localhost \~\]# cat /sys/kernel/mm/transparent_hugepage/enabled

\[always\] madvise never

\[root@localhost \~\]# cat /sys/kernel/mm/transparent_hugepage/defrag

always defer defer+madvise \[madvise\] never

\[root@localhost \~\]#

\[root@localhost \~\]# echo never \> /sys/kernel/mm/transparent_hugepage/enabled

\[root@localhost \~\]# echo never \> /sys/kernel/mm/transparent_hugepage/defrag

\[root@localhost \~\]#

\[root@localhost \~\]# cat /sys/kernel/mm/transparent_hugepage/enabled

always madvise \[never\]

\[root@localhost \~\]# cat /sys/kernel/mm/transparent_hugepage/defrag

always defer defer+madvise madvise \[never\]

 

 

![[CPU-Disk-MEM-Performances_021_case测试_006.png]]

 

 

 

已使用 OneNote 创建。
