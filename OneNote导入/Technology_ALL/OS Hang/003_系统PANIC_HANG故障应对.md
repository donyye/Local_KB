系统PANIC/HANG故障应对

Thursday, December 12, 2013

5:00 PM

在使用操作系统的过程中，可能会遇到如下几种较为严重的系统故障：

1） 系统无响应，屏幕上没有信息出现，按下键盘不会有输出，ping不通等。

2） 系统kernel panic，屏幕上出现oops信息，ping不通。

3） 系统能够响应键盘输入，但是很多命令键入之后会挂起。

这三种情况都可能导致生产环境中的应用无法正常运行，产生严重的后果。

下面会对这三种情况出现的现象以及相应的措施进行详细的介绍。

 

第一、名词解释

系统无响应的情况从表征上可分为两种：可中断挂起和不可中断挂起。

可中断挂起：系统当时应用已无法正常提供，无法通过远程访问（ssh,telnet等）的方式访问，

但是系统可以响应部分中断信号，如从其他机器可以ping通该系统，直连的键盘上的数字键盘

灯/大写字母锁定键灯可响应亮灭，鼠标有响应，可切换到其他终端窗口

（Ctrl+Alt+F2/F3/F4/F5/F6/F7等）。该种情况属于可中断挂起。

注意：如果通过KVM连接各系统，出现该问题时，需要将键盘直接到该机进行测试判断。

不可中断挂起：系统当时应用已无法正常提供，且上述的所有操作均无响应，基本可判断为不

可中断挂起。

Kernel panic：译为内核恐慌。当发生严重的错误而且被内核捕捉到时,内核将会停止所有的进

程并且送出内核恐慌信息。发生内核恐慌的理由很多。其中最常见的是,忘了为内核指定它的根

文件系统的位置。内核恐慌实际上是内核在捕捉到严重错误时会调用panic函数进行处理。最

终打印出kernel panic的信息。

内存信息转储（core dump）：当系统出现无响应时，kernel已无法向日志中打印出错信息，故

而系统重启后收集的sosreport中很难从日志里分析出系统无响应的具体原因。此时需要收集系

统无响应当时内存里面的信息以帮助工程师进行具体原因的诊断。内存里面的信息只有通过特

殊的配置：netdump/diskdump/kdump并结合kernel panic才能获取到，最终获取的文件名为

vmcore。

netdump/diskdump/kdump: 这三种方法最终实现的都是将出现系统故障当时的内存信息转储到

指定的位置，只是实现的机制不同。

Netdump：适用于RHEL3/4，通过网络的方式将出现故障的系统的内存信息转储到指定的server

上，转储完成后系统会自动重启。

Diskdump：适用于RHEL3/4，将出现故障的系统的内存信息转储到本地的一个独立的分区/磁盘

上，转储完成后需要手动重启系统。

Kdump：适用于RHEL5，替换之前的netdump/diskdump，可支持网络方式（ssh/nfs等）或者本

地方式将出现故障的系统的内存信息转储到指定位置。转储完成后系统会自动重启。

以上的三种系统故障的原因分析都需要依赖vmcore文件。所以dump的配置是必需的且需要提

前设置完成并测试成功。

 

 

第二、预设置：netdump/diskdump/kdump的配置和测试

1\. netdump

使用条件：

1） 系统为RHEL3/4，相关 的软件包有netdumpserver

和netdump。

2） 需要至少两台机器，通常被监控的机器作为netdump的客户端，而另一台作为服务器。

注意：在服务器端需要设置netdump的密码，而在客户端需要有一步来输入netdump的密码。

3）需要两台机器网络正常，且网卡能够支持netdump。

查看网卡是否支持 netdump功能，请参见：http://kbase.redhat.com/faq/docs/DOC3926

4）服务器目录/var/目录剩余空间大小\> netdump客户端的内存大小。生成的core文件保存于服

务器/var/crash目录下，文件名为vmcore。

Netdump的配置方法请参考：

<http://kbase.redhat.com/faq/docs/DOC6913>

Netdump的测试方法请参考：

<http://kbase.redhat.com/faq/docs/DOC6855>

2\. diskdump

使用条件：

1） 系统为RHEL3/4，相关的软件包为diskdumputils。

2） 可存放vmcore文件的分区，也可以使用swap分区。

3） 因为diskdump是直接访问磁盘驱动器，所以不能使用通过LVM或者RAID管理的设备作为

dump内存信息的设备。

4） 收集的core文件存放在该系统的/var/crash目录下，所以需要该目录下可用空间\>内存大小。

5） 当dump内存信息完成后，需要手动重启系统以确保所有信息已被写入系统的/var/crash目录

下。

