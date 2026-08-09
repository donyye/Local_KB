|--analysis

星期四, 2025年10月23日

下午 4:42

 

SUSE 给客户的邮件：

2025.10.19:

之前我们给出的解决方案是：

\>\>\>\>\>\>\> 

通过邮件之前的信息，本问题基本上可以明确I/O异常是mdcheck引起的。

问题发生时间和持续事件也和默认的mdcheck配置对应的上。

 

默认启动时间是每周日的凌晨1:00

S155:~ # grep OnCalendar /usr/lib/systemd/system/mdcheck_start.timer

OnCalendar=Sun *-*-* 1:00:00

持续时间是6小时：

S155:~ # grep MDADM_CHECK_DURATION /etc/sysconfig/mdadm

MDADM_CHECK_DURATION="6 hours"

S155:~ #

 

在这个环境中，RAID容量大概是20T，mdcheck还是需要些时间的，如果系统在该时间点负载较高，将花费更多的时间。

 

下面是在此问题中一些可优化的相关参数，以减少对系统性能的影响。

如果有条件，还是建议使用硬件RAID来替代软件RAID，这样RAID本身的检查或管理都有专门的硬件处理，不会额外消耗系统的算力资源，增加系统负担。

 

1. I/O 调度和速率限制

/proc/sys/dev/raid/speed_limit_min

和 /proc/sys/dev/raid/speed_limit_max

这些参数定义了 RAID 阵列的同步和检查操作的最小和最大速率。

通过调整这两个值，你可以控制mdcheck对系统资源的使用。

speed_limit_min设置为较低的值，可以减少mdcheck对系统性能的影响，但会延长检查时间。

speed_limit_max设置为较高的值，可以加快检查速度，但可能会增加对系统的负载。

 

查看当前设置的值（没有调整过就是默认值）

sysctl dev.raid.speed_limit_min

dev.raid.speed_limit_min = 1000

sysctl dev.raid.speed_limit_max

dev.raid.speed_limit_max = 200000

临时修改成您想设定的值，比如

sysctl -w dev.raid.speed_limit_min=500000

dev.raid.speed_limit_min = 500000

sysctl -w dev.raid.speed_limit_max=5000000

dev.raid.speed_limit_max = 5000000

永久修改可以这样：

cat \> /etc/sysctl.d/10-raid-speed.conf \<\< EOF

dev.raid.speed_limit_min = 500000

dev.raid.speed_limit_max = 5000000

EOF

sysctl -p

cat \> /etc/sysctl.d/10-raid-speed.conf \<\< EOF

dev.raid.speed_limit_min = 500000

dev.raid.speed_limit_max = 5000000

EOF

sysctl -p

可以通过查看# cat /proc/mdstat 中的speed=XXX看到变化。

注意：上面的500000和5000000都是示例，请根据需要自行决定。

查看两个参数的文档，并没有范围，所以没有最大可以调到多少。

 

详情可以参考以下网页中的这两个说明sync_min, sync_max

