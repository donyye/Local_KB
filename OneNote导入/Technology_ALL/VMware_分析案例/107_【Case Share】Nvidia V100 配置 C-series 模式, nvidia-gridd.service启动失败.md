【Case Share】Nvidia V100 配置 C-series 模式, nvidia-gridd.service启动失败

2020年4月2日

15:19

  ------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   【Case Share】Nvidia V100 配置 C-series 模式, nvidia-gridd.service启动失败
  From      Zhang, Ji Fu
  To        CN XMN TS ENT L2 SME
  Sent      2020年4月2日 15:03
  ------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

问题：

两台服务器 GPU V100配置在gridd-V100d-C-series模式的情况下, nvidia-gridd.service启动失败; 另外两台机器配置在gridd-V100d-Q-series模式下正常

![[Technology_ALL_VMware_分析案例_107_【Case Share】Nvidia V100 配置 C-series 模式, n_001.jpg]]

 

 

解决方案：

参考配置手册, 按照如下要求配置VM,问题解决\| Passthrough for a GPU PCI device 的方式也要满足如下要求

[https://docs.nvidia.com/grid/10.0/grid-vgpu-release-notes-vmware-vsphere/index.html](https://docs.nvidia.com/grid/10.0/grid-vgpu-release-notes-vmware-vsphere/index.html) 

![[Technology_ALL_VMware_分析案例_107_【Case Share】Nvidia V100 配置 C-series 模式, n_002.jpg]]

 

 

图形界面具体操作方法和截图：

1."Edit Settings -\> VM Options -\> Boot Options" in order to get to the "Firmware" parameter. Ensure that the "UEFI" or "EFI" is enabled in the Firmware area as shown below

![[Technology_ALL_VMware_分析案例_107_【Case Share】Nvidia V100 配置 C-series 模式, n_003.png]]

 

2\. "Edit Settings → VM Options→ EDIT CONFIGURATION → ADD CONFIGURATION PARAMS"

![[Technology_ALL_VMware_分析案例_107_【Case Share】Nvidia V100 配置 C-series 模式, n_004.png]]

 

![[Technology_ALL_VMware_分析案例_107_【Case Share】Nvidia V100 配置 C-series 模式, n_005.png]]

 

![[Technology_ALL_VMware_分析案例_107_【Case Share】Nvidia V100 配置 C-series 模式, n_006.png]]

 

 

 

附：

关联的VMware KB 

[https://kb.vmware.com/s/article/2142307?lang=en_US](https://kb.vmware.com/s/article/2142307?lang=en_US) 

VMware vSphere VMDirectPath I/O: Requirements for Platforms and Devices (2142307)

![[Technology_ALL_VMware_分析案例_107_【Case Share】Nvidia V100 配置 C-series 模式, n_007.jpg]]

 

 

Thanks and Regards,

 

Steven Zhang

Senior Engineer, Server Support

Dell EMC \| Technical Support

 

 

已使用 OneNote 创建。
