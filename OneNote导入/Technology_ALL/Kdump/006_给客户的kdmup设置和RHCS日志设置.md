给客户的kdmup设置和RHCS日志设置

Monday, May 05, 2014

11:04 AM

1. kdump 修改

1)安装包

\# yum install kernel-kdump

 

2）修改/boot/grub/grub.conf

default=0

timeout=5

splashimage=(hd0,0)/grub/splash.xpm.gz

hiddenmenu

title Red Hat Enterprise Linux Server (2.6.32-220.4.2.el6.x86_64)

        root (hd0,0)

        kernel /vmlinuz-2.6.32-220.4.2.el6.x86_64 root=/dev/mapper/rootvg-LogVol00 ro rd_LVM_LV=rootvg/LogVol06 rd_NO_LUKS rd_LVM_LV=rootvg/LogVol00 rd_NO_MD crashkernel=768M  quiet rhgb LANG=zh_CN.UTF-8 KEYBOARDTYPE=pc KEYTABLE=us rd_NO_DM

\# 注意红色修改部分，字体建议使用UTF8中文字符集。

 

3）重启kdump

\# /etc/init.d/kdump restart

 

4）测试

\# echo c \> /proc/sysrq-trigger

 

如果设置后kdump日志还是不能生成，可以使用Redhat 官方提供的kdump script 。

[https://access.redhat.com/labs/kdumphelper/](https://access.redhat.com/labs/kdumphelper/)

 

![[Technology_ALL_Kdump_006_给客户的kdmup设置和RHCS日志设置_001.jpg]]

 

2. 提高系统日志级别

修改红色部分

\*.debug;mail.none;authpriv.none;cron.none                /var/log/messages

 

\# /etc/init.d/rsyslog restart

 

3. cluster debug log 设置

首先备份cluster.conf文件，在logging 标签下修改和添加。

\# vim /etc/cluster/cluster.conf

 

\<logging logfile_priority=\"debug\" syslog_facility=\"daemon\" syslog_priority=\"info\" to_logfile=\"yes\" to_syslog=\"yes\"\>

        \<logging_daemon logfile=\"/var/log/cluster/qdiskd.log\" name=\"qdiskd\"/\>

        \<logging_daemon logfile=\"/var/log/cluster/fenced.log\" name=\"fenced\"/\>

        \<logging_daemon logfile=\"/var/log/cluster/rgmanager.log\" name=\"rgmanager\"/\>

        \<logging_daemon logfile=\"/var/log/cluster/corosync.log\" name=\"corosync\"/\>

    \</logging\>

 

 

修改来源文章：[https://access.redhat.com/site/solutions/414883](https://access.redhat.com/site/solutions/414883)

 

 

 

已使用 OneNote 创建。
