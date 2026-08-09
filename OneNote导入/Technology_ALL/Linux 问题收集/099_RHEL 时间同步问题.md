RHEL 时间同步问题

2024年5月15日

16:58

<https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-configuring_ntp_using_the_chrony_suite>

 

rtcsync

The rtcsync directive is present in the /etc/chrony.conf file by default. This will inform the kernel the system clock is kept synchronized and the kernel will update the real-time clock every 11 minutes. 

 

 

RTC（Real-Time Clock） 就是硬件时钟，

在系统启动时，操作系统会读取 RTC 的时间并设置系统时钟。通过 rtcsync 指令，系统时钟与 RTC 可以保持同步。

RTC 是硬件级别的时钟，不依赖于操作系统，可以独立运行。

 

 

默认情况下，NTP 配置里是有自动同步到硬件时钟的。

上面就是说明

 

 

已使用 OneNote 创建。
