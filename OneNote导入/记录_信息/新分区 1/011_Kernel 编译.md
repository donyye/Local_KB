Kernel 编译

Tuesday, December 31, 2013

2:48 PM

1.   

\[root@localhost linux-2.6.34.14\]# make menuconfig

  HOSTCC  scripts/basic/fixdep

  HOSTCC  scripts/basic/docproc

  HOSTCC  scripts/basic/hash

  HOSTCC  scripts/kconfig/conf.o

  HOSTCC  scripts/kconfig/kxgettext.o

 \*\*\* Unable to find the ncurses libraries or the

 \*\*\* required header files.

 \*\*\* \'make menuconfig\' requires the ncurses libraries.

 \*\*\*

 \*\*\* Install ncurses (ncurses-devel) and try again.

 \*\*\*

make\[1\]: \*\*\* \[scripts/kconfig/dochecklxdialog\] Error 1

make: \*\*\* \[menuconfig\] Error 2

 

1.   

报错提示叫你安装"ncurses-devel"后再尝试

\[root@localhost linux-2.6.34.14\]# yum install ncurses-devel

 

1.   

\[root@localhost linux-2.6.34.14\]# make menuconfig

![Machine generated alternative text: ApplicationsPlacesSystem 晒穆团 乙）呼 Edit VieWSea代h ·性如丽下硬困山 飞叮nina! l口口口口e 目阅．｝洲．亡r} 沈丽f泥 甘2。6。34. 旦eIp 14Con UFat Arrowkeysnavigatethemenu includes,\<N\>excludes，《M\> \<Enter\> modUlariZeS ＜州卜module 匕工」，1 SeleCtSSUbmenUS 11{ 一＞.Highlightedlettersarehotkeys. Pressing 【＊\]built一in featUreS. \<\>module Press\<Esc\>\<Esc\>toexit,\<?\>forHelp,\</\>forSearch capable ＜丫＞ Legend: eXCIUded 曰 11Gener犯Set叮1口口旧 【＊』印ableloadablemodulesupport一＞ 一＊一〔nabletheblocklayer一＞ roceSSOrtypeandfeatures一＞ owermanagementandACPIoptions一＞ 日usOptionS(PCIetc.）一＞ 压xecutablefileformats/Emulations－一＞ 一＊一Ntworkingsupport一＞ \[eVICeDriVerS－一＞ FirlnwareDFIVeFS－一＞ FilesystemS一＞ Kernelhacking一＞ Securityoptions一＞ 一奉一cryptographicAPI一＞ 【＊』virtualization一＞ Libraryroutines一＞ oadanAlternateConfigurationFile SaveanAlternateConfigurationFile 国国国国国目一](attachments/记录_信息_新分区%201_011_Kernel%20编译_001.png)

 

 

1.   

现在开始对内核进行正式编译，这个操作估计得半个个小时左右。

编译时可以在命令行后面加上 -jXX 选项，XX是数字，表示同时进行编译的job数量。

这个数量通常设定为编译本机cpu的内核支持的并发线程的1-2倍，非常有用的一个选项。

这在多核多cpu的电脑上特别有用，公司的8核suse服务器执行make -j16只要不到20分钟，而双核PC上需要4个小时！

[\[root@localhost linux-2.6.34.14\]#] make -j16

 

5\.

安装内核模块

\[root@localhost linux-2.6.34.14\]# make modules_install -j2

 

6\.

安装内核

\[root@localhost linux-2.6.34.14\]# make install -j2

sh /usr/src/linux-2.6.34.14/arch/x86/boot/install.sh 2.6.34.14 arch/x86/boot/bzImage \\

System.map \"/boot\"

ERROR: modinfo: could not find module nf_defrag_ipv6

ERROR: modinfo: could not find module vmhgfs

ERROR: modinfo: could not find module vsock

ERROR: modinfo: could not find module vmci

// 发现有些模块找不到，上述找不到的module，从ISO安装包，或者网上找到后复制到/sys/module中。

 

 

 

已使用 OneNote 创建。
