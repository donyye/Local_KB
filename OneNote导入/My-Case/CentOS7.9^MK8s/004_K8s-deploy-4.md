K8s-deploy-4

星期一, 2025年12月29日

下午 4:31

Calico组件的安装

以下步骤只在master01执行（ .x 不需要更改）

# cd /root/k8s-ha-install && git checkout manual-installation-v1.23.x && cd calico/

 

修改Pod网段：

# POD_SUBNET=`cat /etc/kubernetes/manifests/kube-controller-manager.yaml | grep cluster-cidr= | awk -F= ''`

# sed -i "s#POD_CIDR#$#g" calico.yaml

# kubectl apply -f calico.yaml

 

查看容器和节点状态：

# kubectl get po -n kube-system

![[My-Case_CentOS7.9^MK8s_004_K8s-deploy-4_001.png]]

# kubectl get node -owide

![[My-Case_CentOS7.9^MK8s_004_K8s-deploy-4_002.png]]

---- 照样做完成没问题。

 

Metrics部署

在新版的Kubernetes中系统资源的采集均使用Metrics-server，可以通过Metrics采集节点和Pod的内存、磁盘、CPU和网络的使用率。

将Master01节点的front-proxy-ca.crt复制到所有Node节点

# scp /etc/kubernetes/pki/front-proxy-ca.crt k8s-node01:/etc/kubernetes/pki/front-proxy-ca.crt

(其他节点自行拷贝)

 

安装metrics server

# cd /root/k8s-ha-install/kubeadm-metrics-server

# kubectl  create -f comp.yaml 

serviceaccount/metrics-server created

clusterrole.rbac.authorization.k8s.io/system:aggregated-metrics-reader created

clusterrole.rbac.authorization.k8s.io/system:metrics-server created

rolebinding.rbac.authorization.k8s.io/metrics-server-auth-reader created

clusterrolebinding.rbac.authorization.k8s.io/metrics-server:system:auth-delegator created

clusterrolebinding.rbac.authorization.k8s.io/system:metrics-server created

service/metrics-server created

deployment.apps/metrics-server created

apiservice.apiregistration.k8s.io/v1beta1.metrics.k8s.io created

 

查看状态

# kubectl get po -n kube-system -l k8s-app=metrics-server

![[My-Case_CentOS7.9^MK8s_004_K8s-deploy-4_003.png]]

变成1/1     Running后

 

# kubectl top node

NAME          CPU(cores)  CPU%  MEMORY(bytes)  MEMORY%   

k8s-master01  153m        3%    1701Mi          44%       

k8s-master02  125m        3%    1693Mi          44%       

k8s-master03  129m        3%    1590Mi          41%       

k8s-node01    73m          1%    989Mi          25%       

k8s-node02    64m          1%    950Mi          24%       

# kubectl top po -A

NAMESPACE    NAME                                      CPU(cores)  MEMORY(bytes)   

kube-system  calico-node-6gqpb                          21m          85Mi   

kube-system  calico-node-bmvjt                          29m          76Mi   

kube-system  calico-node-hdp9c                          15m          82Mi   

kube-system  calico-node-wwrfv                          23m          86Mi   

kube-system  calico-node-zzv88                          22m          84Mi   

kube-system  calico-typha-67c6dc57d6-hj6l4              2m          23Mi   

kube-system  calico-typha-67c6dc57d6-jm855              2m          22Mi   

kube-system  coredns-7d89d9b6b8-sr6mf                  1m          16Mi     

kube-system  coredns-7d89d9b6b8-xqwjk                  1m          16Mi     

kube-system  etcd-k8s-master01                          24m          96Mi   

kube-system  etcd-k8s-master02                          20m          91Mi   

kube-system  etcd-k8s-master03                          21m          92Mi   

kube-system  kube-apiserver-k8s-master01                41m          502Mi  

kube-system  kube-apiserver-k8s-master02                35m          476Mi  

kube-system  kube-apiserver-k8s-master03                71m          480Mi  

kube-system  kube-controller-manager-k8s-master01      15m          65Mi   

kube-system  kube-controller-manager-k8s-master02      1m          26Mi     

kube-system  kube-controller-manager-k8s-master03      2m          27Mi     

kube-system  kube-proxy-8lt45                          1m          18Mi     

