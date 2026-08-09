K8s-deploy-2

星期一, 2025年12月29日

下午 4:27

K8s组件和Runtime安装

如果安装的版本低于1.24，选择Docker和Containerd均可，高于1.24选择Containerd作为Runtime。

注意：Runtime安装选择两个小节的其中一个小节即可。

 

 Containerd作为Runtime

所有节点安装docker-ce-20.10：

# yum install docker-ce-20.10.* docker-ce-cli-20.10.* -y

可以无需启动Docker，只需要配置和启动Containerd即可。

首先配置Containerd所需的模块（所有节点）：

# cat \<\<EOF | sudo tee /etc/modules-load.d/containerd.conf

overlay

br_netfilter

EOF

所有节点加载模块：

# modprobe -- overlay

# modprobe -- br_netfilter

所有节点，配置Containerd所需的内核：

# cat \<\<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf

net.bridge.bridge-nf-call-iptables = 1

net.ipv4.ip_forward = 1

net.bridge.bridge-nf-call-ip6tables = 1

EOF

 

所有节点加载内核：

# sysctl --system

所有节点配置Containerd的配置文件：

# mkdir -p /etc/containerd

# containerd config default | tee /etc/containerd/config.toml

 

所有节点将Containerd的Cgroup改为Systemd：

# vim /etc/containerd/config.toml

找到 containerd.runtimes.runc.options，添加SystemdCgroup = true（如果已存在直接修改，否则会报错 | 注意有重复的），如下图所示：

![[My-Case_CentOS7.9^MK8s_002_K8s-deploy-2_001.png]]

所有节点将sandbox_image的Pause镜像改成符合自己版本的地址registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.6：（这个路径比较疑惑）

![[My-Case_CentOS7.9^MK8s_002_K8s-deploy-2_002.png]]

所有节点启动Containerd，并配置开机自启动：

# systemctl daemon-reload

# systemctl enable --now containerd

所有节点配置crictl客户端连接的运行时位置：

# cat \> /etc/crictl.yaml \<\<EOF

runtime-endpoint: unix:///run/containerd/containerd.sock

image-endpoint: unix:///run/containerd/containerd.sock

timeout: 10

debug: false

EOF

--- 做到这里完成没问题。没有做使用 Docker 为 Runtime。

 

Docker作为Runtime（版本小于1.24）

如果选择Docker作为Runtime，安装步骤较Containerd较为简单，只需要安装并启动即可。

所有节点安装docker-ce 20.10：

# yum install docker-ce-20.10.* docker-ce-cli-20.10.* -y

由于新版Kubelet建议使用systemd，所以把Docker的CgroupDriver也改成systemd：

# mkdir /etc/docker

# cat \> /etc/docker/daemon.json \<\<EOF

EOF

所有节点设置开机自启动Docker：

# systemctl daemon-reload && systemctl enable --now docker

 

安装Kubernetes组件

首先在Master01节点查看最新的Kubernetes版本是多少：

# yum list kubeadm.x86_64 --showduplicates | sort -r

所有节点安装1.23最新版本kubeadm、kubelet和kubectl：

# yum install kubeadm-1.23* kubelet-1.23* kubectl-1.23* -y

我这里安装的是 1.23.17-0 

如果选择的是Containerd作为的Runtime，需要更改Kubelet的配置使用Containerd作为Runtime：

# cat \>/etc/sysconfig/kubelet\<\<EOF

KUBELET_KUBEADM_ARGS="--container-runtime=remote --runtime-request-timeout=15m --container-runtime-endpoint=unix:///run/containerd/containerd.sock"

EOF

注意

如果不是采用Containerd作为的Runtime，请不要执行上述命令。

所有节点设置Kubelet开机自启动（由于还未初始化，没有kubelet的配置文件，此时kubelet无法启动，无需管理）：

# systemctl daemon-reload

# systemctl enable --now kubelet

 此时kubelet是起不来的，日志会有报错不影响！

--- 做到这里完成没问题。没出现报错。

高可用组件安装

（注意：如果不是高可用集群，haproxy和keepalived无需安装）

公有云要用公有云自带的负载均衡，比如阿里云的SLB，腾讯云的ELB，用来替代haproxy和keepalived，因为公有云大部分都是不支持keepalived的，另外如果用阿里云的话，kubectl控制端不能放在master节点，推荐使用腾讯云，因为阿里云的slb有回环的问题，也就是slb代理的服务器不能反向访问SLB，但是腾讯云修复了这个问题。

所有Master节点通过yum安装HAProxy和KeepAlived：

# yum install keepalived haproxy -y

所有Master节点配置HAProxy（详细配置参考HAProxy文档，所有Master节点的HAProxy配置相同）：

[root@k8s-master01 etc]# mkdir /etc/haproxy

[root@k8s-master01 etc]# vim /etc/haproxy/haproxy.cfg 

global

  maxconn  2000

  ulimit-n  16384

  log  127.0.0.1 local0 err

  stats timeout 30s

 

