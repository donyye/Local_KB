Are Broadcom 5719 or 5720 devices supported?

Friday, July 18, 2014

9:50 AM

客户环境：

RHEL 6.1

 

编译 Broadcom 5720 NIC issue

 

solution ：

In order for the Broadcom 57xx devices to be supported under RHEL 6, Red Hat Enterprise Linux 6.2 or later is required.

 

客户编译时出错

\[root@SGNTTNFS01 SPECS\]# rpmbuild -bb tg3.spec

Executing(%prep): /bin/sh -e /var/tmp/rpm-tmp.07YZ15

\+ umask 022

\+ cd /root/rpmbuild/BUILD

\+ cd /root/rpmbuild/BUILD

\+ rm -rf tg3-3.133d

\+ /usr/bin/bzip2 -dc /root/rpmbuild/SOURCES/tg3-3.133d.tar.bz2

\+ /bin/tar -xvvf -

drwxr-xr-x root/root         0 2013-08-08 06:48 tg3-3.133d/

-rw-r\--r\-- root/root    131321 2013-08-08 06:48 tg3-3.133d/tg3.h

-rw-r\--r\-- root/root      5750 2013-08-08 06:48 tg3-3.133d/tg3_vmware.h

-rw-r\--r\-- root/root     12652 2013-08-08 06:48 tg3-3.133d/README.TXT

-rw-r\--r\-- root/root    571225 2013-08-08 06:48 tg3-3.133d/ChangeLog

-rw-r\--r\-- root/root     38349 2013-08-08 06:48 tg3-3.133d/tg3_vmware.c

-rw-r\--r\-- root/root     11315 2013-08-08 06:48 tg3-3.133d/tg3_compat2.h

-rw-r\--r\-- root/root     46650 2013-08-08 06:48 tg3-3.133d/tg3_compat.h

-rw-r\--r\-- root/root    508371 2013-08-08 06:48 tg3-3.133d/tg3.c

-rw-r\--r\-- root/root     15153 2013-08-08 06:48 tg3-3.133d/LICENSE

-rw-r\--r\-- root/root     12875 2013-08-08 06:48 tg3-3.133d/makeflags.sh

-rw-r\--r\-- root/root     45311 2013-08-08 06:48 tg3-3.133d/tg3_firmware.h

-rw-r\--r\-- root/root      4027 2013-08-08 06:48 tg3-3.133d/tg3.4

-rw-r\--r\-- root/root      2223 2013-08-08 06:48 tg3-3.133d/esx_ioctl.h

-rw-r\--r\-- root/root      4041 2013-08-08 06:48 tg3-3.133d/Makefile

\+ STATUS=0

\+ \'\[\' 0 -ne 0 \'\]\'

\+ cd tg3-3.133d

\+ /bin/chmod -Rf a+rX,u+w,g-w,o-w .

\+ exit 0

Executing(%build): /bin/sh -e /var/tmp/rpm-tmp.ZGRxXN

\+ umask 022

\+ cd /root/rpmbuild/BUILD

\+ cd tg3-3.133d

\+ value=

\+ \'\[\' -z \'\' \'\]\'

++ uname -r

\+ KVER=2.6.32-131.0.15.el6.x86_64

\+ make KVER=2.6.32-131.0.15.el6.x86_64

sh makeflags.sh   \> tg3_flags.h

makeflags.sh: No kernel source directory provided.

make: \*\*\* \[tg3_flags.h\] Error 255

error: Bad exit status from /var/tmp/rpm-tmp.ZGRxXN (%build)

 

RPM build errors:

    Bad exit status from /var/tmp/rpm-tmp.ZGRxXN (%build)

\[root@SGNTTNFS01 SPECS\]#

 

红帽KB:

[https://access.redhat.com/knowledge/solutions/73713](https://access.redhat.com/knowledge/solutions/73713)

 

已使用 OneNote 创建。