Diskdump的配置方法请参考：

<http://kbase.redhat.com/faq/docs/DOC7075>

Diskdump的测试方法同Netdump。

参考文档：

/usr/share/doc/diskdumputils\<

version\>/R

3\. kdump

使用条件：

1） 系统为RHEL5及以上，相关软件包为kexectools

。

2） 需要单独的内存空间，一般128M足够，在16M之后。该部分内存在系统正常运行时无法

使用。

Kdump的配置方法请参考：

<http://kbase.redhat.com/faq/docs/DOC6039>

Kdump的测试方法同Netdump。

参考文档：

/usr/share/doc/kexectools\<version\>/kexeckdumphowto.txt

 

注意：

1）内存信息的转储时间跟内存的大小以及当时的网络情况（如果用网络方式转储）有关系。

2）对于需要手动重启完成转储操作的dump,需要待转储完成再重启系统。

2）尽管有的dump操作可支持只dump一部分内存，但由于在发生问题当时无法判断问题所在，

故而建议将所有内存都dump出来以备分析。

 

 

第三、如何触发dump并生成vmcore

默认情况下，系统panic会自行触发dump去产生vmcore, 但是在系统freeze或者是系统livelock

的情况下，必须通过手动产生信号给kernel触发一个dump。

通常触发的方法有如下几个步骤。

 

第四，通过nmi_watchdog触发netdump

nmi_watchdog 会通过NMI信号定期检查Linux kernel是否正常，如果nmi_watchdog认为kernel

在一段时间内(根据kernel版本不同，这个数值是5秒或者是15秒)都不正常的话，

nmi_watchdog就会触发panic。

使用条件：

1）内核支持APIC

2）32位系统需要添加内核启动参数来打开nmi_watchdog。

4）64位系统默认已打开nmi_watchdog。

nmi_watchdog的解释和配置方法请参考：

<http://kbase.redhat.com/faq/docs/DOC4128>

 

参考文档：

/usr/share/doc/kerneldoc\<version\>/Documentation/nmi_watchdog.txt

第五、使用nmi按钮触发panic

使用条件和nmi_watchdog类似。但是需要通过手动按压nmi按钮产生nmi信号触发panic.

设置文件是/proc/sys/kernel/unknown_nmi_panic,

echo 1 \> /proc/sys/kernel/unknown_nmi_panic 就能开启这个功能。

但是这个参数和nmi_watchdog无法同时使用。

 

第六、出现系统故障时的现场操作

当出现系统故障时，现场信息的收集对于系统故障的区分和后续的分析有很大的帮助。

基本思路为：

当前故障为哪种类型 \>执行该类型对应操作收集信息\>重启系统，收集sosreport \>联系800

 

1\. 确定当前故障类型并采用对应措施收集信息

判断方法如下：

1）是否能远程登录，如ssh,telnet 等；

2）是否能 ping 通物理IP地址；

3）本地鼠标、键盘是否可响应；

4）本地是否能登录；具体可通过 Ctrl+Alt+F(1,2,3,4,5,6)，尝试从本地虚拟终端是否能登录

5）如果能登录，登录后是否可以执行一些基本命令操作，如ls, pwd, top, ps, df等。

1） 如果以上五条有任何一条满足，则系统处于可中断挂起状态。该情况下请按照以下说明操作：

依次执行组合键：

Alt + SysRq + m

Alt + SysRq + t

Alt + SysRq + p

以上组合键请执行三遍，每次时间间隔三分钟左右。

将每次执行完屏幕上打印的信息截取下来保存。

做完以上操作后，再执行 Alt+SysRq+c，触发kernel panic，通过之前的dump配置的配合转储内

存信息生成vmcore文件。

各个魔术键的用法请参考：

<http://kbase.redhat.com/faq/docs/DOC11884>

2） 如果上述情况均不符合，而且系统也没有直接打印kernel panic或者crash字样，那么系统

处于不可中断挂起状态。该状态下无需手动操作，只需通过之前的dump配置和nmi_watchdog

的配合来触发kernel panic收集内存转储文件vmcore。

3）如果屏幕上直接打印kernel panic或者系统crash字样，那么此时系统会通过之前的dump配

置将内存信息转储到指定位置。

 

2\. 收集完上述信息后，重启系统，并立即收集sosreport/sysreport。

Sosreport(RHEL4U6及以上版本) / sysreport(RHEL4U6之前版本) 都是系统分析的工具，作为一

个命令，它会收集系统日志和一部分配置文件以及安装包的校验结果，默认生成的是tar.bz2结

尾的文件。

 

3\. 将sosreport和vmcore文件传递给技术支持工程师。

 

已使用 OneNote 创建。
