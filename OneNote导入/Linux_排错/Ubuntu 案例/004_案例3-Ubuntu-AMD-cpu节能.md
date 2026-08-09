案例3-Ubuntu-AMD-cpu节能

2024年8月2日

14:13

测试环境 

R7525

CPU : AMD EPYC 7662 64-Core Processor

Ubuntu 20.04.4

 

处理器型号: AMD EPYC 7662 64-Core Processor

制程工艺: 7nm

核心/线程数: 64核/128线程

基础主频: 2.0 GHz

最大睿频: 3.3 GHz

缓存: 256MB L3缓存

内存支持: 8通道DDR4-3200

TDP: 225W

 

查看平率：

test@test:\~\$ cat /proc/cpuinfo \|grep \'cpu MHz\'

cpu MHz                : 1500.000

cpu MHz                : 1500.000

cpu MHz                : 1500.000

cpu MHz                : 1500.000

cpu MHz                : 1500.000

cpu MHz                : 1500.000

cpu MHz                : 1500.000

cpu MHz                : 1500.000

cpu MHz                : 1500.000

cpu MHz                : 1376.963

cpu MHz                : 1500.000

......

 

查看模式

test@test:\~\$ cat /sys/devices/system/cpu/cpu\*/cpufreq/scaling_governor

schedutil

schedutil

schedutil

schedutil

schedutil

......

 

powersave是无论如何都只会保持最低频率的所谓"省电"模式；

userspace 是自定义频率时的模式，这个是当你设定特定频率时自动转变的；

ondemand 一有cpu计算量的任务，就会立即达到最大频率运行，等执行完毕就立即回到最低频率；

conservative 翻译成保守的，也就是默认的模式，一般选择这个，会自动在频率上下限调整；

performance 顾名思义只注重效率，无论如何一直保持以最大频率运行。

 

修改成功 performance 模式：

1）图形化修改，先安装 "udo apt-get install indicator-cpufreq "安装后重启电脑，会再图形界面右上角会有个图标，点击图标，设置为performance就可以。

 

2）安装下面命令

test@test:\~\$ sudo apt-get install cpufrequtils

 

创建配置文件：

test@test:\~\$ sudo echo \'GOVERNOR=\"performance\"\' \> /etc/default/cpufrequtils

\#对所有cpu和逻辑cpu都生效。

 

重启服务：

test@test:\~\$ sudo /etc/init.d/cpufrequtils restart

 

可以看到频率已经改变：

test@test:\~\$ cat /proc/cpuinfo \| grep -i \"cpu mhz\"

cpu MHz                : 2000.000

cpu MHz                : 2000.000

cpu MHz                : 2000.000

cpu MHz                : 2000.000

cpu MHz                : 2000.000

......

 

也变成 performance 模式：

test@test:\~\$ cat /sys/devices/system/cpu/cpu\*/cpufreq/scaling_governor

performance

performance

performance

performance

performance

performance

performance

......

 

 

sudo cpufreq-set -c 0 -g performance 

这个命令只是对单个cpu做设定，比如 -c 0 指定设置的cpu核心编号，0表示第一个cpu核心。

 

 

 

另外改成 performance 在BIOS设置是[  ]performance per watt (OS) [ ]或是custom

 

![[Ubuntu 案例_004_案例3-Ubuntu-AMD-cpu节能_001.png]]

 

已使用 OneNote 创建。
