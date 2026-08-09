==》RHEL9 VNC 部署

2024年1月15日

14:35

RHEL9 VNC

<https://itstorage.net/index.php/ldce/guie/562-linuxguivnc>

 

[\[root@secondary1 \~\]# cat /etc/os-release]

NAME=\"Red Hat Enterprise Linux\"

VERSION=\"9.1 (Plow)\"

ID=\"rhel\"

ID_LIKE=\"fedora\"

VERSION_ID=\"9.1\"

PLATFORM_ID=\"platform:el9\"

PRETTY_NAME=\"Red Hat Enterprise Linux 9.1 (Plow)\"

ANSI_COLOR=\"0;31\"

LOGO=\"fedora-logo-icon\"

CPE_NAME=\"cpe:/o:redhat:enterprise_linux:9::baseos\"

HOME_URL=\"https://www.redhat.com/\"

DOCUMENTATION_URL=\"https://access.redhat.com/documentation/red_hat_enterprise_linux/9/\"

BUG_REPORT_URL=\"https://bugzilla.redhat.com/\"

 

REDHAT_BUGZILLA_PRODUCT=\"Red Hat Enterprise Linux 9\"

REDHAT_BUGZILLA_PRODUCT_VERSION=9.1

REDHAT_SUPPORT_PRODUCT=\"Red Hat Enterprise Linux\"

REDHAT_SUPPORT_PRODUCT_VERSION=\"9.1\"

 

[\[root@secondary1 \~\]# dnf install tigervnc-server xorg-x11-fonts-Type1]

 

[\[root@secondary1 \~\]# useradd sysadmin]

 

[\[root@secondary1 \~\]# su - sysadmin]

 

![[Depoly_001_==》RHEL9 VNC 部署_001.png]]

 

[\[sysadmin@secondary1 \~\]\$ chmod u+x /home/sysadmin/.vnc/xstartup]

\[sysadmin@secondary1 \~\]\$

[\[sysadmin@secondary1 \~\]\$ mv \~/.vnc/xstartup \~/.vnc/xstartup.backup]

 

[\[sysadmin@secondary1 \~\]\$ cat \~/.vnc/xstartup]

#!/bin/bash

xrdb \$HOME/.Xresources

startxfce4 &

 

[\[sysadmin@secondary1 \~\]\$ cp /etc/X11/Xresources \~/.Xresources]

\[sysadmin@secondary1 \~\]\$

[\[sysadmin@secondary1 \~\]\$ chmod +x \~/.vnc/xstartup]

 

[\[root@secondary1 \~\]# cat /etc/tigervnc/vncserver.users]

\# TigerVNC User assignment

\#

\# This file assigns users to specific VNC display numbers.

\# The syntax is \<display\>=\<username\>. E.g.:

\#

\# :2=andrew

\# :3=lisa

:2=sysadmin

 

Firewall:

\# firewall-cmd \--permanent \--add-port=5902/tcp

\# firewall-cmd \--reload

 

[\[root@secondary1 \~\]# grep -E \'AllowTcpForwarding\|GatewayPorts\' /etc/ssh/sshd_config]

#AllowTcpForwarding yes

AllowTcpForwarding yes

#GatewayPorts no

GatewayPorts yes

#        AllowTcpForwarding no

 

[\[root@secondary1 \~\]# restorecon -RFv /home/sysadmin/.vnc]

[\[root@secondary1 \~\]# systemctl start] vncserver@:2.service

[\[root@secondary1 \~\]# systemctl enable] vncserver@:2.service

Created symlink /etc/systemd/system/multi-user.target.wants/vncserver@:2.service → /usr/lib/systemd/system/vncserver@.service.

 

![[Depoly_001_==》RHEL9 VNC 部署_002.png]]

 

 

[\[root@secondary1 \~\]# ss -tnplan \|grep \'5902\*\']

LISTEN 0      5            0.0.0.0:5901      0.0.0.0:\*     users:((\"Xvnc\",pid=42336,fd=6))                         

LISTEN 0      5            0.0.0.0:5902      0.0.0.0:\*     users:((\"Xvnc\",pid=43360,fd=6))                         

LISTEN 0      5               [\[::\]:5901]         [\[::\]:\*]     users:((\"Xvnc\",pid=42336,fd=7))                         

LISTEN 0      5               [\[::\]:5902]         [\[::\]:\*]     users:((\"Xvnc\",pid=43360,fd=7))  

 

 

Windows 端 VNC 软件登录测试：

![[Depoly_001_==》RHEL9 VNC 部署_003.png]]

 

 

![[Depoly_001_==》RHEL9 VNC 部署_004.png]]

 

 

![[Depoly_001_==》RHEL9 VNC 部署_005.png]]

 

 

已使用 OneNote 创建。
