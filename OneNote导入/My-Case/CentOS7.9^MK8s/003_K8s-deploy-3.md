K8s-deploy-3

星期一, 2025年12月29日

下午 4:30

集群初始化

官方初始化文档：

<https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/high-availability/>

以下操作只在master01节点执行

Master01节点创建kubeadm-config.yaml配置文件如下：

Master01：（# 注意，如果不是高可用集群，10.10.40.236:16443改为master01的地址，16443改为apiserver的端口，默认是6443，注意更改kubernetesVersion的值和自己服务器kubeadm的版本一致：kubeadm version）

注意:

以下文件内容，宿主机网段、podSubnet网段、serviceSubnet网段不能重复，具体看课程资料的【安装前必看】集群安装网段划分

以下操作在master01：

# vim kubeadm-config.yaml

apiVersion: kubeadm.k8s.io/v1beta2

bootstrapTokens:

- groups:

  - system:bootstrappers:kubeadm:default-node-token

  token: 7t2weq.bjbawausm0jaxury

  ttl: 24h0m0s

  usages:

  - signing

  - authentication

kind: InitConfiguration

localAPIEndpoint:

  advertiseAddress: 10.10.40.201

  bindPort: 6443

nodeRegistration:

  # criSocket: /var/run/dockershim.sock  # 如果是Docker作为Runtime配置此项

  criSocket: /var/run/containerd/containerd.sock # 如果是Containerd作为Runtime配置此项 （注意这里，格式要对齐，否则会报错。）

  name: k8s-master01

  taints:

  - effect: NoSchedule

    key: node-role.kubernetes.io/master

---

apiServer:

  certSANs:

  - 10.10.40.236

  timeoutForControlPlane: 4m0s

apiVersion: kubeadm.k8s.io/v1beta2

certificatesDir: /etc/kubernetes/pki

clusterName: kubernetes

controlPlaneEndpoint: 10.10.40.236:16443

controllerManager: 

dns:

  type: CoreDNS

etcd:

  local:

    dataDir: /var/lib/etcd

imageRepository: registry.cn-hangzhou.aliyuncs.com/google_containers

kind: ClusterConfiguration

kubernetesVersion: v1.23.0 # 更改此处的版本号和kubeadm version一致

networking:

  dnsDomain: cluster.local

  podSubnet: 172.16.0.0/12

  serviceSubnet: 192.168.0.0/16

scheduler: 

更新kubeadm文件

# kubeadm config migrate --old-config kubeadm-config.yaml --new-config new.yaml

将new.yaml文件复制到其他master节点，

# for i in k8s-master02 k8s-master03; do scp new.yaml $i:/root/; done

之后所有Master节点提前下载镜像，可以节省初始化时间（其他节点不需要更改任何配置，包括IP地址也不需要更改）：

# kubeadm config images pull --config /root/new.yaml 

 

所有节点设置开机自启动kubelet

# systemctl enable --now kubelet

(如果启动失败无需管理，初始化成功以后即可启动)

 

Master01节点初始化，初始化以后会在/etc/kubernetes目录下生成对应的证书和配置文件，之后其他Master节点加入Master01即可：

# kubeadm init --config /root/new.yaml  --upload-certs

--- 一次过初始化成功

如果初始化失败，重置后再次初始化，命令如下（没有失败不要执行）：

# kubeadm reset -f ; ipvsadm --clear  ; rm -rf ~/.kube

初始化成功以后，会产生Token值，用于其他节点加入时使用，因此要记录下初始化成功生成的token值（令牌值）：（它会在24小时后失效，失效后需要重新初始化，后续有说）

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

 

  mkdir -p $HOME/.kube

  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config

  sudo chown $(id -u):$(id -g) $HOME/.kube/config

 

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.

Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:

  <https://kubernetes.io/docs/concepts/cluster-administration/addons/>

 

You can now join any number of the control-plane node running the following command on each as root:

  kubeadm join 10.10.40.236:16443 --token 7t2weq.bjbawausm0jaxury \

 --discovery-token-ca-cert-hash sha256:df72788de04bbc2e8fca70becb8a9e8503a962b5d7cd9b1842a0c39930d08c94 \

 --control-plane --certificate-key c595f7f4a7a3beb0d5bdb75d9e4eff0a60b977447e76c1d6885e82c3aa43c94c

 

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!

As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use

"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

 

Then you can join any number of worker nodes by running the following on each as root:

 

kubeadm join 10.10.40.236:16443 --token 7t2weq.bjbawausm0jaxury \

 --discovery-token-ca-cert-hash sha256:df72788de04bbc2e8fca70becb8a9e8503a962b5d7cd9b1842a0c39930d08c94

Master01节点配置环境变量，用于访问Kubernetes集群：

# cat \<\<EOF \>\> /root/.bashrc

export KUBECONFIG=/etc/kubernetes/admin.conf

EOF

# source /root/.bashrc

查看节点状态：

![[My-Case_CentOS7.9^MK8s_003_K8s-deploy-3_001.png]]

采用初始化安装方式，所有的系统组件均以容器的方式运行并且在kube-system命名空间内，此时可以查看Pod状态：

![[My-Case_CentOS7.9^MK8s_003_K8s-deploy-3_002.png]]

--- 做到这里，能看到输出。前两个 pending 是正确的。

 

高可用Master

注意：以下步骤是上述init命令产生的Token过期了才需要执行以下步骤，如果没有过期不需要执行，直接join即可.

Token过期后生成新的token：（添加 node 可以直接使用输出加入）

kubeadm token create --print-join-command

 

Master需要生成--certificate-key

kubeadm init phase upload-certs  --upload-certs

这个是生成master的，但是需要拼接一些 node的，后加 --control-plane --certificate-key 再这个命令输出的 key。

 

Token没有过期直接执行Join就行了

其他master加入集群，master02和master03分别执行

# kubeadm join 10.10.40.236:16443 --token 7t2weq.bjbawausm0jaxury \

 --discovery-token-ca-cert-hash sha256:df72788de04bbc2e8fca70becb8a9e8503a962b5d7cd9b1842a0c39930d08c94 \

 --control-plane --certificate-key c595f7f4a7a3beb0d5bdb75d9e4eff0a60b977447e76c1d6885e82c3aa43c94c

 

查看当前状态：

![[My-Case_CentOS7.9^MK8s_003_K8s-deploy-3_003.png]]

 

Node节点的配置

Node节点上主要部署公司的一些业务应用，生产环境中不建议Master节点部署系统组件之外的其他Pod，测试环境可以允许Master节点部署Pod以节省系统资源。

# kubeadm join 10.10.40.236:16443 --token 7t2weq.bjbawausm0jaxury \

 --discovery-token-ca-cert-hash sha256:df72788de04bbc2e8fca70becb8a9e8503a962b5d7cd9b1842a0c39930d08c94

 

所有节点初始化完成后，查看集群状态

![[My-Case_CentOS7.9^MK8s_003_K8s-deploy-3_004.png]]

---- 完成没问题。