[https://www.kernel.org/doc/html/v4.18/admin-guide/md.html[ [kernel.org]]](https://nam02.safelinks.protection.outlook.com/?url=https%3A%2F%2Furldefense.com%2Fv3%2F__https%3A%2Fwww.kernel.org%2Fdoc%2Fhtml%2Fv4.18%2Fadmin-guide%2Fmd.html__%3B!!LpKI!iiowvmnZKbmm6oDrWlR183PVeyQIWpk1UuxOrNEU45-F_2mqDG8rLqu54z9kwzGJFQDmCQRqPu5TKD5MgpHGKOg7%24&data=05%7C02%7CDony.Ye%40dell.com%7C6d13b91cb980462b293f08de11f19e72%7C945c199a83a24e809f8c5a91be5752dd%7C0%7C0%7C638967927075135071%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=imRUU2rPd9DxFQGfC3m8Q2TLSUQmD%2BdiTEYItyiQvTA%3D&reserved=0)

 

2. 调整检查频率和持续时间，

改变mdcheck的执行频率，比如从每周改为每月，持续时间从6小时改成更长或更短，这样可以在系统负载较低的时间段运行。

执行频率请修改/usr/lib/systemd/system/mdcheck_start.timer中的OnCalendar=Sun *-*-* 1:00:00

持续时间请修改/etc/sysconfig/mdadm中的MDADM_CHECK_DURATION=

比如想设置为季度check一次，可以把上面的OnCalendar=Sun *-*-* 1:00:00 改成 OnCalendar=*-01,04,07,10-01 00:00:00

这样就是每三个月1日00点执行。OnCalenda的格式为：星期 年-月-日 时:分:秒

详情请参阅：# man systemd.time

改成更长比如一年就这样：OnCalendar=*-01-01 00:00:00

 

持续时间改短之后，mdcheck没有完成也会退出，等待下一次唤醒。

 

3. 调整条带缓存

stripe_cache_size（仅适用于RAID 5）

调整条带缓存大小可以提高 RAID 阵列的性能，进而影响mdcheck的运行效率。

示例 echo 16384 \> /sys/block/md0/md/stripe_cache_size

详情参考：

[https://www.kernel.org/doc/html/v4.18/admin-guide/md.html[ [kernel.org]]](https://nam02.safelinks.protection.outlook.com/?url=https%3A%2F%2Furldefense.com%2Fv3%2F__https%3A%2Fwww.kernel.org%2Fdoc%2Fhtml%2Fv4.18%2Fadmin-guide%2Fmd.html__%3B!!LpKI!iiowvmnZKbmm6oDrWlR183PVeyQIWpk1UuxOrNEU45-F_2mqDG8rLqu54z9kwzGJFQDmCQRqPu5TKD5MgpHGKOg7%24&data=05%7C02%7CDony.Ye%40dell.com%7C6d13b91cb980462b293f08de11f19e72%7C945c199a83a24e809f8c5a91be5752dd%7C0%7C0%7C638967927075181820%7CUnknown%7CTWFpbGZsb3d8eyJFbXB0eU1hcGkiOnRydWUsIlYiOiIwLjAuMDAwMCIsIlAiOiJXaW4zMiIsIkFOIjoiTWFpbCIsIldUIjoyfQ%3D%3D%7C0%7C%7C%7C&sdata=U0dW7GCf5hXU1bVCFmiq7niGlGyQc9BiVe0WwD%2F52f4%3D&reserved=0)

 

建议的优化值因环境而已，我们无法提供准确值，需要根据现网实际环境进行测试调整，望理解。

\>\>\>\>\>\> 

还请反馈一下当前这些设置的情况，以及当前系统的负载情况：

# vmstat 1 4

# free -h

# /bin/ps axwwo user,pid,ppid,%cpu,%mem,vsz,rss,stat,time,cmd

 

 

==============================================

 

 

日志分析如下：

硬件日志：

PowerEdge R960 -- HCL4804

BIOS       1.9.7

iDRAC & LC         7.10.50.10

BOSS on Slimline SL11    2.1.13.2025

Backplanes 1 & 2              7.10

Disks 0 & 1 on BOSS on Slimline SL11       1.3.0

Disks 0-5 in Backplane 1 2.5.0

 

BOSS on Slimline SL11    BOSS-N1 Monolithic

Embedded AHCI Controllers 1 & 2              Sapphire Rapids SATA AHCI Controller

Disks 0 & 1 on BOSS on Slimline SL11       SK hynix 480 GB Dell NVMe PE8010 RI M.2 480GB

Disks 0-5 in Backplane 1 Samsung 3.84 TB Dell Ent NVMe v2 AGN RI U.2 3.84TB

 

BOSS on Slimline SL11

0                            Online   SK hynix               Dell NVMe PE8010 RI M.2 480GB 480 GB               NNC7N5267I1608459      Not Applicable   1.3.0

1                            Online   SK hynix               Dell NVMe PE8010 RI M.2 480GB 480 GB               NNC7N5267I160845A     Not Applicable   1.3.0

Backplane 1

0             Ready                   Samsung              Dell Ent NVMe v2 AGN RI U.2 3.84TB         3.84 TB               S6CPNA0T901554                           2.5.0

1             Ready                   Samsung              Dell Ent NVMe v2 AGN RI U.2 3.84TB         3.84 TB               S6CPNA0T901560                           2.5.0

2             Ready                   Samsung              Dell Ent NVMe v2 AGN RI U.2 3.84TB         3.84 TB               S6CPNA0T901561                           2.5.0

3             Ready                   Samsung              Dell Ent NVMe v2 AGN RI U.2 3.84TB         3.84 TB               S6CPNA0T901562                           2.5.0

4             Ready                   Samsung              Dell Ent NVMe v2 AGN RI U.2 3.84TB         3.84 TB               S6CPNA0T901564                           2.5.0

5             Ready                   Samsung              Dell Ent NVMe v2 AGN RI U.2 3.84TB         3.84 TB               S6CPNA0T901555                           2.5.0

 

 

系统日志分析：

OS message：

2025-10-19T11:00:33.878540+08:00 BWHANA-Master-PRD root: mdcheck start checking /dev/md0

[2025-10-19T11:00:33.881601+08:00 BWHANA-Master-PRD kernel: [9696190.044318][T104279] md: data-check of RAID array md0]

2025-10-19T11:05:00.436115+08:00 BWHANA-Master-PRD systemd[1]: Starting MD array scrubbing - continuation...

2025-10-19T11:05:00.451457+08:00 BWHANA-Master-PRD systemd[1]: mdcheck_continue.service: Deactivated successfully.

2025-10-19T11:05:00.451677+08:00 BWHANA-Master-PRD systemd[1]: Finished MD array scrubbing - continuation.

 

[2025-10-19T17:00:35.337574+08:00 BWHANA-Master-PRD kernel: [9717791.501891][T104279] md: md0: data-check interrupted.]

2025-10-19T17:00:35.491578+08:00 BWHANA-Master-PRD root: pause checking /dev/md0 at 3664407800

2025-10-19T17:00:35.493692+08:00 BWHANA-Master-PRD systemd[1]: mdcheck_start.service: Deactivated successfully.

[2025-10-19T17:00:35.493871+08:00 BWHANA-Master-PRD systemd[1]: Finished MD array scrubbing.]

 

 

MD check 运行之前IO状态：

                             DEV       tps     rkB/s     wkB/s   areq-sz    aqu-sz     await     svctm     %util

11:00:01       dev0-0    142.12  10184.10  13144.04    164.14      0.13      0.91      2.14     30.48

11:00:01       dev0-0    142.04   9992.62   13219.11    163.41      0.13      0.92      2.15     30.52

11:00:01       dev0-0    143.89  10292.07  13053.15    162.24      0.14      0.94      2.13     30.59

11:00:01       dev0-0    143.05  10199.82  13222.15    163.73      0.13      0.92      2.13     30.47

11:00:01       dev0-0    142.41  10097.11  13103.03    162.91      0.13      0.92      2.14     30.53

 

MD check运行过程中IO状态：

11:10:01       dev0-0    193.04  84673.33   1173.60    444.71      1.15      5.96      3.80     73.40

11:10:01       dev0-0    193.67  84702.89   1148.66    443.29      1.16      6.01      3.79     73.44

11:10:01       dev0-0    193.92  84702.68   1167.86    442.82      1.18      6.08      3.80     73.60

11:10:01       dev0-0    193.11  84684.09   1160.71    444.53      1.17      6.06      3.84     74.08

11:10:01       dev0-0    193.46  84702.81   1142.68    443.73      1.19      6.16      3.85     74.54

 

14:40:01       dev0-0    193.46  91784.43   1856.76    484.04      1.01      5.23      4.20     81.28

14:40:01       dev0-0    193.93  91795.75   1908.69    483.18      1.03      5.30      4.18     81.07

14:40:01       dev0-0    193.78  91808.35   1880.76    483.49      1.04      5.35      4.23     81.99

14:40:01       dev0-0    193.76  91810.64   1895.08    483.61      1.02      5.27      4.22     81.70

14:40:01       dev0-0    193.71  91802.18   1893.65    483.70      1.05      5.41      4.26     82.59

 

MD check运行结束后IO状态：

17:20:01       dev0-0    331.26  19304.16  39340.35    177.04      0.45      1.37      1.82     60.23

17:20:01       dev0-0    331.60  19355.62  39322.33    176.95      0.46      1.39      1.81     60.15

17:20:01       dev0-0    332.19  19511.81  39266.49    176.94      0.49      1.48      1.83     60.95

17:20:01       dev0-0    333.73  19342.92  39437.21    176.13      0.48      1.43      1.81     60.31

17:20:01       dev0-0    331.75  19402.66  39350.01    177.10      0.48      1.46      1.81     59.99

 

18:20:01       dev0-0    162.15   6332.90   9711.80     98.95      0.13      0.80      1.15     18.62

18:20:01       dev0-0    164.91   6305.43   9688.72     96.99      0.14      0.83      1.12     18.48

18:20:01       dev0-0    160.33   6374.38   9766.31    100.67      0.14      0.87      1.16     18.64

18:20:01       dev0-0    161.01   6290.55   9681.43     99.20      0.14      0.89      1.15     18.49

18:20:01       dev0-0    163.12   6330.57   9754.49     98.61      0.15      0.93      1.14     18.53

 

MDcheck 配置运行时间规则如下：

# /bin/systemctl show 'mdcheck_start.service' | sort

ActiveEnterTimestamp=n/a

ActiveEnterTimestampMonotonic=0

ActiveExitTimestamp=n/a

ActiveExitTimestampMonotonic=0

ActiveState=inactive

After=system.slice systemd-journald.socket mdcheck_start.timer sysinit.target basic.target

AllowIsolate=no

AssertResult=yes

AssertTimestamp=Sun 2025-10-19 11:00:33 CST

AssertTimestampMonotonic=9696238775750

DynamicUser=no

Environment="MDADM_CHECK_DURATION=6 hours"

 

# /bin/systemctl show 'mdcheck_continue.service' | sort

ActiveEnterTimestamp=n/a

ActiveEnterTimestampMonotonic=0

ActiveExitTimestamp=n/a

ActiveExitTimestampMonotonic=0

ActiveState=inactive

After=basic.target system.slice mdcheck_continue.timer systemd-journald.socket sysinit.target

AllowIsolate=no

AssertResult=yes

AssertTimestamp=Sun 2025-10-19 11:05:00 CST

 

Mdadm raid配置：

# /proc/mdstat

Personalities : [raid6] [raid5] [raid4]

[md0 : active raid5 nvme2n1[0] nvme6n1[6] nvme3n1[1] nvme1n1[5](S) nvme5n1[3] nvme4n1[2]]

      15002423296 blocks super 1.2 level 5, 512k chunk, algorithm 2 [5/5] [UUUUU]

      bitmap: 10/28 pages [40KB], 65536KB chunk

 

10/19号mdcheck在6小时内没有校验完成，默认会在每天早上11:05继续执行

Mon 2025-10-20 11:05:00 CST 1h 9min left       Sun 2025-10-19 11:05:00 CST 22h ago            mdcheck_continue.timer       mdcheck_continue.service

 

分析和建议：

1：从当前硬件TSR日志查看未发现客户反馈系统出现磁盘性能问题时间点（10/19 14:40分左右）存在硬件故障，磁盘状态均正常,磁盘固件已经最新版本

2：检查系统sar磁盘io文件可以明显看到当天11:00：33后MDchecking运行过程中，磁盘的await时间升高，硬盘使用率上升,17:00:35 mdcheck结束后，磁盘延迟和使用率恢复到正常状态，基本可以确认MDcheck运行引起IO异常。

3:mdcheck为系统开源软件，建议客户联系SUSE系统厂商对MDcheck策略进行调整避免MDcheck运行与业务IO冲突,降低MDcheck运行优先级。

4：结合suse的排查邮件来看系统厂商也确认是MDcheck导致，建议进一步按照系统厂商提供建议进行调整后观察是否可以规避MDcheck运行期间导致业务IO影响的问题。