kube-system  kube-proxy-d6jfh                          1m          18Mi     

kube-system  kube-proxy-hfnvz                          1m          19Mi     

kube-system  kube-proxy-nsms8                          1m          18Mi     

kube-system  kube-proxy-xmlhq                          3m          21Mi     

kube-system  kube-scheduler-k8s-master01                2m          26Mi   

kube-system  kube-scheduler-k8s-master02                2m          24Mi   

kube-system  kube-scheduler-k8s-master03                2m          24Mi   

kube-system  metrics-server-d54b585c4-4dqpf            46m          16Mi

---- 照样做完成没问题。

 

Dashboard部署

Dashboard用于展示集群中的各类资源，同时也可以通过Dashboard实时查看Pod的日志和在容器中执行一些命令等。

 

安装指定版本dashboard

# cd /root/k8s-ha-install/dashboard/

[root@k8s-master01 dashboard]# kubectl  create -f .

serviceaccount/admin-user created

clusterrolebinding.rbac.authorization.k8s.io/admin-user created

namespace/kubernetes-dashboard created

serviceaccount/kubernetes-dashboard created

service/kubernetes-dashboard created

secret/kubernetes-dashboard-certs created

secret/kubernetes-dashboard-csrf created

secret/kubernetes-dashboard-key-holder created

configmap/kubernetes-dashboard-settings created

role.rbac.authorization.k8s.io/kubernetes-dashboard created

clusterrole.rbac.authorization.k8s.io/kubernetes-dashboard created

rolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created

clusterrolebinding.rbac.authorization.k8s.io/kubernetes-dashboard created

deployment.apps/kubernetes-dashboard created

service/dashboard-metrics-scraper created

deployment.apps/dashboard-metrics-scraper created

 

安装最新版

--- 注意这里是有一个指定版本和安装最新的版本，不要两个都安装。

