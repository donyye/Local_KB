K8s-deploy-1

星期一, 2025年12月29日

下午 4:26

 

基本环境配置

表1-1  高可用Kubernetes集群规划

  -------------------- ---------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------
  主机名               IP地址   说明
  k8s-master01 ~ 03   10.10.40.201 ~ 203                                                                master节点 * 3
  k8s-master-lb        10.10.40.236                                                                       keepalived虚拟IP
  k8s-node01 ~ 02     10.10.40.204 ~ 205                                                                worker节点 * 2
  -------------------- ---------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------

请统一替换这些网段，Pod网段和service和宿主机网段不要重复！！！

  ---------------------------------------------------------------------------------------------------------------------- ----------------
  配置信息                                                                                   备注
  系统版本                                                                                                               CentOS 7.9
  Docker版本    20.10.x
  Pod网段       172.16.0.0/12
  Service网段                                  192.168.0.0/16
  ---------------------------------------------------------------------------------------------------------------------- ----------------

注意

宿主机网段、K8s Service网段、Pod网段不能重复，具体看课程资料的【安装前必看】集群安装网段划分

VIP（虚拟IP）不要和公司内网IP重复，首先去ping一下，不通才可用。VIP需要和你的主机在同一个局域网内（不是直接用我的VIP）！

公有云上搭建VIP是公有云的负载均衡的IP，比如阿里云的内网SLB的地址，腾讯云内网ELB的地址。不需要再搭建keepalived和haproxy

所有节点配置hosts，修改/etc/hosts如下：

需要替换时，请统一替换这些IP地址！！！

[root@k8s-master01 ~]# cat /etc/hosts

10.10.40.201 k8s-master01

10.10.40.202 k8s-master02

10.10.40.203 k8s-master03

10.10.40.236 k8s-master-lb  # 如果不是高可用集群，该IP为Master01的IP

10.10.40.204 k8s-node01

10.10.40.205 k8s-node02

CentOS 7安装yum源如下：

# curl -o /etc/yum.repos.d/CentOS-Base.repo <https://mirrors.aliyun.com/repo/Centos-7.repo>

# yum install -y yum-utils device-mapper-persistent-data lvm2

# yum-config-manager --add-repo <https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo>

# cat \<\<EOF \> /etc/yum.repos.d/kubernetes.repo

[[kubernetes]]

name=Kubernetes

baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/

enabled=1

gpgcheck=0

repo_gpgcheck=0

gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg [https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg](https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg)

EOF

# sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo

必备工具安装

# yum install wget jq psmisc vim net-tools telnet yum-utils device-mapper-persistent-data lvm2 git -y

所有节点关闭防火墙、selinux、dnsmasq、swap。服务器配置如下：

# systemctl disable --now firewalld 

# systemctl disable --now dnsmasq

 Diskable selinux:

# setenforce 0

# sed -i 's#SELINUX=enforcing#SELINUX=disabled#g' /etc/selinux/config

关闭swap分区

# swapoff -a && sysctl -w vm.swappiness=0

# sed -ri '/^[^#]*swap/s@^@#@' /etc/fstab

所有节点同步时间。时间同步配置如下：

# vim /etc/chrony.conf

server 10.10.40.10 iburst

所有节点配置limit：

# ulimit -SHn 65535

 

末尾添加如下内容

# vim /etc/security/limits.conf

* soft nofile 65536

* hard nofile 131072

* soft nproc 65535

* hard nproc 655350

* soft memlock unlimited

* hard memlock unlimited

Master01节点免密钥登录其他节点，安装过程中生成配置文件和证书均在Master01上操作，集群管理也在Master01上操作，阿里云或者AWS上需要单独一台kubectl服务器。密钥配置如下：

# ssh-keygen -t rsa

# for i in k8s-master01 k8s-master02 k8s-master03 k8s-node01 k8s-node02;do ssh-copy-id -i .ssh/id_rsa.pub $i;done

下载安装所有的源码文件：

# cd /root/ ; git clone <https://github.com/dotbalo/k8s-ha-install.git>

如果无法下载就下载：https://gitee.com/dukuan/k8s-ha-install.git

[root@k8s-master01 ~]# ll k8s-ha-install/

total 24

-rw-r--r-- 1 root root 18092 Dec 17 09:23 LICENSE

drwxr-xr-x 2 root root    29 Dec 17 09:23 metrics-server-0.3.7

drwxr-xr-x 2 root root  227 Dec 17 09:23 metrics-server-3.6.1

