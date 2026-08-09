【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1集群，开启Dashboard-2.7.0、部署ingress-nginx-1.10.1

[ ][blog.csdn.net /m0_53928179/article/details/139068769 ](https://blog.csdn.net/m0_53928179/article/details/139068769)

 

Ubuntu-22.04搭建 k8s-1.30.1集群，开启Dashboard-v2.7.0（以及Token不生成的问题）、部署ingress-nginx-1.10.1

 

引言

最近在研究分布式计算，想将分布式计算都容器化，使用 k8s 来调度，所以从0开始学 k8s ，这是我遇到坑后百度、查资料一点一点总结的搭建过程，贴在这方便以后自己找，也希望对你能有所帮助，后续我会不定时更新这篇博客的内容

 

*** 特别注意：当 k8s 集群 init 了以后的 kubectl 命令最好都等上一步的命令执行完了再进行下一步操作，不要抢着执行，随时看 Pod 的状态 kubectl get pods -A ，只要还有一个没有 Running 就不要着急下一步

2024-09-06实测v1.31.0也可用本文的方式搭建

 

一、系统环境准备

*** 以下在所有节点上都要做

用 Ubuntu-22.04 版本，CentOS 操作核心是一样的，只是命令不同罢了。

 

1、关闭 swap

官方要求关闭swap，虚拟内存相关，因为 Kubernetes 无法读取虚拟内存相关的数据，开启这个可能会导致 Kubernetes 的内存问题

要想永久关，得先解除程序占用，临时关闭 swap：

swapoff -a

修改配置文件永久关闭，注释这个文件：

vi /etc/fstab

中的关于 swap 的那一行，如图，注释掉它：

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_001.png]]

 

2、关闭 SELinux

Ubuntu 没有这个东西可以不管，CentOS 执行：

sudo sed -i 's/^SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config

 

然后重启系统即可。SELinux 是系统安全方面的，如果你懂怎么弄可以自己配置 SELinux 就不用关，很麻烦，所以大家基本都关了的

 

3、关闭防火墙

需要关闭防火墙，因为节点之间要互相通信，开防火墙容易出问题

ufw disable\
# 或者
systemctl disable --now ufw

 

4、设置时区（根据自己的时间情况可选）

因为关系到集群机器之间的通信，需要一个时间同步大家的行为。设置为上海时区

timedatectl set-timezone Asia/Shanghai

 

重启时间同步服务

systemctl restart systemd-timesyncd.service

 

看一下时间对不对：

timedatectl status\
# 或者
date

 

5、修改 /etc/hosts（可选） 、/etc/hostname（可选但建议）

这个是为了后面填地址的时候方便，不用填IP地址，直接填名字就行

修改 hosts 文件，将自己的IP地址和主机名填进去：

echo "192.168.10.10 k8s-master" | sudo tee -a /etc/hosts

 

其他节点的你想填也填进去

修改 hostsname 文件，两种办法，一种是 hostnamectl 命令，一种是直接修改文件，修改文件的方式：

echo "k8s-master" | sudo tee /etc/hostname # 不同的节点改不同的名字

修改完主机名文件后最好重启一次ssh终端或系统

 

6、开启流量转发

开这个的原因是让各个主机承担起网络路由的角色，因为后续还要安装网络插件，要有一个路由器各个 Pod 才能互相通信。执行：

cat \<\<EOF | sudo tee /etc/sysctl.d/k8s.conf\
net.ipv4.ip_forward = 1
EOF

 

应用参数：

sudo sysctl --system

 

查看是否开启成功：

sysctl net.ipv4.ip_forward

 

没错的话会看到如下已经是1，说明开启成功

net.ipv4.ip_forward = 1

 

 

7、其他相关知识（不重要）

Ⅰ、IP地址固定以及网络相关知识

你有可能想固定机器的IP地址，编辑/etc/netplan下的xxx.yaml文件，如果只有一张网卡，就只有一个文件，如果有多张网卡根据网卡名来，wifi网卡的文件名也带有wifi字样。以下是一个有线网卡的固定IP例子，根据你的需求改就行：

