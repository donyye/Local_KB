Wednesday, May 21, 2014

5:19 PM

<https://access.redhat.com/site/solutions/726353>

 

 

Issue

 

    Why does the system log displays the message: \"Could not enable interrupts, failed set, using polled mode.\"? On starting the IPMI service on Dell PowerEdge R620, T620, M620, R720, and R720xd systems, the system log displays the message.

 

 

 

 

 

 

错误信息

    关于启用IPMI驱动程序在安装OMSA

描述

    在开始在Dell PowerEdge R620，T620，M620，R720，和R720xd系统IPMI服务，系统日志显示消息：无法允许中断，失败集，使用轮询模式。

解决方法

    这是事先设计好的。这将在iDRAC固件的更高版本中得到解决。

原因

    虽然该消息表明操作系统处于轮询模式下，Linux的驱动程序继续在中断模式下工作。

 

 

 

已使用 OneNote 创建。
