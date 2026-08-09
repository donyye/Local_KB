\|\_\_RHEL_NVME_Test

2024年4月24日

14:32

三个不同版本的系统，测试的时 NVME盘，这盘时直接插在 PCIE 上的，性能表现都不同。

 

RHEL8.8

![[CPU-Disk-MEM-Performances_009____RHEL_NVME_Test_001.png]]

 

 

RHEL7.8

![[CPU-Disk-MEM-Performances_009____RHEL_NVME_Test_002.png]]

 

 

RHEL 9.3

![[CPU-Disk-MEM-Performances_009____RHEL_NVME_Test_003.png]]

没看到其它的过程，只有Q2Q的过程。从 iostat 来看也基本没什么 await

 

=================================

其它盘测试

RHEL7.8

在测试 sdc 盘，SSD

 

测试时发现在I2D上花费了大量时间，所有调整调度测试。

 

\# echo noop \> /sys/block/sdc/queue/scheduler

\# cat /sys/block/sdc/queue/scheduler

\[noop\] deadline cfq

 

修改完调度算法后在 I2D 所消耗的百分比要比没修改的时少10%多，有所提升。而通过iostat 查看 await 下降了20 左右。原来是100\~60，调整后 40多。不能说很好，有改善。

对比图：

![[CPU-Disk-MEM-Performances_009____RHEL_NVME_Test_004.png]]

 

SCHED 那项就是调度算法

![[CPU-Disk-MEM-Performances_009____RHEL_NVME_Test_005.png]]

或者 grep -H . /sys/block/sd\*/queue/scheduler 查看

 

 

 

已使用 OneNote 创建。
