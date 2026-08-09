kdump

2025年2月27日

15:47

kdump 会启动一个子kernel，大概会占用机器内存1G的空间，在系统出现hang机的时候收集服务器内存里的信息，如果服务器内存越大，需要的时间会越久，所以最好设置好kdump后需要测试一些。

1、设置kdump

\[root@localhost \~\]# vim /etc/kdump.conf

\...\...

145 path /var/crash                                  

146 core_collector makedumpfile -c \--message-level 1 -d 31

\...\...

\\\\ -c 启动压缩功能 -d 去除不需要的信息，31是去除空页信息

2、sysctl 里的一些设置

\[root@localhost \~\]# sysctl -a \|grep panic

kernel.panic = 0     #panic error中自动重启，如果是20，就是等待timeout为20秒

kernel.panic_on_oops = 1  

kernel.softlockup_panic = 0

kernel.unknown_nmi_panic = 0

kernel.panic_on_unrecovered_nmi = 0

kernel.panic_on_io_nmi = 0

kernel.hung_task_panic = 0

vm.panic_on_oom = 0

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

kernel.sysrq = 1    #激活Magic SysRq  否则，键盘鼠标没有响应 \[ Alt+截屏键[ \] ]

3、vmcore安装

1）收到kdump文件的名字是vmcore

2）需要 crash 和 kernel-debinginfo 

3）kernel-debinginfo 需要根据客户的kernel版本来进行下载

RHEL Server Debuginfo (v.6 for x86_64)

<https://rhn.redhat.com/network/software/channels/details.pxt?cid=10488>

RHEL Server Debuginfo (v.6 for x86)

<https://rhn.redhat.com/network/software/channels/details.pxt?cid=10360>

Red Hat Enterprise Linux Debuginfo (v. 5 for 64-bit x86_64)

<https://rhn.redhat.com/network/software/channels/details.pxt?cid=12920>

Red Hat Enterprise Linux Debuginfo (v. 5 for 32-bit x86)

