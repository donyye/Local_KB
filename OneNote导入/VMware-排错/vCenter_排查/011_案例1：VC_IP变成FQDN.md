案例1：VC_IP变成FQDN

2023年4月12日

8:50

PNID 流程

 

先做个快照

 

VC 修改DNS

\# /opt/vmware/share/vami/vami_config_net

![[VMware-排错_vCenter_排查_011_案例1：VC_IP变成FQDN_001.png]]

 

这里加DNS自能加两个，第一个 127.0.0.1 不要改，默认的。

![[VMware-排错_vCenter_排查_011_案例1：VC_IP变成FQDN_002.png]]

点击会让你填入下一个DNS，只要两个就好。

按0可用看当前配置是否对。

另外不要在这里修改 Hostname，会有问题。

 

主机名字到 5480 这边改。

![[VMware-排错_vCenter_排查_011_案例1：VC_IP变成FQDN_003.png]]

 

需要注意的时 FQDN的名字，不能有下滑线，不符合规则，会报下面的错误。

Specified hostname is invalid

另外最好是都小写。

![[VMware-排错_vCenter_排查_011_案例1：VC_IP变成FQDN_004.png]]

 

 

另外成功更换FQDN过后，hosts 文件也会相应的自动被修改。

![[VMware-排错_vCenter_排查_011_案例1：VC_IP变成FQDN_005.png]]

 

 

 

 

已使用 OneNote 创建。
