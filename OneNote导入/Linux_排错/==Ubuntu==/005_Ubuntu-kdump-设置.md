Ubuntu-kdump-设置

2024年6月6日

14:24

FYI：https://www.ebpf.top/post/ubuntu_kdump_crash/\
\
安装 kdump:\
root@user1:\~# apt install linux-crashdump[  ]

root@user1:\~# apt install crash

 

 

添加红色部分：

user1@user1:\~\$ sudo cat /etc/default/grub.d/kdump-tools.cfg 

GRUB_CMDLINE_LINUX_DEFAULT=\"\$GRUB_CMDLINE_LINUX_DEFAULT crashkernel=2G-4G:320M,4G-32G:512M,32G-64G:1024M,64G-128G:2048M,128G-:4096M\"

\
root@user1:\~# grub-mkconfig -o /boot/grub/grub.cfg \
-设置完成后重启系统生效

 

一系列检查kdump设置情况：

root@user1:\~# kdump-config show

DUMP_MODE:                kdump

USE_KDUMP:                1

KDUMP_COREDIR:                /var/crash

crashkernel addr: 0x32000000

0x3f7f000000

   /var/lib/kdump/vmlinuz: symbolic link to /boot/vmlinuz-5.15.0-112-generic

kdump initrd:

   /var/lib/kdump/initrd.img: symbolic link to /var/lib/kdump/initrd.img-5.15.0-112-generic

current state:    ready to kdump

 

kexec command:

  /sbin/kexec -p \--command-line=\"BOOT_IMAGE=/vmlinuz-5.15.0-112-generic root=/dev/mapper/ubuntu\--vg\--1-ubuntu\--lv ro processor.max_castate=0 intel_idle.max_cstate=0 intel_pstate=disable idle=poll reset_devices systemd.unit=kdump-tools-dump.service nr_cpus=1 irqpoll nousb\" \--initrd=/var/lib/kdump/initrd.img /var/lib/kdump/vmlinuz

root@user1:\~# 

 

查看 kdump-tools 服务状态：

![[__Ubuntu___005_Ubuntu-kdump-设置_001.png]]

 

root@user1:\~# cat /proc/cmdline 

BOOT_IMAGE=/vmlinuz-5.15.0-112-generic root=/dev/mapper/ubuntu\--vg\--1-ubuntu\--lv ro processor.max_castate=0 intel_idle.max_cstate=0 intel_pstate=disable idle=poll crashkernel=2G-4G:320M,4G-32G:512M,32G-64G:1024M,64G-128G:2048M,128G-:4096M

 

 

root@user1:\~# dmesg -T \| grep -i crash

\[Wed Jun 12 09:17:25 2024\] Command line: BOOT_IMAGE=/vmlinuz-5.15.0-112-generic root=/dev/mapper/ubuntu\--vg\--1-ubuntu\--lv ro processor.max_castate=0 intel_idle.max_cstate=0 intel_pstate=disable idle=poll crashkernel=2G-4G:320M,4G-32G:512M,32G-64G:1024M,64G-128G:2048M,128G-:4096M

\[Wed Jun 12 09:17:25 2024\] x86/split lock detection: #AC: crashing the kernel on kernel split_locks and warning on user-space split_locks

\[Wed Jun 12 09:17:25 2024\] Reserving 256MB of low memory at 800MB for crashkernel (low RAM limit: 4096MB)

\[Wed Jun 12 09:17:25 2024\] Reserving 4096MB of memory at 260080MB for crashkernel (System RAM: 261595MB)

\[Wed Jun 12 09:17:25 2024\] Kernel command line: BOOT_IMAGE=/vmlinuz-5.15.0-112-generic root=/dev/mapper/ubuntu\--vg\--1-ubuntu\--lv ro processor.max_castate=0 intel_idle.max_cstate=0 intel_pstate=disable idle=poll crashkernel=2G-4G:320M,4G-32G:512M,32G-64G:1024M,64G-128G:2048M,128G-:4096M

\[Wed Jun 12 09:17:29 2024\] pstore: Using crash dump compression: deflate

\[Wed Jun 12 09:17:30 2024\] megaraid_sas 0000:65:00.0: firmware crash dump        : no

-这里可以看到预留内存是 4096MB。

 

root@user1:\~# cat /proc/iomem \| grep -i crash

  36000000-41ffffff : Crash kernel

-查看 crashkernel 内存分配的地址空间

 

root@user1:\~# cat /sys/kernel/kexec_crash_size

4294967296

-查看 crashkernel 内存分配的大小，4G多，也就是预留给kdump了。

 

 

手动触发 crash 测试，回触发系统重启

\$ sudo echo c \> /proc/sysrq-trigger\
如果保修的内存不够大，系统回无法自动重启，会一直卡住。

 

root@user1:/var/crash/202406120914# ls -hl 

total 713M

-rw\-\-\-\-\-\-- 1 root root 152K Jun 12 09:15 dmesg.202406120914

-rw-r\--r\-- 1 root root 713M Jun 12 09:15 dump.202406120914

 

-此文件可以查看，从信息看可以知道是手动触发的

root@user1:/var/crash# vim 202406120914/dmesg.202406120914

![[__Ubuntu___005_Ubuntu-kdump-设置_002.png]]

 

=== done ===

 

 

 

已使用 OneNote 创建。