-rw-r--r-- 1 root root  379 Dec 17 09:23 README.md

所有节点升级系统并重启，此处升级没有升级内核，下节会单独升级内核：

# yum update -y --exclude=kernel* && reboot 

CentOS7需要升级，CentOS8可以按需升级系统

 内核配置

CentOS7 需要升级内核至4.18+，本地升级的版本为4.19

在master01节点下载内核：(购买架构师课程的可以从百度网盘下载)

# cd /root

# wget <http://193.49.22.109/elrepo/kernel/el7/x86_64/RPMS/kernel-ml-devel-4.19.12-1.el7.elrepo.x86_64.rpm>

# wget <http://193.49.22.109/elrepo/kernel/el7/x86_64/RPMS/kernel-ml-4.19.12-1.el7.elrepo.x86_64.rpm>

如果无法下载可以使用，有下载好现成的。

从master01节点传到其他节点：

# for i in k8s-master02 k8s-master03 k8s-node01 k8s-node02;do scp kernel-ml-4.19.12-1.el7.elrepo.x86_64.rpm kernel-ml-devel-4.19.12-1.el7.elrepo.x86_64.rpm $i:/root/ ; done

所有节点安装内核

# cd /root && yum localinstall -y kernel-ml*

所有节点更改内核启动顺序

# grub2-set-default  0 && grub2-mkconfig -o /etc/grub2.cfg

# grubby --args="user_namespace.enable=1" --update-kernel="$(grubby --default-kernel)"

检查默认内核是不是4.19

# grubby --default-kernel

/boot/vmlinuz-4.19.12-1.el7.elrepo.x86_64

所有节点重启，然后检查内核是不是4.19

# uname -a

Linux k8s-master02 4.19.12-1.el7.elrepo.x86_64 #1 SMP Fri Dec 21 11:06:36 EST 2018 x86_64 x86_64 x86_64 GNU/Linux

所有节点安装ipvsadm：

# yum install ipvsadm ipset sysstat conntrack libseccomp -y

所有节点配置ipvs模块，在内核4.19+版本nf_conntrack_ipv4已经改为nf_conntrack， 4.18以下使用nf_conntrack_ipv4 即可：

modprobe -- ip_vs

modprobe -- ip_vs_rr

modprobe -- ip_vs_wrr

modprobe -- ip_vs_sh

modprobe -- nf_conntrack

 # 加入以下内容

# vim /etc/modules-load.d/ipvs.conf 

ip_vs

ip_vs_lc

ip_vs_wlc

ip_vs_rr

ip_vs_wrr

ip_vs_lblc

ip_vs_lblcr

ip_vs_dh

ip_vs_sh

ip_vs_fo

ip_vs_nq

ip_vs_sed

ip_vs_ftp

ip_vs_sh

nf_conntrack

ip_tables

ip_set

xt_set

ipt_set

ipt_rpfilter

ipt_REJECT

ipip

然后执行 systemctl enable --now systemd-modules-load.service 即可

开启一些k8s集群中必须的内核参数，所有节点配置k8s内核：

# cat \<\<EOF \> /etc/sysctl.d/k8s.conf

net.ipv4.ip_forward = 1

net.bridge.bridge-nf-call-iptables = 1

net.bridge.bridge-nf-call-ip6tables = 1

fs.may_detach_mounts = 1

net.ipv4.conf.all.route_localnet = 1

vm.overcommit_memory=1

vm.panic_on_oom=0

fs.inotify.max_user_watches=89100

fs.file-max=52706963

fs.nr_open=52706963

net.netfilter.nf_conntrack_max=2310720

 

net.ipv4.tcp_keepalive_time = 600

net.ipv4.tcp_keepalive_probes = 3

net.ipv4.tcp_keepalive_intvl =15

net.ipv4.tcp_max_tw_buckets = 36000

net.ipv4.tcp_tw_reuse = 1

net.ipv4.tcp_max_orphans = 327680

net.ipv4.tcp_orphan_retries = 3

net.ipv4.tcp_syncookies = 1

net.ipv4.tcp_max_syn_backlog = 16384

net.ipv4.ip_conntrack_max = 65536

net.ipv4.tcp_max_syn_backlog = 16384

net.ipv4.tcp_timestamps = 0

net.core.somaxconn = 16384

EOF

# sysctl --system

 

 所有节点配置完内核后，重启服务器，保证重启后内核依旧加载

# reboot

# lsmod | grep --color=auto -e ip_vs -e nf_conntrack