defaults

  log global

  mode  http

  option  httplog

  timeout connect 5000

  timeout client  50000

  timeout server  50000

  timeout http-request 15s

  timeout http-keep-alive 15s

 

frontend monitor-in

  bind *:33305

  mode http

  option httplog

  monitor-uri /monitor

 

frontend k8s-master

  bind 0.0.0.0:16443

  bind 127.0.0.1:16443

  mode tcp

  option tcplog

  tcp-request inspect-delay 5s

  default_backend k8s-master

 

backend k8s-master

  mode tcp

  option tcplog

  option tcp-check

  balance roundrobin

  default-server inter 10s downinter 5s rise 2 fall 2 slowstart 60s maxconn 250 maxqueue 256 weight 100

  server k8s-master01 10.10.40.201:6443  check

  server k8s-master02 10.10.40.202:6443  check

  server k8s-master03 10.10.40.203:6443  check

所有Master节点配置KeepAlived，配置不一样，注意区分。

注意每个节点的IP和网卡（interface参数）加粗部分。备份原有的文件，贴下面的修改。

Master01节点的配置：

[root@k8s-master01 ~]# mkdir /etc/keepalived

[root@k8s-master01 ~]# vim /etc/keepalived/keepalived.conf 

! Configuration File for keepalived

global_defs 

vrrp_script chk_apiserver 

vrrp_instance VI_1 {

    state MASTER

    interface ens33

    mcast_src_ip 10.10.40.201

    virtual_router_id 51

    priority 101

    advert_int 2

    authentication 

    virtual_ipaddress 

    track_script 

}

Master02节点的配置：

[root@k8s-master02 ~]# vim /etc/keepalived/keepalived.conf 

! Configuration File for keepalived

global_defs 

vrrp_script chk_apiserver 

vrrp_instance VI_1 {

    state BACKUP

    interface ens33

   mcast_src_ip 10.10.40.202

    virtual_router_id 51

    priority 100

    advert_int 2

    authentication 

    virtual_ipaddress 

    track_script 

}

Master03节点的配置：

[root@k8s-master03 ~]# vim /etc/keepalived/keepalived.conf 

! Configuration File for keepalived

global_defs 

vrrp_script chk_apiserver 

vrrp_instance VI_1 {

    state BACKUP

    interface ens33

    mcast_src_ip 10.10.40.203

   virtual_router_id 51

    priority 100

    advert_int 2

    authentication 

    virtual_ipaddress 

    track_script 

}

所有master节点配置KeepAlived健康检查文件：

[root@k8s-master01 keepalived]# cat /etc/keepalived/check_apiserver.sh 

#!/bin/bash

err=0

for k in $(seq 1 3)

do

    check_code=$(pgrep haproxy)

    if [[ $check_code == "" ]]; then

        err=$(expr $err + 1)

        sleep 1

        continue

    else

        err=0

        break

    fi

done

 

if [[ $err != "0" ]]; then

    echo "systemctl stop keepalived"

    /usr/bin/systemctl stop keepalived

    exit 1

else

    exit 0

fi

# chmod +x /etc/keepalived/check_apiserver.sh

启动haproxy和keepalived (所有 master 节点)

[root@k8s-master01 keepalived]# systemctl daemon-reload

[root@k8s-master01 keepalived]# systemctl enable --now haproxy

[root@k8s-master01 keepalived]# systemctl enable --now keepalived

如果成功你可以看到VIP IP。 

![[My-Case_CentOS7.9^MK8s_002_K8s-deploy-2_003.png]]

重要：如果安装了keepalived和haproxy，需要测试keepalived是否是正常的

测试VIP

[root@k8s-master01 ~]# ping 10.10.40.236 -c 4

PING 10.10.40.236 (10.10.40.236) 56(84) bytes of data.

64 bytes from 10.10.40.236: icmp_seq=1 ttl=64 time=0.464 ms

64 bytes from 10.10.40.236: icmp_seq=2 ttl=64 time=0.063 ms

64 bytes from 10.10.40.236: icmp_seq=3 ttl=64 time=0.062 ms

64 bytes from 10.10.40.236: icmp_seq=4 ttl=64 time=0.063 ms

--- 10.10.40.236 ping statistics ---

4 packets transmitted, 4 received, 0% packet loss, time 3106ms

rtt min/avg/max/mdev = 0.062/0.163/0.464/0.173 ms

[root@k8s-master01 ~]# telnet 10.10.40.236 16443

Trying 10.10.40.236...

Connected to 10.10.40.236.

Escape character is '^]'.

Connection closed by foreign host.

如果ping不通且telnet没有出现[ ] ]，则认为VIP不可以，不可在继续往下执行，需要排查keepalived的问题，比如防火墙和selinux，haproxy和keepalived的状态，监听端口等

所有节点查看防火墙状态必须为disable和inactive：systemctl status firewalld

所有节点查看selinux状态，必须为disable：getenforce

master节点查看haproxy和keepalived状态：systemctl status keepalived haproxy

master节点查看监听端口：netstat -lntp

--- 做到这里完成没问题。VIP能出来，可以ping通。做了快照 *-03
