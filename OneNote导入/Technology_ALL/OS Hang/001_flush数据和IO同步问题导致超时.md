flush数据和IO同步问题导致超时

Wednesday, December 04, 2013

2:41 PM

INFO: task blocked for more than 120 seconds.解决办法

INFO: task blocked for more than 120 seconds.

 

INFO: task bash:12543 blocked for more than 120 seconds.

\"echo 0 \> /proc/sys/kernel/hung_task_timeout_secs\" disables this message.

bash          D 0000000000000012     0 12543  12542 0x00000084

ffff880c343b3ce8 0000000000000082 ffff880c343b3d98 ffffffffffffffe9

ffff880c343b3c88 ffffffffa00c9129 ffff880c343f4aa0 0000010100000015

ffff880c343f5058 ffff880c343b3fd8 000000000000fb88 ffff880c343f5058

Call Trace:

[\[\<ffffffffa00c9129\>\] ? ext4_check_acl+0x29/0x90 \[ext4\]]

[\[\<ffffffffa008fbf0\>\] ? ext4_file_open+0x0/0x130 \[ext4\]]

[\[\<ffffffff8150ea05\>\] schedule_timeout+0x215/0x2e0]

[\[\<ffffffff8117e514\>\] ? nameidata_to_filp+0x54/0x70]

[\[\<ffffffff81277379\>\] ? cpumask_next_and+0x29/0x50]

[\[\<ffffffff8150e683\>\] wait_for_common+0x123/0x180]

[\[\<ffffffff81063310\>\] ? default_wake_function+0x0/0x20]

[\[\<ffffffff8150e79d\>\] wait_for_completion+0x1d/0x20]

[\[\<ffffffff8106513c\>\] sched_exec+0xdc/0xe0]

[\[\<ffffffff8118a0a0\>\] do_execve+0xe0/0x2c0]

[\[\<ffffffff810095ea\>\] sys_execve+0x4a/0x80]

[\[\<ffffffff8100b4ca\>\] stub_execve+0x6a/0xc0]

 

 

his is a know bug. By default Linux uses up to 40% of the available memory for file system caching. After this mark has been reached the file system flushes all outstanding data to disk causing all following IOs going synchronous. For flushing out this data to disk this there is a time limit of 120 seconds by default. In the case here the IO subsystem is not fast enough to flush the data withing 120 seconds. This especially happens on systems with a lof of memory.

The problem is solved in later kernels and there is not "fix" from Oracle. I fixed this by lowering the mark for flushing the cache from 40% to 10% by setting "vm.dirty_ratio=10″ in /etc/sysctl.conf. This setting does not influence overall database performance since you hopefully use Direct IO and bypass the file system cache completely.

 

原理：linux会设置40%的可用内存用来做系统cache，当flush数据时这40%内存中的数据由于和IO同步问题导致超时（120s），

 

解决办法：

1）简单讲就是设置在文件 /etc/sysctl.conf中加入 "vm.dirty_ratio=10″ 。

所将40%减小到10%，避免超时。

 

2） 查看服务器 CPU 型号是否 E5 CPU ，如果是参照 my_case 里的方式处理（上海天文台）

 

已使用 OneNote 创建。
