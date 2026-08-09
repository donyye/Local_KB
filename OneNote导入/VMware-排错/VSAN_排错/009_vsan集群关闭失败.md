vsan集群关闭失败

2022年9月19日

16:03

 

案例：vCenter 7.0u3c 使用关闭 vSAN 群集向导功能导致集群故障一则

<https://www.sworditsys.com/share/virtualization/vcenter-7-0u3c-shutdown-vsan-cluster-results-in-broken-cluster.html>

![[VMware-排错_VSAN_排错_009_vsan集群关闭失败_001.png]]

 

 

解决方法：

[https://kb.vmware.com/s/article/87350](https://kb.vmware.com/s/article/87350)

[https://communities.vmware.com/t5/VMware-vSAN-Discussions/vCenter-7-0u3-shutdown-vSAN-cluster-results-in-broken-cluster/m-p/2885089](https://communities.vmware.com/t5/VMware-vSAN-Discussions/vCenter-7-0u3-shutdown-vSAN-cluster-results-in-broken-cluster/m-p/2885089)

 

![[VMware-排错_VSAN_排错_009_vsan集群关闭失败_002.png]]

关闭集群：是关闭vsan集群的服务并关闭hosts,vsan集群关机的作用，主要是为了断电维护。

关闭vsan：是拆解整个vsan的集群，但是只要磁盘不格式化还是可以重新组件和之前一样的vsan集群。

 

已使用 OneNote 创建。
