分享\--14G服务器使用GPU设置成VGPU模式不能启动Xrog服务和GPU设置成Pass-Through不能启动虚拟机

2018年6月15日

9:19

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       分享\--14G服务器使用GPU设置成VGPU模式不能启动Xrog服务和GPU设置成Pass-Through不能启动虚拟机
  发件人     Chen9, Jack
  收件人     CN XMN TS ENT L2 SME
  发送时间   2018年6月13日 23:45
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

 

Dell - Internal Use - Confidential 

Dear all,

 

      客户反馈一台R740服务器安装ESXi6.5的系统，显卡使用的是M10，驱动安装的是NVIDIA-VMware_ESXi_6.5_Host_Driver_390.42-1OEM.650.0.0.4598673，之前一直插在上面使用直通模式，但是3D建模比较卡顿，其它使用正常，根据客户的问题疑似使用直通模式未发挥完全性能，通过调试改成VGPU模式，但是比较奇怪，xorg服务无法启动，从而不能分配切分的显卡。问题是SSH运行/etc/init.d/xorg start不显示任何执行反馈，包括在web界面下启用xorg服务，虚拟机不能启动，下方任务显示成功完成，但是服务仍是停止状态。

![[Technology_ALL_VMware_分析案例_092_分享--14G服务器使用GPU设置成VGPU模式不能启动Xrog服务和GPU设置成_001.jpg]]

 

![[Technology_ALL_VMware_分析案例_092_分享--14G服务器使用GPU设置成VGPU模式不能启动Xrog服务和GPU设置成_002.jpg]]

  

 

【解决方法】

 

14G服务器使用GPU设置成VGPU模式不能启动Xrog服务和GPU设置成Pass-Through不能启动虚拟机，需要到BIOS里将Memory Mapped I/O Base设置12TB。

 

![[Technology_ALL_VMware_分析案例_092_分享--14G服务器使用GPU设置成VGPU模式不能启动Xrog服务和GPU设置成_003.png]]

 

![[Technology_ALL_VMware_分析案例_092_分享--14G服务器使用GPU设置成VGPU模式不能启动Xrog服务和GPU设置成_004.png]]

 

参考资料：

[http://topics-cdn.dell.com/pdf/vmware-esxi-6.5.x_release-notes_en-us.pdf](http://topics-cdn.dell.com/pdf/vmware-esxi-6.5.x_release-notes_en-us.pdf)

[https://kb.vmware.com/s/article/2142307](https://kb.vmware.com/s/article/2142307)

[https://gridforums.nvidia.com/default/topic/2099/dell-resources/p40-with-dell-740xd-nvidia-smi-failed-to-initialize-nvml-unknown-error/](https://gridforums.nvidia.com/default/topic/2099/dell-resources/p40-with-dell-740xd-nvidia-smi-failed-to-initialize-nvml-unknown-error/)

 

Jack

2018-6-13

 

 

已使用 OneNote 创建。
