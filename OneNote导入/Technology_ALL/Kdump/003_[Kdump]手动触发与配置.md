[\[Kdump\]]手动触发与配置

Friday, January 03, 2014

8:50 AM

配置netconsole和kdump，收集信息以便在下次发生hang机收集到足够的信息进行root cause分析。步骤如下：

 

===========

注意下述步骤中 ：

echo c \> /proc/sysrq-trigger 表示通过手动的方式触发 kernel panic，该操作会造成系统宕机，系统的所有应用都将停止运行，所以需在合适的宕机时间操作。

===========

 

一、如何配置 kdump

红帽企业版Linux上需要配置 kdump，配置方法请参考： How do I configure kexec/kdump on Red Hat Enterprise Linux?

[https://access.redhat.com/kb/docs/DOC-6039](https://access.redhat.com/kb/docs/DOC-6039)

 

二、打开 sysrq

编辑/etc/sysctl.conf 文件：

kernel.sysrq = 1[     ]\\\\[  ]激活 Magic SysRq否则，键盘鼠标没有响应.

修改之后，使用如下命令使配置永久生效：

\# sysctl -p

可使用命令 sysctl -a 查看配置是否生效。

 

打开 sysrq 的主要目的是可使用魔术键打印系统当时的信息。关于魔术键的使用，请参考：

What is the SysRq facility and how do I use it?   [http://kbase.redhat.com/faq/docs/DOC-2024\<http://kbase.redhat.com/faq/docs/DOC-2024](http://kbase.redhat.com/faq/docs/DOC-2024)\>

 

三、打开 netconsole

为了能在出现问题时，收集到更多屏幕上的打印信息，建议打开 netconsole 功能。

netconsole 的配置方法请参考：

配置所有节点上的netconsole文件。

How do I configure netconsole?

[http://kbase.redhat.com/faq/docs/DOC-4259](http://kbase.redhat.com/faq/docs/DOC-4259)

设定一个rsyslogd服务器用于接收客户端的netconsole日志:

[https://access.redhat.com/kb/docs/DOC-54364](https://access.redhat.com/kb/docs/DOC-54364)

 

四、测试 kdump 是否配置成功

执行如下命令对需要进行内存转储的机器模拟产生 kernel panic：

\# echo c \> /proc/sysrq-trigger

正常情况下，系统会在触发 kernel panic 之后将内存信息进行转储。如果配置正常，转储完成系统重启之后会在指定位置找到文件vmcore，使用 ls 查看大小与内存大小相当。

该操作的目的有两个：

1. 确定 dump 配置成功，可生成 vmcore 文件。

2. 确定生成 vmcore 文件所需要的时间，以便确定在生产系统中是否允许这样的 宕机时间。

注意 ：

echo c \> /proc/sysrq-trigger 表示通过手动的方式触发 kernel panic，该操作会造成系统宕机，系统的所有应用都将停止运行，所以需在合适的宕机时间操作。

 

已使用 OneNote 创建。
