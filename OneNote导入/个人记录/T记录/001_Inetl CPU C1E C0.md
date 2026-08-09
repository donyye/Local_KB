Inetl CPU C1E C0

2015年1月9日

10:18

EIST全名Enhanced Intel SpeedStep Technology（增强型Intel SpeedStep技术），是Intel的新节能技术。和早期的SpeedStep技术不同，增强型的EIST技术可以动态调整CPU频率，随着CPU使用率地下或者接近0时候降低CPU频率并且降压，从而降低功耗和发热。一旦检测到CPU使用率高，立马回复原始工作频率。 

 

C1E：系统闲置状态时的CPU节能功能

 

C3/C6是比C1E更深层的省电模式，

C3 状态（深度睡眠）

总线频率和 PLL 均被锁定

在多核心系统下，缓存无效

在单核心系统下，内存被关闭，但缓存仍有效

可以节省 70% 的 CPU 功耗，但平台功耗比 C2 状态下大一些

唤醒时间需要 50 微秒

 

 

C6 状态

二级缓存减至零后， CPU 的核心电压更低

不保存 CPU context

功耗未知，非常低接近零

唤醒时间未知

 

Redhat KB: [https://access.redhat.com/solutions/202743](https://access.redhat.com/solutions/202743)

 

Linux 从kernel层关闭Intel 节能模式，防止kernel回写BIOS。

kernel \<2.6.32-358.11.1.el6 [ ][ ]在回写会有此问题。\<https://access.redhat.com/solutions/403433\> 

kernel层添加：intel_idle.max_cstate=0

echo 0 \> /sys/module/intel_idle/parameters/max_cstate

cpupower frequency-set -g performance[  ]\#立即生效[   (BIOS  ]要改成 watt OS) 8这样改

需要注意的是 如设置cstate=1  但CPUPOWER 命令就不能使用

 

永久生效，改 /usr/lib/tuned/latency-performance/tuned.conf。改完执行 tuned-adm profile latency-performance 命令生效

![[个人记录_T记录_001_Inetl CPU C1E C0_001.png]]

 

echo performance  \> /sys/devices/system/cpu/cpuX/cpufreq/scaling_governor

==================================================================================

需要注意的是 RHEL7.8 版本和之后 改这个已经不生效了。需要修改BIOS\
在BIOS中那个performance项要改成DAPC。然后CPU会锁定最高频率。

![[个人记录_T记录_001_Inetl CPU C1E C0_002.png]]

=========

最近研究了一下Linux的休眠问题，分享给大家，如有问题还请指正。

 

状态说明：

C-state：允许CPU进入非C0的状态

C1E：当CPU进入C1状态的时候允许降频使用。

 

推荐工具：

Linux好像没有自带查看C-state的工具，推荐使用i7z小工具查看，make之后可以直接使用，非常简单。

[https://code.google.com/p/i7z/](https://code.google.com/p/i7z/)

![[个人记录_T记录_001_Inetl CPU C1E C0_003.jpg]]

C1E的状态可以直接在/proc/cpuinfo中看到（降频的时候看到每个core的频率可能是不一样的）

![[个人记录_T记录_001_Inetl CPU C1E C0_004.jpg]]

 

我们经常遇到在BIOS中关闭了C1E和C-state后在linux环境下CPU仍然会休眠，原因是intel idle driver可以绕过BIOS的C1E,C-state设置，我之前的做法是在BIOS中关闭Monitor/Mwait选项（关闭之后会使用acpi idle driver），测试中发现关了这个仍然没用，如果要让CPU 100%运行在C0状态下有2种方式：

1，关闭C1E,C-state，在grub中添加：intel_idle.max_cstate=0 和idle=poll两个参数，重启生效

2，关闭C1E,C-states和Monitor/Mwait，在grub中添加idle=poll，重启生效

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

RHEL7 修改方法：\"intel_idle.max_cstate=0 processor.max_cstate=1 intel_pstate=disable idle=poll"

\-\-\-\-\-\-\-\-\-\-\-\-\--

[ sed -i \'/GRUB_CMDLINE_LINUX/\' grub ]

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

Ubuntu,节能kernel设置：

processor.max_cstate=1 intel_idle.max_cstate=0

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

1)将 intel_idle.max_cstate 设置为零将将cpuidle驱动程序恢复为 acpi_idle。

2)processor.max_cstate 选项将 acpi_idle 驱动程序的最大C 状态设置为零

3）idle=poll 这将禁用c1/c1e,最高性能选项，以功耗和热量为代价。

idle=halt

4） intel_pstate=disable[  ]此驱动程序通过内置调频器，实现 Intel Core 处理器的调频驱动。

FYI：https://wiki.archlinux.org/index.php/CPU_frequency_scaling\_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)

 

有案例显示，在BIOS开启C1E，也就是关闭CPU的节能模式情况下会出现下面错误：

kernel: p4-clockmod: Warning: EST-capable CPU detected. The acpi-cpufreq module offers voltage scaling in addition to frequency scaling. You should use that instead of p4-clockmod, if possible.

 

但是不影响机器使用，开启节能模式后就不会有。

后来：

p4-clockmod ：

面向 Intel Pentium 4/Xeon/Celeron 处理器的 CPUFreq 驱动程序，可通过跳频来降低 CPU 温度。（您最好使用 SpeedStep 驱动程序。） 

 

关于BIOS的PerfPerWattOptimizedDapc参数设置为Performance ，主要的目的是禁用CPU的C-state和C1E。

C-state和C1E属于CPU的节能模式的一部分。其中C1E 是C-state 1的增强模式。Intel设计CPU在处于空闲的情况下，会进入不同的空闲状态，也就是C-state。部分或者全部处理器核心会挂起停止执行指令。一些CPU内部的相关组件，例如内存控制器，PCIE控制器也会有相应的状态变化。

C-state的主要设计目的是减少服务器在空闲时的功耗， 所以出厂默认这个项目是打开的，也就是BIOS设置在PerfPerWattOptimizedDapc 。不过CPU进出C-state的状态切换可能带来一些兼容性方面的问题\-\-\--其中一部分问题我们可以通过更更新BIOS中附带的CPU Microcode来改善，不过当遇到CPU相关的稳定性问题时，我们建议永久禁用节能模式， 或者至少禁用节能模式作测试对照。

 

====================================

睿频维持MAX:\
Ubutun: BIOS设置成performance osdbpm模式，操作系统里cpupower frequency-set -g performance && cpupower idle-set -D 1

有机会测试一下

====================================

ESXI 下检查C1E是否有关闭：

在命令行中再次确认相关CPU 节电功能是否关闭

ESXi里面是有C1E的kernel参数的(没有CSTATS参数)，而且ESXi启动的时候会检测BIOS是否打开了C1E

 

![[个人记录_T记录_001_Inetl CPU C1E C0_005.jpg]]

 

检查Kernel parameter中的当前C1E设置

 

![[个人记录_T记录_001_Inetl CPU C1E C0_006.jpg]]

 

如果没有关闭C1E，关闭ESXi中的C1E参数可以通过如下命令来执行，然后运行下/sbin/auto-backup.sh，重启后才会生效

 

 

已使用 OneNote 创建。
