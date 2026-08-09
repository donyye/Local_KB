FCOE 驱动 issue(vmware/linux)

Monday, October 20, 2014

4:33 PM

问题：

客户的M910配置了brocade 57810,需要配置FCOE。

无法把此网卡识别为HBA卡，只识别为网卡。（驱动不正确）

用DELL的esxi image安装，成功安装驱动后，可以识别到HBA卡，但是客户反馈一直无法识别到端口，客户一直怀疑是驱动的问题。

 

解决：在GuoXun的大力帮助下，首先检查了驱动：

装上驱动后在"storage adapter"和"network adapter"里面各自己看到自己的FC卡和网卡，如果都看到了，就证明驱动是OK的了.

后查看日志，发现客户的FCOE未激活。

esxcli fcoe adapter list

 

vmnic4:

   User Priority: 3

   Source MAC: 18:a9:9b:f9:5c:c7

   Active: false

   Priority Settable: false

   Source MAC Settable: false

   VLAN Range Settable: false

 

vmnic5:

   User Priority: 3

   Source MAC: 18:a9:9b:f9:5d:d0

   Active: false

   Priority Settable: false

   Source MAC Settable: false

   VLAN Range Settable: false

 

使用命令：esxcli fcoe nic discover --n vmnic4 和vmnic5来激活FCOE，问题解决。

但是客户发现重启后FCOE又需要再次激活，原来还需要做一个操作：

激活后运行/bin/auto-backup.sh一次即可

 

ESXi下面有/bin/auto-backup.sh 这个脚本，正常情况下是每半个小时自动在后台执行一次。

所以如果对ESXi做了什么配置，最好手动执行以下这个脚本，ESXi会把当前配置写到硬盘上，

附件是上次CASE的总结，供参考。

 

 

 

另外，如果是linux下FCOE的问题，可以用命令fcoeadm --i查看FCOE的情况。

 

已使用 OneNote 创建。
