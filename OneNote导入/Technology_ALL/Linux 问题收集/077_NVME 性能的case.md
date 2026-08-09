NVME 性能的case 

2022年6月16日

14:57

 

\[14:44\] Zhang, Ji Fu

 

大家遇到H755n 接NVME 性能的case ， 如下信息参考下

 

我遇到一个H755n 4块NVME raid 0/5, 安装redhat 8.3 的OS , 随机读IOPS 只有400\~500K， 硬件方面如何调整都上不去, 后面是系统调整一些包括I/O 调度在内的参数就能测到 3000K了

 

echo none \> /sys/block/sdb/queue/scheduler

echo 0 \> /sys/block/sdb/queue/add_random

echo 2 \> /sys/block/sdb/queue/nomerges

echo 2048 \> /sys/block/sdb/queue/nr_requests

echo 0 \> /sys/block/sdb/queue/rotational

echo 2 \> /sys/block/sdb/queue/rq_affinity

echo 1024 \> /sys/block/sdb/queue/max_sectors_kb

echo 1024 \> /sys/block/sdb/device/queue_depth

 

 

 

还有就是出于设计， 如果一个H755n 接8块盘同时测试，总吞吐不会超过16GB/s ， 8块盘单盘同时测试， 单盘不会超过2GB/s . 如果同时测试只测一块，可以达到6GB/s ， 跳着4块盘同时测试也可以达到单盘6GB/s

 

![[Technology_ALL_Linux 问题收集_077_NVME 性能的case_001.png]]

 

 

 

已使用 OneNote 创建。
