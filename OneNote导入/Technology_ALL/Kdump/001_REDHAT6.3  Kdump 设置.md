REDHAT6.3[  Kdump ]设置

Wednesday, December 04, 2013

9:58 AM

下图是设置Kdump和启用NMI的方式，下次如果机器出现了非Panic的Hang住情形，请提示用户进入Idrac的电源管理界面，

通过发送NMI指令的方式，获取dump文件，这将是分析hang住原因最有利的证据。

 

 

1 - 开启Kdump配置界面

Usage the使用"system-config-kdump"命令启动kdump配置界面。

[\[root@localhost \~\]# system-config-kdump      ]

 

OR

在System \--\> Administration \--\> Kernel crash dumps，点击运行，开启Kdump配置工具界面。

![[Technology_ALL_Kdump_001_REDHAT6.3  Kdump 设置_001.gif]]

 

 

2 - 点击"Enable"按钮，启用Kdump。

NOTE:

默认启用kdump，即会在grub.conf文件的kernel行加入crashkernel=auto。

![[Technology_ALL_Kdump_001_REDHAT6.3  Kdump 设置_002.jpg]]

 

![Machine generated alternative text: 憲 Kernel Dump Configuration \[ile ptions eIp ci . Apply Reload Enak Disable Help Basic Settings ? Automated kdump memory settings Target settings O Manual kdump memory settings Filtering settings -\]- Expert settings](attachments/Technology_ALL_Kdump_001_REDHAT6.3%20%20Kdump%20设置_003.jpg)

For Example:

\[root@localhost \~\]# more /boot/grub/grub.conf

default=0

timeout=5

splashimage=(hd0,0)/grub/splash.xpm.gz

hiddenmenu

title Red Hat Enterprise Linux (2.6.32-279.el6.x86_64)

root (hd0,0)

kernel /vmlinuz-2.6.32-279.el6.x86_64 ro root=/dev/mapper/VolGroup-lv_ro

ot nomodeset rd_NO_LUKS LANG=en_US.UTF-8 rd_NO_MD rd_LVM_LV=VolGroup/lv_swap SYS

FONT=latarcyrheb-sun16 crashkernel=auto rd_LVM_LV=VolGroup/lv_root  KEYBOARDTYPE

=pc KEYTABLE=us rd_NO_DM rhgb quiet

initrd /initramfs-2.6.32-279.el6.x86_64.img

 

 

3 - 建议使用手动指定kdump memory大小。

![[Technology_ALL_Kdump_001_REDHAT6.3  Kdump 设置_004.jpg]]

NOTE:

\## BIOS模式 \##

I686/X86_64:  crashkernel=128M

PATH: /boot/grub/grub.conf

 

/boot

└── grub

   └── grub.conf   \<\-- crashkernel=128M

 

 

\## UEFI模式 \##

IA64:  crashkernel=256M

PATH: /boot/efi/EFI/redhat/grub.conf         

 

/boot

└── efi

   └── EFI

       ├── Dell

       │   └── BootOptionCache

       │       └── BootOptionCache.dat

       └── redhat

           └── grub.conf   \<\-- crashkernel=256M

 

 

4 -

 

 

 

![[Technology_ALL_Kdump_001_REDHAT6.3  Kdump 设置_005.jpg]]

 

 

 

 

![[Technology_ALL_Kdump_001_REDHAT6.3  Kdump 设置_006.jpg]]

 

 

 

通过NMI按钮，来触发死机

 

(a) -- 在BIOS \--\> System Security \--\> NMI button设置成Enabled。

![[Technology_ALL_Kdump_001_REDHAT6.3  Kdump 设置_007.jpg]]

(b) -- 在系统下设置kdump，配置方法参看附件。

(c) -- 系统下启动用NMI

//检查NMI是否为开启

\[root@localhost \~\]# sysctl kernel.unknown_nmi_panic

kernel.unknown_nmi_panic = 0    \<\-- 表示为禁用状态

kernel.panic_on_unrecovered_nmi = 0

// 编辑配置文件在系统启用NMI

\[root@localhost \~\]# vim /etc/sysctl.conf

kernel.unknown_nmi_panic = 1   \<\-- 添加此行

kernel.panic_on_unrecovered_nmi = 1

 

//重新加载设定

\[root@localhost \~\]# sysctl -p

 

之后可以通过idrac 的电源管理界面中，手动发送NMI指令，测试是否会产生kdump

 

![[Technology_ALL_Kdump_001_REDHAT6.3  Kdump 设置_008.jpg]]

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

其它选项：

![[Technology_ALL_Kdump_001_REDHAT6.3  Kdump 设置_009.png]]

 

![[Technology_ALL_Kdump_001_REDHAT6.3  Kdump 设置_010.png]]

 

![[Technology_ALL_Kdump_001_REDHAT6.3  Kdump 设置_011.png]]

 

 

已使用 OneNote 创建。
