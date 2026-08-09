OOM与Numa

2022年7月5日

10:29

[ ]从下面的日志可以看出，当出现了OOM的时候，Numa 0 是空闲的内存也只有20KB，而 Numa 1 的空闲内存还有61G，说明 Numa 1 的内存根本没有被使用到。

 

 

Jul 23 15:04:28 ZX-JZ-5GCDBS04 kernel: sds-2.5.0.254 invoked oom-killer: gfp_mask=0x200da, order=0, oom_score_adj=0

    Jul 23 15:04:28 ZX-JZ-5GCDBS04 kernel: sds-2.5.0.254 cpuset=/ mems_allowed=0-1

    \-\-\--

    Jul 23 15:04:28 ZX-JZ-5GCDBS04 kernel: Node 0 Normal free:54236kB min:43840kB low:54800kB high:65760kB active_anon:20kB inactive_anon:48kB active_file:4kB inactive_file:32kB

    unevictable:0kB isolated(anon):0kB isolated(file):0kB present:65011712kB managed:63937168kB mlocked:0kB dirty:4kB writeback:0kB mapped:4kB shmem:0kB slab_reclaimable:28328kB slab_unreclaimable:93712kB kernel_stack:8192kB pagetables:6592kB unstable:0kB bounce:0kB free_pcp:2300kB local_pcp:0kB free_cma:0kB writeback_tmp:0kB pages_scanned:2068 all_unreclaimable? yes

    Jul 23 15:04:28 ZX-JZ-5GCDBS04 kernel: lowmem_reserve\[\]: 0 0 0 0

    Jul 23 15:04:28 ZX-JZ-5GCDBS04 kernel: Node 1 Normal free:64780040kB min:45276kB low:56592kB high:67912kB active_anon:179108kB inactive_anon:61560kB active_file:102404kB

                                                                  \\\_\_\_\_\_\_\_ 61 GiB free

 

    inactive_file:147172kB unevictable:0kB isolated(anon):0kB isolated(file):0kB present:67108864kB managed:66029944kB mlocked:0kB dirty:0kB writeback:0kB mapped:86412kB shmem:66868kB slab_reclaimable:33576kB slab_unreclaimable:116076kB kernel_stack:10224kB pagetables:6340kB unstable:0kB bounce:0kB free_pcp:392kB local_pcp:96kB free_cma:0kB writeback_tmp:0kB pages_scanned:0 all_unreclaimable? No

 

 

一些建议：

\- 运行 numad 以跨节点分配负载。

 - 您可以选择启用"/proc/sys/vm/zone_reclaim_mode"， 但那是在稍后阶段，在调整时。

<http://linux.laoqinren.net/kernel/vm-sysctl-zone_reclaim_mode/>

zone_reclaim_mode是用来控制内存zone回收模式，在内存分配中，用来管理当一个内存区域内部的内存耗尽时，是从其内部进行内存回收来满足分配还是直接从其它内存区域中分配内存。

\# echo 0 \> /proc/sys/vm/zone_reclaim_mode

\# \# 意味着关闭zone_reclaim模式，可以从其他zone或NUMA节点回收内存

\# echo 1 \> /proc/sys/vm/zone_reclaim_mode

\# \# 表示打开zone_reclaim模式，这样内存回收只会发生在本地节点内

\# echo 2 \> /proc/sys/vm/zone_reclaim_mode

\# \# 在本地回收内存时，可以将cache中的脏数据写回硬盘，以回收内存。

\# echo 4 \> /proc/sys/vm/zone_reclaim_mode

\# \# 可以用swap方式回收内存。

详细的一定要看连接。

 

 

 

 

已使用 OneNote 创建。