network:\
[  ethernets:\
    ens33: # ]要固定的网卡名
[      dhcp4: false # false]是关闭自动获取地址，true是开启
[      addresses:\
        - 192.168.10.10/24 # ]你要固定的IP地址
[      gateway4: 192.168.10.1 # ]网关地址
[      nameservers:\
        addresses: # DNS]地址，可以有多个
[          - 223.5.5.5 # ]阿里DNS\
[          - 8.8.8.8 # ]谷歌DNS\
[  version: 2]

-------------------------------------------实际配置

network:

  version: 2

  ethernets:

    ens33:

      dhcp4: no

      addresses:

        - 10.10.40.126/16

      routes:

   [      - to: default ]

[           via: 10.120.146.203][     \<-- gateway ]设置

      nameservers:

[             addresses: \10.7.7.7,10.8.8.8[]]

-------------------------------------------

 

改完文件后应用更改：

netplan apply

 

验证是否成功，执行：

ip a # 查看所有网卡以及对应的IP地址

 

二、安装 containerd 运行时环境

*** 以下在所有节点上都要做

 

Kubernetes-1.24 版本移除了 dockershim 的支持，所以之前的先安装 docker 再安装 Kubernets 的方式已经不可行了，可能会导致 Kubelet 报错退出，所以我们要先安装 containerd 运行时环境。这时可能就有人问了，安装 Docker 时不是带有 containerd 了吗，你说得对，安装 Docker 时确实是 apt install -y docker-ce docker-ce-cli containerd.io ，确实安装了 containerd ，但是这是 Docker 的 containerd 啊，这么安装的 containerd 是由 Docker 管理的，k8s 无法管理，所以就会导致报错，踩过的坑啊。

 

运行时环境除了containerd以外还有CRI-O、Mirantis Container Runtime、Docker Engine，但我们用containerd就行，其他的运行时安装请参阅[官方手册](https://kubernetes.io/zh-cn/docs/setup/production-environment/container-runtimes/)

 

安装containerd有两种方式：

1、下载最新二进制文件安装

这种方式虽然麻烦，但是可以安装最新版的containerd，具体操作：

 

Ⅰ、安装runc客户端

因为我们是手动安装containerd，所以也得手动安装runc，这里就不上Github上找包了，apt工具的runc已经很新了。执行：

sudo apt install -y runc

 

Ⅱ、下载安装containerd

去 GitHub 上下载二进制包，想要其他版本的话替换两处 1.7.17 为你想要的版本即可：

 

curl -O <https://github.com/containerd/containerd/releases/download/v1.7.17/containerd-1.7.17-linux-amd64.tar.gz>

 

解压文件到当前目录：

tar -zxvf containerd-1.7.17-linux-amd64.tar.gz

 

会得到一个文件夹bin，移动bin中的所有文件到目录/bin中去：

mv bin/* /bin

 

由于我们是手动解压二进制文件安装的，所以得先生成一个Service，创建这个Service文件并且添加内容：

cat \<\<EOF | sudo tee /etc/systemd/system/containerd.service\
[Unit]
Description=containerd container runtime\
Documentation=https://containerd.io\
After=network.target

[Service]
ExecStart=/bin/containerd\
Type=notify\
Delegate=yes\
KillMode=process\
Restart=always\
RestartSec=5

[Install]
WantedBy=multi-user.target\
EOF

 

然后重新加载systemd并启动containerd：

sudo systemctl daemon-reload && \\
sudo systemctl start containerd && \\
sudo systemctl enable containerd

 

执行看版本正不正常：

containerd -v

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_002.png]]

 

2、通过包管理器安装

这种方式最简单，但是一般来说版本会滞后三四个版本左右，例如我们手动安装的版本是v1.7.17，通过apt安装的版本一般为v1.7.12

 

Ⅰ、安装

先更新源再安装，这里不用安装runc，会自动安装的：

apt update && apt install -y containerd

 

Ⅱ、查看版本

执行命令版本没问题就可以了（一般落后最新版三四个版本左右）：

containerd -v

 

3、生成配置文件（重要）

还要手动生成配置文件，不管是二进制文件安装的还是包管理器安装的，都要执行：

sudo mkdir -p /etc/containerd && \\
sudo containerd config default \> /etc/containerd/config.toml

 

修改 /etc/containerd/config.toml 文件中：

[[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]] 下[ [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options] ]下的 SystemdCgroup 为 true：

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_003.png]]

 

修改这个配置是因为kubelet和底层容器运行时（我们用的是containerd）都需要对接控制组来强制为Pod和容器管理资源，并且运行时和k8s需要用同一个初始化系统，Ubuntu默认使用的初始化系统是systemd，k8s v1.22起，如果没有在KubeletConfiguration下设置cgroupDriver字段，kubeadm默认使用systemd，所以我们只需要设置containerd就行了。

k8s官方[解释地址](https://kubernetes.io/zh-cn/docs/setup/production-environment/container-runtimes/#cgroup-drivers)

 

改完了别忘记重启一下 containerd ：

systemctl restart containerd

 

三、安装 kubeadm、kubelet、kubectl

*** 以下在所有节点上都要做

 

1、简单介绍

kubeadm 是自动引导整个集群的工具，本质上 k8s 就是一些容器服务相互配合完成管理集群的任务，如果你知道具体安装哪些容器那么可以不用这个。

kubalet 是各个节点的总管，它上面都管，管理Pod、资源、日志、节点健康状态等等，它不是一个容器，是一个本地软件，所以必须得安装

kubectl 是命令行工具，给我们敲命令与 k8s 交互用的，必须得安装

 

2、安装 （All node）

首先得保证源都是新的：

sudo apt update && \\
sudo apt upgrade -y

 

然后安装一些必要工具：

sudo apt install -y apt-transport-https ca-certificates curl gpg

 

如果 /etc/apt/keyrings 目录不存在，先创建

sudo mkdir -p -m 755 /etc/apt/keyrings

 

下载 k8s 包仓库的公共签名密钥。解释一下，密钥中有一个v1.30，所有版本都是用的这个格式的密钥，即使你改为其他版本，下载的都是一个密钥，不影响，不过你想改也行：

 

curl -fsSL <https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key> | \\
sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg && \\
sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg

 

添加 k8s 的apt仓库，使用其他版本的替换地址中的 v1.30 就行：

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] <https://pkgs.k8s.io/core:/stable:/v1.30/deb/> /' \\
| sudo tee /etc/apt/sources.list.d/kubernetes.list

 

更新 apt 索引，并且安装，还要防止软件更新，三步：

sudo apt update && \\
sudo apt install -y kubelet kubectl kubeadm && \\
sudo apt-mark hold kubelet kubeadm kubectl

 

启动 kubelet ，并且设置开机自启：

sudo systemctl enable --now kubelet

 

看看有没有安装成功：

kubeadm version

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_004.png]]

 

 

四、初始化主节点（只在主节点上做）

1、提前拉取镜像（可选）

如果你觉得慢或者出了什么未知的问题，可以提前将所需的镜像拉取下来，因为之前说过了，k8s 实质上是一堆容器服务组合，调度管理其他容器的，那么运行容器就得需要镜像。你可以在 init 前运行这个命令：

sudo kubeadm config images pull \\
--kubernetes-version=v1.30.1 \[   ]#这个版本需要注意，是对应 kubeadm version 显示的版本

--cri-socket=unix:///run/containerd/containerd.sock\
# --image-repository=registry.aliyuncs.com/google_containers \ # 觉得慢加上这个

这个命令会将所需的镜像提前拉取下来，然后再 init 就会快很多

 

2、初始化节点

有了 kubeadm，就能一键初始化集群主节点了，运行：

sudo kubeadm init \\
--apiserver-advertise-address=192.168.10.10 \ \
--control-plane-endpoint=k8s-master \\
--kubernetes-version=v1.30.1 \\
--service-cidr=10.50.0.0/16 \\
--pod-network-cidr=10.60.0.0/16 \\
--cri-socket=unix:///run/containerd/containerd.sock\
# --image-repository=registry.aliyuncs.com/google_containers \ # 嫌慢的可以加上这句，用阿里云的镜像，我科学上网没试能不能用

---------实际使用下面没成功

sudo kubeadm init --apiserver-advertise-address=10.10.40.126 --control-plane-endpoint=k8s-master --kubernetes-version=v1.30.1 --service-cidr=10.50.0.0/16 --pod-network-cidr=10.60.0.0/16 --cri-socket=unix:///run/containerd/containerd.sock

-----------------

apiserver-advertise-address 填主节点的IP地址 

 

control-plane-endpoint ，还记得我们在 /etc/hosts 文件中配置的映射关系吗，填主节点的地址或者主机名 kubernetes-version 版本不多说 

 

service-cidr 这是 Service 负载均衡的网络，就是你运行了一堆容器后有一个将它们统一对外暴露的地址，并且将对它们的请求统一收集并负载均衡的网络节点，得为它配置一个网段、

 

[ ]pod-network-cidr 每个 Pod 所在的网段 cri-socket 指定容器化环境

 

如果 init 失败，而且失败的原因是没有连接上 api-server 的话，使用命令查看 kubelet 的日志：

journalctl -u kubelet -xe

 

如果其中有类似这样的错误，无法拉取的镜像叫 pause:3.8 的话（新版本v1.31.0实测没有这个问题）：

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_005.png]]

 

* 如果不是 pause:3.8 ，那就是镜像拉取失败，可能是没有指定国内的源去国外下载失败了，需要指定国内的源并提前拉取镜像；

* 还有可能是我们指定的源（比如阿里源）没有这个镜像，因为 k8s-v1.30.1 这个版本会默认使用 3.8 版本的沙箱，不知道什么原因拉取不下来这个镜像，所以只能拉取 3.9 下来改为 3.8：

ctr --namespace k8s.io image pull registry.aliyuncs.com/google_containers/pause:3.9
ctr --namespace k8s.io image tag registry.aliyuncs.com/google_containers/pause:3.9 registry.k8s.io/pause:3.8

 

init 失败后需要重置再重新 init，执行：

sudo kubeadm reset # 重置 kubeadm ，执行这个后需要敲 y 回车
sudo rm -rf /etc/cni/net.d # 删除上次 init 生成的文件
sudo rm -rf /var/lib/etcd # 删除上次 init 生成的文件

 

其他问题请参阅：[故障排查](https://kubernetes.io/zh-cn/docs/setup/production-environment/tools/kubeadm/troubleshooting-kubeadm/)

 

再次 init，当然，你也可以选择配置文件的方式，和命令行的方式二选一：

apiVersion: kubeadm.k8s.io/v1beta3
kind: ClusterConfiguration\
kubernetesVersion: v1.30.1

[ # master host name]

controlPlaneEndpoint: "k8s-master:6443"[    ]

networking:\
  podSubnet: "10.100.2.0/24"\
  serviceSubnet: "10.100.1.0/24"\
apiServer:\
  extraArgs:

[ # master IP address]

[    advertise-address: "192.168.2.203"][     ]
etcd:\
[  local:\
    imageRepository: registry.aliyuncs.com/google_containers\
imageRepository: registry.aliyuncs.com/google_containers\
---\
apiVersion: kubeadm.k8s.io/v1beta3
kind: InitConfiguration\
nodeRegistration:\
  criSocket: /run/containerd/containerd.sock]

 

这个只需要 init 时指定配置文件就行：

kubeadm init --config conf.yaml

# 测试时使用配置文件初始化方式成功了

 

init 后成功的话会看到类似：

You can now join any number of control-plane nodes by copying certificate authorities\
and service account keys on each node and then running the following as root:

kubeadm join k8s-master:6443 --token is5atc.cc70psy934ptmb4j \\
        --discovery-token-ca-cert-hash sha256:cc01bdb1c2c0677ce9043af9f4996352320ae29b81c567a88c42f510f1817715 \\
        --control-plane

 

Then you can join any number of worker nodes by running the following on each as root:

 

kubeadm join k8s-master:6443 --token is5atc.cc70psy934ptmb4j \\
[        --discovery-token-ca-cert-hash] sha256:cc01bdb1c2c0677ce9043af9f4996352320ae29b81c567a88c42f510f1817715 

 

的东西，在这些命令之前还有几个命令，都执行一下，这是固定的，都这么执行：

mkdir -p $HOME/.kube\
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config\
sudo chown $(id -u):$(id -g) $HOME/.kube/config

 

执行完这三条后我们就可以查看主节点上的Pod是否正常了：

kubectl get pods -A # 记住，kubectl命令只能在主节点上执行，其他节点上执行会被拒绝

 

会看到类似下图的，就是初始化主节点成功：

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_006.png]]

 

还有，我们执行完 init 后控制台打印的命令：

kubeadm join k8s-master:6443 --token is5atc.cc70psy934ptmb4j \\
        --discovery-token-ca-cert-hash sha256:cc01bdb1c2c0677ce9043af9f4996352320ae29b81c567a88c42f510f1817715 \\
        --control-plane

 

一个有 --control-plane ，一个没有，没有的那个是子节点运行的，一个子节点只要按照上面的步骤走到安装 kubelet、kubectl、kubeadm 后执行不带 --control-plane 的这部分命令就可以加入集群作为一个子节点，同样的，带 --control-plane 的是加入集群作为主节点，当真正的主节点挂后，这个加入的主节点就有可能成为主节点

 

3、注意的问题

*** 特别注意，apiserver-advertise-address、service-cidr、pod-network-cidr 三者的IP网段 不能重叠 不能重叠 不能重叠，不但三者之间不能重叠，三者每个也不能与互联网上的地址重叠，不然会出问题，后两个一般用 10.x.x.x 网段，这个网段是留给内网的

关于IP地址的设定，请参阅本文 第一大节 \> 第7小节 \> 第Ⅰ节 IP地址固定以及网络相关知识

 

五、安装网络插件 Calico （只在主节点[[master]]上做）

我们走到这步后还没有完成，因为集群只是在主节点上初始化了，其他机器要想加入集群，还得使用网络插件将它们连接起来，所以得安装一个网络插件，有很多个，选 Calico 就行。按照官网给的教程安装：

kubectl create -f <https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml>

 

如果报错连接不上的话将文件手动下载下来再执行，没办法国外的网站：

wget [https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml](https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml)
# 或者
curl -O [https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml](https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml)

 

下载下来后一定不要用 kubectl apply -f 来执行，会报错：

The CustomResourceDefinition "installations.operator.tigera.io" is invalid: metadata.annotations:\
Too long: must have at most 262144 bytes

 

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_007.png]]

 

意思是 annotation 长度过长了，原因是 apply 和 create 的处理不同，这点 GitHub 上也有人在吐槽，这是 [GitHub上的吐槽地址](https://github.com/projectcalico/calico/issues/7826) 改配置文件中这个选项的长度就不改了，我们不用 apply 使用 create：

kubectl create -f tigera-operator.yaml

 

没报错就没问题

 

第二步将配置文件下载下来，因为要改内容：

curl -O <https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/custom-resources.yaml>

 

修改这个文件中的 192.168.0.0 为你刚才 init 时指定的 --pod-network-cidr：

vi custom-resources.yaml

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_008.png]]

 

原本是 192.168.0.0 改为你指定的IP地址

 

改好后执行命令，这个文件可以用 apply 因为没有超限制（乐）：

kubectl apply -f custom-resources.yaml

 

就会开始初始化网络插件，耐心等待，直到：

kubectl get pods -A

 

显示的所有容器都 Running 就完成了：

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_009.png]]

 

 

六、其他节点加入集群（可选）

1、其他节点加入集群

这是可选的，如果只有一个主节点，那么就要去除主节点上的污点，否则主节点上无法启动我们自己的Pod。

去除污点参阅本节的第二小节

 

首先得保证这个节点能与主节点网络连通，然后执行本文目录中 一 、 二 和 三 的步骤，三个步骤一点都不要漏。

 

*** 需要注意的是，我们上面遇到的那个沙箱的问题，pause-3.9、pause-3.8在这个新节点上也要手动拉（v1.31.0版本实测没有这个问题），在 join 之前。

 

还记得我们之前 init 初始化主节点成功时得到的类似：

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_010.png]]

 

的字符串吗？复制下面那串：

kubeadm join k8s-master:6443 --token 9qk132.qldpmmgf4hpv37lx \\
        --discovery-token-ca-cert-hash \\
        sha256:08f59184a092f52d114b114f89a4e05079f63985bf7e62a68a3c254aea8b7469

 

到要加入集群的这个节点中去执行，但是前提是 一、二、三 中的步骤你都完全执行完了并且没有报错。

 

这个命令中有一个 --token ，它是会过期的，过期时间好像是 24小时 ，如果 token 过期了，执行：

kubeadm token create

 

生成新的 token，替换命令中的 --token 。

 

此外，kubeadm join命令后面的--discovery-token-ca-cert-hash如果你也没记下来的话，可以从主节点的CA证书中提取哈希值，执行：

openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2\>/dev/null \\
| sha256sum | awk ''

 

同样的，提取到的哈希值替换命令中--discovery-token-ca-cert-hash的值就行。

 

工作节点加入集群成功类似这样：

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_011.png]]

执行完后这里的成功其实还不算完全成功，要等它 init 完成才算，你只需要在主节点上盯着所有的 Pod 都 Running 了后就可以继续下一步了。

 

2、主节点成为工作节点 worker node（可选）

如果你只想在本机上运行所有的 Pod，那么只需要将主节点配置为工作节点，以便它可以调度并运行工作负载。在主节点上启用调度器，一般是移除主节点上的 污点（taint），这些污点会阻止调度器将工作负载调度到主节点。执行命令查看都有哪些污点：

kubectl describe node \<主节点的名字，就是我们设置的主机名hostname\> | grep Taints

 

# 我的是：
kubectl describe node k8s-master | grep Taints\
Taints:[             node-role.kubernetes.io/control-plane:NoSchedule]

 

然后再删除这个污点：

kubectl taint nodes k8s-master node-role.kubernetes.io/control-plane-

 

现在主节点上也会被部署 Pod 了。

 

3、如果节点初始化失败

如果这个节点初始化失败，需要重置集群中关于这个节点的东西：

kubectl drain \<节点名称\> --ignore-daemonsets --delete-emptydir-data # 驱逐节点上的所Pod\
kubectl delete node \<节点名称\> # 从集群中删除节点

 

然后到节点上执行：

sudo kubeadm reset # 执行后按 y

 

sudo rm -rf /etc/cni/net.d # 移除 CNI 配置

 

# 清除 iptables 规则
sudo iptables -F\
sudo iptables -t nat -F\
sudo iptables -t mangle -F\
sudo iptables -t raw -F\
sudo iptables -X

 

sudo rm -rf ~/.kube # 删除 kubeconfig 文件（根据你的具体配置路径可能有所不同）

 

sudo reboot # 可选，可以重启一下节点

 

重置后再根据情况重新初始化

 

 

============= 以下对整个集群的操作均在主节点上

 

七、安装 Kubernetes Dashboard 前端控制面板（可选）

一直使用命令行手都敲累了，这时候你可以选择安装一个前端可视化页面来控制整个集群。

 

首先你得安装官方提供的前端页面，下载这个配置文件，你可以将链接中的 v2.7.0 替换成你想要的版本：

curl -O <https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml>

 

需要修改差不多二三十行处的

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_012.png]]

 

再执行：

kubectl apply -f recommended.yaml

 

 

然后就是等待它结束后创建一个配置，用于配置账户和获取token以登录页面，一定要等上面的结束再接着（结束标志是所有的Pod 都 Running）：

vi admin.yaml

 

添加以下内容：

apiVersion: v1
kind: ServiceAccount\
metadata:\
  name: admin-user\
  namespace: kubernetes-dashboard\
---\
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding\
metadata:\
  name: admin-user\
roleRef:\
  apiGroup: rbac.authorization.k8s.io\
  kind: ClusterRole\
  name: cluster-admin\
subjects:\
  - kind: ServiceAccount\
    name: admin-user\
    namespace: kubernetes-dashboard                                                

 

然后再执行：

kubectl apply -f k8s-admin-user.yaml # 这句是创建账户

 

做到这步后网上的大多数帖子都是让执行：

kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '')

 

他们都能生成Token，但是我的不行我的执行了这句代码后出来的是这样的：

root@k8s-master:/app/k8s/dashboard# kubectl -n kubernetes-dashboard describe secret \\
$(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '')
Name:         kubernetes-dashboard-certs\
Namespace:    kubernetes-dashboard\
Labels:       k8s-app=kubernetes-dashboard\
Annotations:  \<none\>

Type:  Opaque

Data\
====

Name:         kubernetes-dashboard-csrf\
Namespace:    kubernetes-dashboard\
Labels:       k8s-app=kubernetes-dashboard\
Annotations:  \<none\>

Type:  Opaque

Data\
====\
csrf:  256 bytes

Name:         kubernetes-dashboard-key-holder\
Namespace:    kubernetes-dashboard\
Labels:       \<none\>\
Annotations:  \<none\>

Type:  Opaque

Data\
====\
priv:  1675 bytes\
pub:   459 bytes

 

并没有 Token，可能是由于 API 服务器还没有为我们创建的账户创建默认的 Token，这时候需要自己手动生成密钥，新建一个yaml 文件，加入：

apiVersion: v1
kind: Secret\
metadata:\
[  name: admin-user-token\
  namespace: kubernetes-dashboard\
  annotations:\
    kubernetes.io/service-account.name: admin-user] # 如果上面的用户名你自定义了，记得替换这里
type: kubernetes.io/service-account-token

 

创建密钥：

kubectl apply -f admin-user-secret.yaml

 

检查一下生成没有：

kubectl -n kubernetes-dashboard get secret | grep admin-user

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_013.png]]

 

发现有，这时候我们再查看 Token：

kubectl -n kubernetes-dashboard describe secret token的名字，这里我们的是admin-user-token

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_014.png]]

 

生成了，是不是有点像 RSA 密钥？复制这个 Token 保存一下，以后要用就直接找文件了。

 

查看前端页面暴露出去的端口是多少：

kubectl get svc -n kubernetes-dashboard

 

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_015.png]]

 

很明显，我的这个端口是 32713，网页访问任意一台节点的 [https://IP:端口，就能打开页面，记住，必须是](https://IP:端口，就能打开页面，记住，必须是) https协议哦。

打开页面有警告不管，点击高级继续访问，你会看到：

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_016.png]]

 

还记得之前复制的那个像 RSA密钥的token吗？粘贴进去就能登录了。

 

如果上面的步骤还是存在问题，可以删除并重新创建 ServiceAccount 和 ClusterRoleBinding：

 

kubectl delete serviceaccount admin-user -n kubernetes-dashboard\
kubectl delete clusterrolebinding admin-user

kubectl apply -f - \<\<EOF\
apiVersion: v1
kind: ServiceAccount\
metadata:\
  name: admin-user\
  namespace: kubernetes-dashboard\
---\
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding\
metadata:\
  name: admin-user\
roleRef:\
  apiGroup: rbac.authorization.k8s.io\
  kind: ClusterRole\
  name: cluster-admin\
subjects:\
  - kind: ServiceAccount\
    name: admin-user\
    namespace: kubernetes-dashboard\
EOF

 

等待几分钟，然后再次检查生成的 Secret。

 

八、部署 ingress 接收外部流量转发到 service（可选）

1、简介

大白话就是：在此之前我们都是直接访问 service ，让 service 负载均衡到 Pod 上，优点是直接，缺点是随着 service 的增多端口会越来越多，不好记。

于是我们在 service 之上再套一层，统一管理众多的 service

 

流量流向是：流量 --\> ingress --\> service --\> pod

 

2、部署 ingress-nginx

首先得明确 ingress 其实也是一个 service，它接收外部的流量转发到配置好的指定了的 service，所以当我们部署 ingress-nginx 时会发现生成了一个关于 ingress 的 service；

 

k8s 的 ingress 实现有很多个，就不一一列举了，大家都用 ingress-nginx；

 

执行语句，从 GitHub 上拉取 yaml 开始配置（需要其他版本的改 v1.10.1，但是得注意兼容情况）：

curl -O <https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.1/deploy/static/provider/cloud/deploy.yaml>

 

修改kind: ConfigMap 下面 kind: Service处的两个地方：

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_017.png]]

 

***上面的从 Local 改为 Cluster，如果是 Local ，那么只有到达节点的流量才会被处理，而非自己节点的流量没有反应，意思就是说如果你的主节点是 192.168.10.10 ，恰好 ingress-nginx 又没有部署在主节点上，那么你想通过 192.168.10.10访问 ingress 就不行，改为 Cluster 的话，你在集群中任何一台机器上用 IP:port 访问 ingress 都可以，Cluster 的好处是不管从哪都能访问，缺点是会增加集群内部的流量，因为你的请求流量会在集群之间转发，而且你的 源IP地址 也会变化，因为当你访问到一个没有部署 ingress 的节点上时，它会被路由到有 ingress 的节点上，所以内部流量才会增加，IP才会变

 

***下面的 LoadBalancer 改为 NodePort ，LoadBalancer 是给云服务器或者自己弄了负载均衡的用的

 

应用配置文件：

kubectl apply -f deploy.yaml

 

查看 ingress 暴露的端口：

kubectl get svc -A

 

找一个类似这样的：

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_018.png]]

 

我这里就是 32536 ，配置一个 ingress 资源，编辑一个 yaml 加入：

apiVersion: networking.k8s.io/v1
kind: Ingress\
metadata:\
  name: my-ingress\
  namespace: default\
spec:\
  rules:\
  - host: mynginx.com\
    http:\
      paths:\
      - path: /\
        pathType: Prefix\
        backend:\
          service:\
            name: my-nginx\
            port:\
              number: 80

 

这个配置文件的作用是创建一个 Ingress，并且将 mynginx.com 的流量都转发到 my-nginx 这个 service 的80端口中去，在win机上编辑hosts文件（C:\Windows\System32\drivers\etc），加入 192.168.10.10 mynginx.com，浏览器访问 [http://mynginx.com:32536就能访问到我们部署的](http://mynginx.com:32536就能访问到我们部署的) Nginx Pod了。

 

不想这么麻烦的也可以在集群外搭建一个 MetalLB 实现负载均衡上面的步骤就都不用了，会给你生成一个能直接访问的集群外网地址，EXTERNAL-IP 项就不为 \<none\> 或 \<pending\> 了。

![[My-Case_runing_012_【Kubernetes k8s】Ubuntu-22.04搭建 k8s-1.30.1_019.png]]

图里的命令是我取的别名，不是正规命令

 

 

九、结尾的话

全部内容大概就是这样，给个免责声明吧：以上仅代表我个人观点，不可能全对，也可能有错的地方，如果后续有错了我会回来改正。后续不定时更新这篇博客的内容。

 

参考文献

[kubernetes官方文档](https://kubernetes.io/zh-cn/docs/home/)

[在Ubuntu22.04 LTS上搭建Kubernetes集群](https://blog.csdn.net/m0_51510236/article/details/136329885)

[k8s1.24+ dashboard不能自动生成token的问题](https://blog.csdn.net/kely6666/article/details/139002267)

[Kubernetes --- Dashboard](https://blog.csdn.net/qq_41619571/article/details/127217339)

[CentOS7-安装kubernetes v1.30.0版本](https://blog.csdn.net/a80C51/article/details/138254004)

[k8s集群部署时etcd容器不停重启问题及处理](https://blog.csdn.net/ldjjbzh626/article/details/128400797?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-2-128400797-blog-132977574.235%5Ev43%5Epc_blog_bottom_relevance_base2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-2-128400797-blog-132977574.235%5Ev43%5Epc_blog_bottom_relevance_base2&utm_relevant_index=3)
