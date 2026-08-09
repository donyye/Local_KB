Ubuntu-基础设置

2024年6月13日

15:31

Ubuntu 20.04

 

1. 配置静态IP地址：

<https://www.myfreax.com/how-to-configure-static-ip-address-on-ubuntu-20-04/>

root@dony:\~# vim /etc/netplan/00-installer-config.yaml

network:

  renderer: networkd   \# 有线配置

  ethernets:

    ens160:    #指定需配置网络接口的名称

      dhcp4: no  #是否打开 IPv4 的 dhcp，true 就是开启。

      addresses:

        - 192.168.200.165/24    #定义网络接口的静态 IP 地址

      gateway4: 192.168.200.254

      nameservers:

              addresses: \[8.8.8.8,1.1.1.1\]

network:

  renderer: networkd

  ethernets:

    ens192:

      dhcp4: no

      addresses:

        - 10.10.40.165/16

      gateway4: 10.10.40.10

      nameservers:

              addresses: \[10.7.7.7,10.8.8.8\]

  version: 2

 

root@dony:\~# netplan apply   \--》 运行完抽配置生效

\#需要注意是这里的缩进需要严格遵守否则运行 netplan apply命令后就会报错。

截图：

![[__Ubuntu___002_Ubuntu-基础设置_001.png]]

 

单口配置多IP和网关配置：

network:

  version: 2

  renderer: networkd

  ethernets:

    eno1:

      addresses:

[        - 10.120.146.100/16] \#为 eno1 接口分配的第一个 IP 地址和子网掩码。

[        - 172.16.100.79/16] \#为eno1 接口分配的第二个 IP 地址和子网掩码。

[      routes:][   ][ \# ]静态路由配置

[        - to: 0.0.0.0/0][  ]\# 默认路由（相当于to: 0.0.0.0/0）

[          via: 10.120.146.1][  ]\# 指定第一个 IP 地址的网关

[          on-link: true] \# on-link: true 确保路由器知道这些路由是直接连接的。

        - to: 0.0.0.0/0

[          via: 172.16.99.254][  ]\# 指定第二个 IP 地址的网关

          on-link: true

      nameservers:

        addresses:

          - 8.8.8.8

\# ip add

2: eno1: \<BROADCAST,MULTICAST,UP,LOWER_UP\> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000

    link/ether 18:03:73:b6:64:ed brd ff:ff:ff:ff:ff:ff

    altname enp0s25

    inet 10.120.146.100/23 brd 10.120.147.255 scope global noprefixroute eno1

       valid_lft forever preferred_lft forever

    inet 172.16.100.79/16 brd 172.16.255.255 scope global noprefixroute eno1

       valid_lft forever preferred_lft forever

 

 

无线网卡配置：

[  wifis:   #无线配置\
    wlan0:   #无线网络接口名称\
      dhcp4: true   #是否打开IPV4的dhcp\
      access-points:  \
         \"TP-LINK\":  #连接的无线的名称\
             password: \"123123123\"  #连接的无线的密码]

 

重启网络命令

root@dony:\~# service networkd-dispatcher restart

\# 网络配置后重启无法生效，要运行 netplan apply 才会生效

 

关闭防火墙关闭与开启

root@dony:\~# ufw disable   \--》【enable】开启，【status】查看状态

Firewall stopped and disabled on system startup

 

NTP客户端配置

root@dony:\~# apt install ntpdate -y

root@dony:\~# timedatectl

root@dony:\~# vim /etc/ntp.conf    \--\> 设置客户端同步的服务器

pool 10.10.40.212

root@dony:\~# systemctl restart ntp.service    \-\--\> 重启ntp服务

root@dony:\~# systemctl enable ntp    \--\> 开机启动

root@dony:\~# systemctl is-enable ntp   \--\> 检查是否已经设置开机启动

Unknown operation is-enable.

root@dony:\~# ntpq -p

 

修改时区

![[__Ubuntu___002_Ubuntu-基础设置_002.png]]

root@dony:\~# ls -l /etc/localtime 

lrwxrwxrwx 1 root root 27 Feb  1 17:24 /etc/localtime -\> /usr/share/zoneinfo/Etc/UTC

root@dony:\~# cat /etc/localtime 

TZif2UTCTZif2UTC

UTC0

root@dony:\~# timedatectl list-timezones \|grep -i shang   

Asia/Shanghai   

root@dony:\~# timedatectl set-timezone Asia/Shanghai      \--\>设置时区

Or

ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

 

开启root用户ssh直接登录(20.04)

\# vim /etc/ssh/sshd_config

PermitRootLogin yes[  \<\-- ]添加这项

\# systemctl restart sshd

 

 

Ubuntu 24.04 安装图形化界面

一、确保apt源可用并更新源

sudo apt-get update

二、安装桌面图形化显示

sudo apt-get install ubuntu-desktop

三、设置默认开启方式为图形化界面显示

sudo systemctl set-default graphical.target

切换到多用户登录

sudo systemctl set-default multi-user.target

 

四，重启桌面服务

sudo systemctl restart gdm

 

 

 

 

安装kvm

sudo apt install cpu-checker[   \<]\-\--使用KVM加速

sudo apt -y install qemu-kvm libvirt-daemon-system libvirt-daemon virtinst bridge-utils libosinfo-bin

 

 

 

==== ubuntu 22.04 与 24.04 的网络配置 ====

 

\# cat /etc/netplan/00-installer-config.yaml

\# This is the network config written by \'subiquity\'

network:

  version: 2

  ethernets:

    ens33:

      dhcp4: no

      addresses:

        - 10.10.40.126/16

      routes:

         - to: default

[           via: 10.120.146.203][    \<\-- ]默认网关

[     ][  ][ ][ on-link: true][  ][ \<\-- ]添加这个选项 ip route 才会有显示，否则没有

      nameservers:

             addresses: \[10.7.7.7,10.8.8.8\]

\# 默认路由问题参考\
[https://blog.csdn.net/zhouqi621/article/details/145748463?utm_medium=distribute.pc_relevant.none-task-blog-2\~default\~baidujs_baidulandingword\~default-0-145748463-blog-135415159.235\^v43\^control&spm=1001.2101.3001.4242.1&utm_relevant_index=2](https://blog.csdn.net/zhouqi621/article/details/145748463?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-145748463-blog-135415159.235%5ev43%5econtrol&spm=1001.2101.3001.4242.1&utm_relevant_index=2)

 

 

第二添加默认路由方式是通过 rc.local

添加一个不同子网为默认路由：

sudo ip route add default via 10.120.146.203 dev ens33 onlink

\# 命令添加"onlink"参数： ip route 命令，加入"onlink"选项，这样即使网关在另一个子网，也能允许设置默认路由。如果不加 onlink 添加会报错。

 

Ubuntu 没有 rc.local 可以通过下面的方式自己添加：

echo \'#!/bin/bash\' \> /etc/rc.local

chmod 755 /etc/rc.local

systemctl start rc-local

systemctl enable rc-local

 

 

已使用 OneNote 创建。
