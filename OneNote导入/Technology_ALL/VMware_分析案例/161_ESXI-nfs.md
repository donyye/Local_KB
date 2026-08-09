ESXI-nfs

2024年1月8日

19:03

IC :[  182759954]\
客户ESXI 挂载了来自 NetApp 的 NFS

 

它是NFS存储设备。 NFS 共享文件夹安装到 ESXi 主机一切正常。

在此 NFS 数据存储上创建 VM 失败，但在 VMFS 上一切正常。

使用 root 登录 ESXi 命令行，NFS 读写一切正常。

NFS 导出 NETAPP 存储的权限一切正常。

检查虚拟机创建日志，它指出在虚拟机创建过程中无法锁定 vmx 文件

原因总结问题的根本原因，并包括指定受众的技术详细信息。

 

解决方案总结用于解决问题的步骤，并包括有关所需操作的详细说明。

这不是 ESXi 问题，请告诉客户检查存储操作系统上是否启用了"NetAPP Fpolicy"，此功能将阻止在虚拟机创建或开机过程中创建"lck"文件。 只需更改或禁用 Fpolicy，这对于 NFS 存储上的虚拟机操作来说都是有好处的。

 

<https://kb.netapp.com/onprem/ontap/da/NAS/VMware_ESXI_cannot_power_on_VM%2C_create_new_VM%2C_or_revert_snapshots_after_Native_Fpolicy_was_enabled_in_System_Manager>

 

 

 

已使用 OneNote 创建。
