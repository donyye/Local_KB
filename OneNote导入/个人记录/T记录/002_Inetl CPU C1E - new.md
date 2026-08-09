Inetl CPU C1E - new

2024年6月5日

16:24

- RHEL7 设置：

  intel_idle.max_cstate=0 processor.max_cstate=1 intel_pstate=disable idle=poll

   

  RHEL7 RHEL8 设置：

  intel_idle.max_cstate=0 processor.max_cstate=0 idle=poll idle=halt\
  [https://access.redhat.com/solutions/6969204](https://access.redhat.com/solutions/6969204)

   

   

  Ubuntu 20.04 设置：

  \# vim /etc/default/grub

  ......

  GRUB_CMDLINE_LINUX_DEFAULT=\"processor.max_cstate=1 intel_idle.max_cstate=0 intel_pstate=disable\"

   

  \# grub-mkconfig -o /boot/grub/grub.cfg

   

  安装 cpupower

  \# apt install linux-tools-5.4.0-182-generic

   

  \# uname -r

  5.15.0-107-generic

   

   

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

  root@user1:\~# cpupower frequency-info

   

  root@user1:\~# cpupower frequency-set -g performance

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

   

   

  - 锁定 cpu 频率

  idle=poll idle=halt + Custom

   

  ![[个人记录_T记录_002_Inetl CPU C1E - new_001.png]]

   

   

   

  - CPU 会锁定最高睿频跑

  idle=poll + performance 

   

  ![[个人记录_T记录_002_Inetl CPU C1E - new_002.png]]

   

   

  cpu无法锁定频率可能有相关的：tuned

  systemctl disable \--now tuned

  tuned 是一个动态调整系统性能和功耗的工具，主要用于优化 Linux 系统的性能。它通过监控系统活动并根据预定义的配置文件动态调整各种系统设置，以便在不同的工作负载下实现最佳性能和最低功耗。

   

  \# tuned-adm list[  ]\--\> 列出所有配置

  ......

  throughput-performance      - Broadly applicable tuning that provides excellent performance across a variety of common server workloads

  \# 广泛适用的调整，可在各种常见服务器工作负载中提供出色的性能

  ......

  \# tuned-adm profile throughput-performance[    \--\> ]改成某个配置

   

  关闭是 systemctl disable \--now tuned

   

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

  Antti 告知：

  目前发现一个问题，就算设置了kernel也偶尔会降频, 测试了RHEL8, RHEL9, 和kernel 6.x都是一样, 应该是cpuinfo的更新问题在新的kernel改了, 所以应该是正常的。

  是偶尔降一下, 24小时可能就出现20几次, 应该是统计问题

  有说了一下，没有明说：

  <https://access.redhat.com/solutions/6705591>

   

  - Even if idle state is set to poll is used the cpuinfo will still show base clock frequency, however turbostat will show max frequency. Here is example below

  即使使用空闲状态， poll cpuinfo 仍将显示基本时钟频率，但 turbostat 将显示最大频率。下面是示例

   

   

   

  ============

  Fix CPU frequency on specified value - Beisen Cloud Computing

  Per just conversion, I\'ve been trying to reproduce the issue in our lab using the specific kernel below. And it\'s showing the same result of customer.

  So in order to fulfil customer\'s need that requires all CPUs running with full speed. I\'ve just got an workaround that\'s put \"intel_pstate=passive\" to the kernel parameter. That will force the kernel to use \"intel_cpufreq\" as driver to scale the CPU frequency. I just tested and it will get all CPUs running with full speed in our lab as following.

  So now you may schedule a remote session, that we can demonstrate how it works and the details of this setting. Thanks.

  根据转换，我一直在尝试使用下面的特定内核在我们的实验室中重现该问题。它显示了与客户相同的结果。

  因此，为了满足客户的需求，需要所有 CPU 全速运行。我刚刚找到了一种解决方法，即将"intel_pstate=passive"放入内核参数。这将强制内核使用"intel_cpufreq"作为驱动程序来缩放 CPU 频率。我刚刚测试过，它将使我们实验室中的所有 CPU 全速运行，如下所示。

  所以现在您可以安排一个远程会话，我们可以演示它的工作原理和此设置的详细信息。谢谢。

   

   

  \[root@testr760xd \~\]# cat /proc/cpuinfo \| grep \'cpu MHz\' \| head

  cpu MHz         : 3900.000

  cpu MHz         : 3900.000

  cpu MHz         : 3900.000

  cpu MHz         : 3900.000

  cpu MHz         : 3900.000

  cpu MHz         : 3900.000

  cpu MHz         : 3900.000

  cpu MHz         : 3900.000

  cpu MHz         : 3900.000

  cpu MHz         : 3900.000

  \[root@testr760xd \~\]# uname -a

  Linux testr760xd.sst.lab 5.15.13-1.el7.elrepo.x86_64#1 SMP Tue Jan 4 17:33:28 EST 2022 x86_64 x86_64 x86_64 GNU/Linux

  \[root@testr760xd \~\]# cat /proc/cmdline

  BOOT_IMAGE=/vmlinuz-5.15.13-1.el7.elrepo.x86_64 root=UUID=61818bdd-2e45-454f-821f-adf789519bf3 ro crashkernel=auto rhgb quiet LANG=en_US.UTF-8 intel_pstate=passive

  \[root@testr760xd \~\]#

   

  From GPT:

  intel_pstate=passive 参数

  当你在引导参数中使用 intel_pstate=passive 时，你是在告诉内核将Intel P-state驱动程序设置为被动模式。这意味着：

  1 频率调节控制权交给cpufreq调速器：Intel P-state 驱动程序将不再主动控制CPU频率，而是由其他cpufreq调速器来决定频率调整策略。

  2 驱动程序提供信息：尽管不再主动调节频率，Intel P-state 驱动程序仍会提供可用的频率范围和其他必要的信息给cpufreq调速器。

   

   

   

  ==========

  Cpu 降频需要考虑：

  机器是否单电源，导致供电不足导致降频。有案例。

   

  IOPS有异常也需要考虑 PERC 卡是否在做PR(巡检)或修复导致。有案例。

   

   

  ========================

  AMD  cpu

  <https://access.redhat.com/solutions/7079581>

  amd-pstate 是 AMD CPU 性能扩展驱动程序，它引入了一种新的 CPU 频率控制机制

   

  Ensure that the system BIOS is configured to allow the OS to control CPU power management.

   

  \# grubby \--args=\"amd_pstate=\<parameter\>\" \--update-kernel=ALL\
  \# reboot

   

  ===================

  new 查看命令

  grep \"cpu MHz\" /proc/cpuinfo \|awk \'\' \| column

   

 

已使用 OneNote 创建。
