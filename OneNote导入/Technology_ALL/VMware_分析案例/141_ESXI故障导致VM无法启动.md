ESXI故障导致VM无法启动

2022年8月16日

13:43

因为ESXI节点挂了，无法在短时间里恢复，但是有总要的vm需要马上恢复使用。

 

而vm上有挂载了 93主机本地的ISO，虽然开了HA，但是在出现问题是无法迁移成功。

93主机硬件损坏，无法马上回来，目前测试把"已断开连接"这个vm重新运行到其它node上。

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_001.png]]

 

 

先尝试直接注册vm。

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_002.png]]

 

 

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_003.png]]

 

 

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_004.png]]

 

直接注册会出现下面错误。是因为此vm在VCSA数据库里还有记录，无法记录。

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_005.png]]

 

 

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_006.png]]

 

 

1. Stop the vpxd service

service-control \--stop vpxd

 

2. Connect to VCDB:

使用如下命令登录到 vcdb:

/opt/vmware/vpostgres/current/bin/psql -d VCDB -U postgres

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_007.png]]

FYI:   <https://kb.vmware.com/s/article/2004139?lang=zh_CN>

 

select vpx_entity.Name as \"VM Name\",vpx_vm.file_name as \"File Name / PATH\" from vpx_vm inner join vpx_entity on vpx_vm.id = vpx_entity.id order by vpx_entity.name;

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_008.png]]

 

 

3. Identify the VM ID in the vCenter database with this command:

select \* from vpx_entity where name like \'%Test-RH2%\';

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_009.png]]

 

 

4. Execute the below statements to remove the VM from the VCDB . 

从VCSA 的数据库里删除VC的记录

不同版本的ESXI不的命令不同

先停止 vpxd 服务： service-control \--stop vpxd

<https://virtual-power.in/f/removing-a-stale-vm-entry-from-vcenter-db>

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_010.png]]

VCSA 7.0

delete from  VPX_COMPUTE_RESOURCE_DAS_VM where VM_ID=xxxxx;

delete from  VPX_COMPUTE_RESOURCE_DRS_VM where VM_ID=xxxxx;

delete from  VPX_COMPUTE_RESOURCE_ORC_VM where VM_ID=xxxxx;

delete from  VPX_VM_SGXINFO where VM_ID=xxxxx;

delete from  VPX_GUEST_DISK where VM_ID=xxxxx;

delete from  VPX_VM_VIRTUAL_DEVICE where ID=xxxxx;

delete from  VPX_VM_DS_SPACE where VM_ID=xxxxx;

delete from  VPX_NON_ORM_VM_CONFIG_INFO where ID=xxxxx;

delete from  VPX_NORM_VM_FLE_FILE_INFO where VM_ID=xxxxx;

delete from  VPX_VDEVICE_BACKING_REL where VM_ID=xxxxx;

delete from  VPX_VIRTUAL_DISK_IOFILTERS where VM_ID=xxxxx;

delete from  VPX_VM_STATIC_OVERHEAD_MAP where VM_ID=xxxxx;

delete from  VPX_VM_TEXT where VM_ID=xxxxx;

delete from  VPX_VM where ID=xxxxx;

delete from  VPX_ENTITY where ID=xxxxx;

 

删除完成后重新启动vpxd服务

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_011.png]]

 

启动后你就无法发现那个vm。

 

然后再重新添加vm到其它node就可以了。

 

SSH登录 92 机器，在92上使用命令来重新注册此VM

vim-cmd solo/registervm /vmfs/volumes/vsanDatastore/Test-RH2/Test-RH2.vmx

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_012.png]]

 

 

可以看到vm已经注册到 92上，可以开机。

![[Technology_ALL_VMware_分析案例_141_ESXI故障导致VM无法启动_013.png]]

 

 

==== 完成 ====

 

已使用 OneNote 创建。
