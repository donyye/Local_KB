K8s-Kubernetes-架构介绍

 

Kubernetes 是什么？

 

![[My-Case_runing_010_K8s-Kubernetes-架构介绍_001.png]]

 

 

 

Kubernetes 架构

 

![[My-Case_runing_010_K8s-Kubernetes-架构介绍_002.png]]

 

 

 

Master 节点

![[My-Case_runing_010_K8s-Kubernetes-架构介绍_003.png]]

 

Node 节点

 

. Kube-proxy

· Kube-proxy用于管理service的访问入口，包括集群内pod到service的访 问和集群外访问service

· Kubelet

· Kubelet是在集群内每个节点中运行的一个代理，用于保证pod的运行

· 容器引擎

· 通常使用docker来运行容器，也可使用rkt等做为替代方案

 

 

推荐Add-ons

 

·除了上述组件外,kubernetes使用中通常需要一些额外的组件实现特 定功能,常用的Add-ons包括:

 

· Core-dns:为整个集群提供DNS服务

· Ingress Controller：为service提供外网访问入口

· Dashboard:提供图形化管理界面

· Heapster：提供集群资源监控

· Flannel：为kubernetes提供方便的网络规划服务

 

 

Kubeadm[    ][https://kubernetes.p2hp.com/docs/](https://kubernetes.p2hp.com/docs/)

![[My-Case_runing_010_K8s-Kubernetes-架构介绍_004.png]]

 

 

![[My-Case_runing_010_K8s-Kubernetes-架构介绍_005.png]]

 

 

 

![[My-Case_runing_010_K8s-Kubernetes-架构介绍_006.png]]

 

 

 

![[My-Case_runing_010_K8s-Kubernetes-架构介绍_007.png]]