官方GitHub地址： [https://github.com/kubernetes/dashboard](https://github.com/kubernetes/dashboard)

可以在官方dashboard查看到最新版dashboard

![[My-Case_CentOS7.9^MK8s_004_K8s-deploy-4_004.png]]

# kubectl apply -f <https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.3/aio/deploy/recommended.yaml>

2.0.3 以具体版本号为准

      # vim admin.yaml

apiVersion: v1

kind: ServiceAccount

metadata:

  name: admin-user

  namespace: kube-system

---

apiVersion: rbac.authorization.k8s.io/v1

kind: ClusterRoleBinding 

metadata: 

  name: admin-user

  annotations:

    rbac.authorization.kubernetes.io/autoupdate: "true"

roleRef:

  apiGroup: rbac.authorization.k8s.io

  kind: ClusterRole

  name: cluster-admin

subjects:

- kind: ServiceAccount

  name: admin-user

  namespace: kube-system

# kubectl apply -f admin.yaml -n kube-system

 

 

 登录dashboard

在谷歌浏览器（Chrome）启动文件中加入启动参数，用于解决无法访问Dashboard的问题，参考图1-1：

--test-type --ignore-certificate-errors

![[My-Case_CentOS7.9^MK8s_004_K8s-deploy-4_005.png]]

更改dashboard的svc为NodePort：

kubectl edit svc kubernetes-dashboard -n kubernetes-dashboard

![[My-Case_CentOS7.9^MK8s_004_K8s-deploy-4_006.png]]

将ClusterIP更改为NodePort（如果已经为NodePort忽略此步骤）：

查看端口号：

# kubectl get svc kubernetes-dashboard -n kubernetes-dashboard

![[My-Case_CentOS7.9^MK8s_004_K8s-deploy-4_007.png]]

根据自己的实例端口号，通过任意安装了kube-proxy的宿主机的IP+端口即可访问到dashboard：

访问Dashboard：[https://10.10.40.201:18282（请更改18282为自己的端口）](https://10.103.236.201:18282%EF%BC%88%E8%AF%B7%E6%9B%B4%E6%94%B918282%E4%B8%BA%E8%87%AA%E5%B7%B1%E7%9A%84%E7%AB%AF%E5%8F%A3%EF%BC%89/)，选择登录方式为令牌（即token方式），参考图1-2

![[My-Case_CentOS7.9^MK8s_004_K8s-deploy-4_008.png]]

查看token值：

[root@k8s-master01 1.1.1]# kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '')

Name:        admin-user-token-r4vcp

Namespace:    kube-system

Labels:      \<none\>

Annotations:  kubernetes.io/service-account.name: admin-user

              kubernetes.io/service-account.uid: 2112796c-1c9e-11e9-91ab-000c298bf023

 

Type:  kubernetes.io/service-account-token

 

Data

====

ca.crt:    1025 bytes

namespace:  11 bytes

token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi11c2VyLXRva2VuLXI0dmNwIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImFkbWluLXVzZXIiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiIyMTEyNzk2Yy0xYzllLTExZTktOTFhYi0wMDBjMjk4YmYwMjMiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06YWRtaW4tdXNlciJ9.bWYmwgRb-90ydQmyjkbjJjFt8CdO8u6zxVZh-19rdlL_T-n35nKyQIN7hCtNAt46u6gfJ5XXefC9HsGNBHtvo_Ve6oF7EXhU772aLAbXWkU1xOwQTQynixaypbRIas_kiO2MHHxXfeeL_yYZRrgtatsDBxcBRg-nUQv4TahzaGSyK42E_4YGpLa3X3Jc4t1z0SQXge7lrwlj8ysmqgO4ndlFjwPfvg0eoYqu9Qsc5Q7tazzFf9mVKMmcS1ppPutdyqNYWL62P1prw_wclP0TezW1CsypjWSVT4AuJU8YmH8nTNR1EXn8mJURLSjINv6YbZpnhBIPgUGk1JYVLcn47w

将token值输入到令牌后，单击登录即可访问Dashboard，参考图1-3：

![[My-Case_CentOS7.9^MK8s_004_K8s-deploy-4_009.png]]

必看】一些必须的配置更改

将Kube-proxy改为ipvs模式，因为在初始化集群的时候注释了ipvs配置，所以需要自行修改一下：

在master01节点执行

# kubectl edit cm kube-proxy -n kube-system

mode: ipvs

 

更新Kube-Proxy的Pod：

# kubectl patch daemonset kube-proxy -p "{\"spec\":{\"template\":{\"metadata\":{\"annotations\":}}}}" -n kube-system

验证Kube-Proxy模式

# [root@k8s-master01 1.1.1]# curl 127.0.0.1:10249/proxyMode

ipvs

【必看】注意事项

注意：kubeadm安装的集群，证书有效期默认是一年。master节点的kube-apiserver、kube-scheduler、kube-controller-manager、etcd都是以容器运行的。可以通过kubectl get po -n kube-system查看。

启动和二进制不同的是，

kubelet的配置文件在/etc/sysconfig/kubelet和/var/lib/kubelet/config.yaml，修改后需要重启kubelet进程

其他组件的配置文件在/etc/kubernetes/manifests目录下，比如kube-apiserver.yaml，该yaml文件更改后，kubelet会自动刷新配置，也就是会重启pod。不能再次创建该文件

kube-proxy的配置在kube-system命名空间下的configmap中，可以通过

# kubectl edit cm kube-proxy -n kube-system

进行更改，更改完成后，可以通过patch重启kube-proxy

# kubectl patch daemonset kube-proxy -p "{\"spec\":{\"template\":{\"metadata\":{\"annotations\":}}}}" -n kube-system

 

Kubeadm安装后，master节点默认不允许部署pod，可以通过以下方式打开：

查看Taints：

[root@k8s-master01 ~]# kubectl  describe node -l node-role.kubernetes.io/master=  | grep Taints

Taints:            node-role.kubernetes.io/master:NoSchedule

Taints:            node-role.kubernetes.io/master:NoSchedule

Taints:            node-role.kubernetes.io/master:NoSchedule

删除Taint：

[root@k8s-master01 ~]# kubectl  taint node  -l node-role.kubernetes.io/master node-role.kubernetes.io/master:NoSchedule-

node/k8s-master01 untainted

node/k8s-master02 untainted

node/k8s-master03 untainted

[root@k8s-master01 ~]# kubectl  describe node -l node-role.kubernetes.io/master=  | grep Taints

Taints:            \<none\>

Taints:            \<none\>

Taints:            \<none\>

如果只是删除一个master 节点：

# kubectl taint node k8s-master03 node-role.kubernets.io/master:NoSchedule-

---- 完成，能达到效果
