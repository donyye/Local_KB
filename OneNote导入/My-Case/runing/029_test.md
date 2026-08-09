test

 

1. 所有都要 Ready 状态

![[My-Case_runing_029_test_001.png]]

 

2. 所有pod的状态都要是 Runing 状态， READY都是 1/1 才是正常，前后数字一样。， 1/2 都不正常。

![[My-Case_runing_029_test_002.png]]

如果是二进制搭建可能没有etcd的pod。重启的次数也不需要关心。

 

3. 检查集群的网段是否正常

# kubectl get svc

![[My-Case_runing_029_test_003.png]]

 

# get po -A -owide

![[My-Case_runing_029_test_004.png]]

 

所以这里一共有三个网段

1）192.168.00 是 Service 网段

2）172.16.0.0 是pod网段

3）10.10.40.0 是 master node 和 node 使用

如果有两个是一样的，要看子网掩码是否有冲突，如果有那只能重新搭建了。因为修改需要很多地方，时间也很长，也不一定会成功

 

4. 能否正常创建资源

kubectl create deploy cluster-test --image=registry.cn-beijing.aliyuncs.com/dotbalo/debug-tools -- sleep 3600

![[My-Case_runing_029_test_005.png]]

 

这样才是正常

![[My-Case_runing_029_test_006.png]]

 

部署到那个 node

![[My-Case_runing_029_test_007.png]]

 

# kubectl get deploy cluster-test -oyaml --\> 这个命令可以查看到部署的详细的信息。

 

5. pod 必须需要能解析 Service

能解析，IP地址能出来，有些报错也没关系。

192.168.0.1 能出来就行

这里看到时失败的：

![[My-Case_runing_029_test_008.png]]

 

6 发送键到所有node

所有node能出来这个信息就可以，不需要一样。说明能访问就可以。

![[My-Case_runing_029_test_009.png]]

 

7 所有 node ping 都能通。

![[My-Case_runing_029_test_010.png]]

 

通过 kubectl get po -owide 找其它的 node 的IP地址

找一个 node2 的机器，然后了进到node1里去 ping node2 的 IP 地址