[https://rhn.redhat.com/network/software/channels/details.pxt?cid=1289](https://rhn.redhat.com/network/software/channels/details.pxt?cid=12898)8

4）收到vmcore后查看kernel版本

# strings vmcore \| less

...\...

2.6.18-194.el5

...\...

5）安装需要两个包：

kernel-debuginfo-\*.rpm

kernel-debuginfo-common-\*.rpm

例子：CentOS 的debuginfo包的名字。

kernel-debuginfo-2.6.32-431.el6.x86_64.rpm

kernel-debuginfo-common-x86_64-2.6.32-431.el6.x86_64.rpm

需要先安装"kernel-debuginfo-common"后安装"kernel-debuginfo"。

例子：

 

但是这种方法没有下面提取好，因为不需要安装。

4、提取 vmlinux kernel-debug

# rpm -qpl kernel-debuginfo-2.6.18-194.el5.x86_64.rpm \| grep vmlinux

warning: kernel-debuginfo-2.6.18-194.el5.x86_64.rpm: Header V3

DSA/SHA1 Signature, key ID 37017186: NOKEY

/usr/lib/debug/lib/modules/2.6.18-194.el5/vmlinux

通过 rpm2cpio 提取文件

# rpm2cpio kernel-debuginfo-2.6.18-194.el5.x86_64.rpm \| cpio -idv

\...\...

\...\...

./usr/lib/debug/lib/modules/2.6.18-194.el5/vmlinux

./usr/lib/debug/lib/modules/2.6.18-194.el5/vmlinux

1194352 blocks

\-\-\-\-\-\-\-\-\--

提取后会在当前目录出现了一个usr的文件夹。

5、手动触发kdump

\# echo c \> /proc/sysrq-trigger

\\\\ 这时机器会处于hang机状态

6、打开 vmcore

\# cd /var/crash/

# crash vmcore /usr/lib/debug/lib/modules/2.6.18-194.el5/vmlinux

crash 5.1.8-1.el5

Copyright (C) 2002-2011  Red Hat, Inc.

Copyright (C) 2004, 2005, 2006  IBM Corporation

Copyright (C) 1999-2006  Hewlett-Packard Co

Copyright (C) 2005, 2006  Fujitsu Limited

Copyright (C) 2006, 2007  VA Linux Systems Japan K.K.

Copyright (C) 2005  NEC Corporation

Copyright (C) 1999, 2002, 2007  Silicon Graphics, Inc.

Copyright (C) 1999, 2000, 2001, 2002  Mission Critical Linux, Inc.

This program is free software, covered by the GNU General Public License,

and you are welcome to change it and/or distribute copies of it under

certain conditions.  Enter \"help copying\" to see the conditions.

This program has absolutely no warranty.  Enter \"help warranty\" for details.

 

GNU gdb (GDB) 7.0

Copyright (C) 2009 Free Software Foundation, Inc.

License GPLv3+: GNU GPL version 3 or later \<<http://gnu.org/licenses/gpl.html>\>

This is free software: you are free to change and redistribute it.

There is NO WARRANTY, to the extent permitted by law.  Type \"show copying\"

and \"show warranty\" for details.

This GDB was configured as \"x86_64-unknown-linux-gnu\"\...

      KERNEL: /usr/lib/debug/lib/modules/2.6.18-308.el5/vmlinux

    DUMPFILE: vmcore

        CPUS: 4

        DATE: Fri Feb  7 13:48:12 2014

      UPTIME: 00:14:10

LOAD AVERAGE: 0.00, 0.02, 0.03

       TASKS: 223

    NODENAME: localhost.localdomain

     RELEASE: 2.6.18-308.el5

     VERSION: #1 SMP Fri Jan 27 17:17:51 EST 2012

     MACHINE: x86_64  (2666 Mhz)

      MEMORY: 3.9 GB

       PANIC: \"SysRq : Trigger a crashdump\"

         PID: 3909

     COMMAND: \"bash\"

        TASK: ffff810129019100  \[THREAD_INFO: ffff81011aa8a000\]

         CPU: 2

       STATE: TASK_RUNNING (SYSRQ)

crash\> 

crash \> bt \> bt.txt    \\\\ 导出信息

crash \> log \> log.txt  \\\\ 导出信息

crash \--minimal 最小方式调试

7、问题定位

crash\> bt

PID: 20795 TASK: 10129422030 CPU: 3 COMMAND: \"CtrlLvmFs.sh\"

#0 \[1007306fc80\] try_crashdump at ffffffff8014c7d5

#1 \[1007306fc90\] do_page_fault at ffffffff80124b20

#2 \[1007306fd70\] error_exit at ffffffff80110d91

[\[exception RIP: sysfs_hash_and_remove+14\] ]Something went wrong at

sysfs_hash_and_remove+14 bytes

\\\\ 找 RIP 信息 { 比如：[\<1\>RIP  \[\<ffffffff81053b65\>\] find_busiest_group+0x5c5/0xb20] 这是个已知的issue，在solution可以找到。 }

crash\> dis -rl sysfs_hash_and_remove+14  [\\\\搜索](file://搜索)

/builddir/build/BUILD/kernel-2.6.9/linux-2.6.9/fs/sysfs/inode.c: 161

0xffffffff801b51f5 \<sysfs_hash_and_remove\>: push %r14 The top of the function.

0xffffffff801b51f7 \<sysfs_hash_and_remove+2\>: mov %rsi,%r14

0xffffffff801b51fa \<sysfs_hash_and_remove+5\>: push %r13

0xffffffff801b51fc \<sysfs_hash_and_remove+7\>: mov %rdi,%r13

0xffffffff801b51ff \<sysfs_hash_and_remove+10\>: push %r12

0xffffffff801b5201 \<sysfs_hash_and_remove+12\>: push %rbp

0xffffffff801b5202 \<sysfs_hash_and_remove+13\>: push %rbx

/builddir/build/BUILD/kernel-2.6.9/linux-2.6.9/fs/sysfs/inode.c: 165 So it was at line 165 of

inode.c.

0xffffffff801b5203 \<sysfs_hash_and_remove+14\>: mov 0x10(%rdi),%rbp 14 bytes of

offset from the top. It was here when crashed. 0x10 is offset against rdi.

crash\> CPU tried to move

the value to rbp. Boon.

 

8、查看代码

160 void sysfs_hash_and_remove(struct dentry \* dir, const char \* name)

161 {

162 struct sysfs_dirent \* sd;

163 struct sysfs_dirent \* parent_sd = dir-\>d_fsdata;

164

165 if (dir-\>d_inode == NULL) Ahh. So it tripped here.

166 /\* no inode means this hasn\'t been made visible yet \*/

167 return;

\\\\ 需要懂一些汇报语言的知识

9. 补充

1） 在kdump追踪第三方模块

crash\> mod 

\\\\ 注意这个地方["\[ \]" ]模块名字，有时候会应为第三方模块导致的kernel panic。

\\\\ Redhat 第三方模块存放地方：

 \'modinfo \<module_name\> \| grep filename\'  出现的路径是 /lib/modules/2.6.18.X.X /extra/ 就是地三方的模块。

 

crash\> mod -t

no tainted modules

如果有问题：

crash\> mod -t\
NAME    TAINTS\
hpwdt   (U)\
lpfc    (U)\
hpsa    (U)\
be2net  (U)\
seos    P(U)

crash\> sym -l     \# 一些内存地址

0 (D) \_\_per_cpu_start

0 (D) irq_stack_union

4000 (d) exception_stacks

9000 (D) gdt_page

a000 (D) cpu_llc_shared_map

a008 (D) cpu_core_map

a010 (D) cpu_sibling_map

\...\...\...\...

2）Call 追踪的意思。

Call Trace ：

 \[\<c06d96ed\>\] \_\_neigh_event_send+0xe6/0x17a

 \[\<c06d97a2\>\] neigh_event_send+0x21/0x23

 \[\<c06db2ea\>\] neigh_resolve_output+0x23/0x106

 \[\<c04347a1\>\] ? local_bh_enable+0xd/0xf

 \[\<c06f2ca8\>\] ? ipv4_neigh_lookup+0x90/0xb7

 经常会看到上面的 Call Trace，代表一个D状态的进程，也就是不可中断的进程，超过120秒等待所显示的信息，但是不代表这个进程会有问题，或导致hang机。比如这个进程正在等一个回应超过了120秒，但是如果120后得到了回应就会继续下去。可以适当把这个时间设置长点。

具体的设置地方：

\# sysctl -a \|grep hung 

kernel.hung_task_timeout_secs = 120

3）crash开头的报告信息：

KERNEL: 系统崩溃时运行的 kernel 文件

DUMPFILE: 内核转储文件

CPUS: 所在机器的 CPU 数量

DATE: 系统崩溃的时间

TASKS: 系统崩溃时内存中的任务数

NODENAME: 崩溃的系统主机名

RELEASE: 和 VERSION: 内核版本号

MACHINE: CPU 架构

MEMORY: 崩溃主机的物理内存

PANIC: 崩溃类型，常见的崩溃类型包括：

SysRq (System Request)：通过魔法组合键导致的系统崩溃，通常是测试使用。通过 echo c \> /proc/sysrq-trigger，就可以触发系统崩溃。

oops：可以看成是内核级的 Segmentation Fault。应用程序如果进行了非法内存访问或执行了非法指令，会得到 Segfault 信号，一般行为是 coredump，应用程序也可以自己截获 Segfault 信号，自行处理。如果内核自己犯了这样的错误，则会弹出 oops 信息。

========= 完 ===============

 

已使用 OneNote 创建。
