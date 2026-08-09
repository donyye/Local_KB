Intel CPU 微码下载（[Microcode](https://downloadcenter.intel.com/Detail_Desc.aspx?DwnldID=23984&lang=eng&ProdId=3534)）

Friday, August 29, 2014

2:26 PM

<https://downloadcenter.intel.com/SearchResult.aspx?lang=eng&FamilyId=24&LineId=3005&ProductID=3534&ProdId=3534>

 

 

![[Technology_ALL_未分类知识库_015_Intel CPU 微码下载（Microcode）_001.png]]

 

点击后到下载页面：

![[Technology_ALL_未分类知识库_015_Intel CPU 微码下载（Microcode）_002.png]]

 

 

更新微码：

 

 

\[root@localhost \~\]# rpm -ql microcode_ctl

/etc/init.d/microcode_ctl

/lib/firmware/microcode.dat[   ]\--\> 存放地址

/sbin/microcode_ctl

/usr/share/man/man8/microcode_ctl.8.gz

 

 

\[root@localhost \~\]# rpm -qa \|grep microcode

microcode_ctl-1.17-1.56.el5

\[root@localhost \~\]#

\[root@localhost \~\]#

\[root@localhost \~\]# grep MICROCODE /lib/modules/\$(uname -r)/build/.config

CONFIG_MICROCODE=m

 

\[root@localhost \~\]# microcode_ctl -u

microcode_ctl: writing microcode (length: 460800)

microcode_ctl: microcode successfuly written to /dev/cpu/microcode

 

 

参考：http://jianlee.ylinux.org/Computer/SystemAdmin/microcode.html

 

Microcode

微代码（Microcode）是Intel的P6和NetBurst两个家族CPU（也就是 Pentium Pro以及更新的IA 32 CPU，Pentium X以及它们的 Xeon/Celeron变体）的一种更新特性。通过从Intel的站点下载微代 码，加载到处理器核中，对一些BUG进行勘误。微代码是包含处理器的 微指令（Microinstructions）的数据块，具体细节将在正翻译中的 《P6 Family Processor Microcode Update Feature Review》文档中 给出。

Linux下的Microcode操作

Linux内核提供了支持微代码更新的驱动程序，设备文件是 /dev/cpu/microcode，如果你的机器是SMP的，那可能「cpu」就该替 换成「cpu0」之类。这一驱动程序位于 arch/i386/kernel/microcode.c， make menuconfig时通过 CONFIG_MICROCODE选择静态编译、模块或者是不选。

 

源文档 \<[http://jianlee.ylinux.org/Computer/SystemAdmin/microcode.html](http://jianlee.ylinux.org/Computer/SystemAdmin/microcode.html)\> 

 

 

 

已使用 OneNote 创建。
