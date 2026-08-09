Kdump 设置

2024年4月17日

13:47

\
crashkernel=auto 参数代表系统自动计算为kernel crash 预留的内存空间，但是这种方式并不适合所有的环境。

比如，某些应用加载了一下硬件驱动或是那些驱动调用了其它的东西，那就可能需要更多的内存，这时就需要手动修改。

一个1T的内存预留 768MB已经是足够了。1T内存的大内存预留 512M也够。\
\
此命令参数在RHEL8与9有，RHEL7没有。\
[\[root@localhost \~\]# kdumpctl estimate]

Reserved crashkernel:[    192M][    ]\<\-- 预留的大小

Recommended crashkernel: 192M[    ]\<\-- 推荐预留的大小

 

Kernel image size:   54M

Kernel modules size: 8M

Initramfs size:      26M

Runtime reservation: 64M

Large modules:

    xfs: 1568768

 

在系统开机启动的时候会有提示预留空间的大小\
[\[root@localhost \~\]# grep \'Reserving .\* for crashkernel\' /var/log/messages\*]

......

Apr 11 23:41:30 localhost kernel: Reserving 192MB of memory at 976MB for crashkernel (System RAM: 32281MB)

......

\# 这说明预留了 224MB，总共的内存是 32G多。

 

测试：

echo 1 \> /proc/sys/kernel/sysrq

=====================具体设置 =====================

RHEL7 kdump 设置

 

1. 安装和设置kdump

[\[root@localhost \~\]# yum install crash\* kexec-tools    \#]默认情绪下有安装，可以检查一下。

\[root@localhost \~\]# vim /etc/kdump.conf

path /var/crash                                  

core_collector makedumpfile -l \--message-level 1 -d 31

 

2. sysctl 修改

\[root@localhost \~\]# vim /etc/sysctl.conf

kernel.sysrq = 1  

kernel.unknown_nmi_panic = 1

vm.panic_on_oom = 1   

 

修改之后，使用如下命令使配置永久生效：

\# sysctl -p      

 

3. 添加设置到GRUB。

\[root@localhost \~\]# vim /etc/default/grub

\...\...

GRUB_CMDLINE_LINUX=\"crashkernel=auto rhgb quiet\" \# 添加的是"crashkernel=auto"

\...\...

\[root@localhost \~\]# grub2-mkconfig -o /boot/grub2/grub.cfg

UEFI : \# grub2-mkconfig -o /boot/efi/EFI/redhat/grub.cfg

 

4. 查看 kdump 服务是否运行

\[root@localhost \~\]# systemctl start kdump.service

\[root@localhost \~\]# kdumpctl status   

Kdump is operational

 

检查是否有开启启动：

\[root@localhost \~\]# systemctl enable kdump

\[root@localhost \~\]# systemctl -a \|grep kdump

  kdump.service loaded active exited Crash recovery kernel arming

loaded active

 

5. 测试 kdump 是否配置成功

执行如下命令对需要进行内存转储的机器模拟产生 kernel panic：

echo 1 \> /proc/sys/kernel/sysrq[  \#]当你之前没有设置的情况下。

 

\[root@localhost \~\]# echo c \> /proc/sysrq-trigger

正常情况下，系统会在触发 kernel panic 之后将内存信息进行转储。如果配置正常，转储完成系统重启之后会在指定位置找到文件vmcore，使用 ls 查看。

vmcore日志默认位置：/var/crash/

 

该操作的目的有两个：

1. 确定 dump 配置成功，可生成 vmcore 文件。

2. 确定生成 vmcore 文件所需要的时间，以便确定在生产系统中是否允许这样的宕机时间。

注意 ：

echo c \> /proc/sysrq-trigger 表示通过手动的方式触发 kernel panic，该操作会造成系统宕机，系统的所有应用都将停止运行，所以需在合适的宕机时间操作。

 

6. 当机器一直hang住状态，没有重启。这种情况下也可以通过手动触动dump的方式保存vmcore。

键盘组合键：Alt+PrintScreen+c

 

 

 

已使用 OneNote 创建。
