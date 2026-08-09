2022年4月21日

13:48

 mpathconf \--enable

 

使用multipath --F 命令清除多路径设备缓存后，再用multipath --v3

 

systemctl restart multipathd.service

 

\[root@rh8a \~\]# multipath -a /dev/sdc

wwid \'36589cfc000000b08f05f64ec1b2f5fc3\' added

 

要使用 multipath 重建 initramfs 文件系统，请执行附带以下选项的 dracut 命令：

\# dracut \--force -H \--add multipath

fdisk -l 后你会发现 /dev/mapper/mpathX 设备

![[Linux-TEDP-2018_2025_Share_for_CTE_016_mpathconf --enable_001.png]]

 

规则：

path_selector [ ]指定用来决定下一个 I/O 操作所使用路径的默认算法。可能的值包括：

service-time 0 将下一组 I/O 发送到具有最短预计服务时间的路径，这是由未处理 I/O 的总量除以每个路径的相对流量决定的。

queue-length 0：将下一组 I/O 发送到具有最少未处理 I/O 请求的路径。

round-robin 0：在路径组中循环每个路径，向每个路径发送同样数量的 I/O。

 

path_grouping_policy 指定用于未指定路径的默认路径分组策略，可能的值包括：

默认值为 failover。

failover：每个优先组群有一个路径。

multibus：所有有效路径在一个优先组群中。

group_by_serial：每个检测到的系列号有一个优先组群。

group_by_prio：每个优先组群有一个路径优先值。优先权根据指定为 global、per-controller 或者 per-multipath 选项的调用程序决定。

 

已使用 OneNote 创建。
