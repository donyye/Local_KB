vmware控制台异常

2022年2月15日

10:05

通过vCenter web client 与 vsphere 访问虚拟控制台出现下面的情况。

无法显示，一直在转圈。vm系统是 windows

![[Technology_ALL_VMware_分析案例_135_vmware控制台异常_001.png]]

因为目前vm系统出现了问题需要修复，无法访问，想解决访问vm的问题。

 

此问题有两个可能性：

1. 客户端无法访问ESXI的 443或是 902端口。

<https://docs.vmware.com/cn/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-27A340F5-DE98-41A8-AC73-01ED4949EEF2.html>

防火墙必须允许 vSphere Client 访问端口 443 上的 vCenter Server。

防火墙必须允许 vCenter Server 访问端口 902 上的 ESXi 主机。

 

2. vm加载了vgpu也会这样

解决方法是，记住型号，删除vgpu，等修复了问题，再加回同型号的。

![[Technology_ALL_VMware_分析案例_135_vmware控制台异常_002.png]]

此案例就是因为vgpu的问题导致，删除vgpu后就能显示了。

 

 

 

已使用 OneNote 创建。
