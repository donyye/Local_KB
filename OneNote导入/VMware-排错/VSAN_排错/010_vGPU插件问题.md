vGPU插件问题

2022年9月19日

16:06

SR-IOV没开启导致有vgpu的vm无法开机\
 

问题描述：\
报错：无法初始化 vGPU"nvidia_a16-1b"的插件"libnvidia-vgx.so"

 

<https://www.sworditsys.com/share/virtualization/could-not-initialize-plugin-libnvidia-vgx-so-for-vgpu-nvidia-a16-1b.html>

 

 

我们一位用户在对自家

[VDI](https://www.sworditsys.com/tag/vdi)桌面的GPU卡进行升级后，发现无法启动带有vGPU的虚拟机，报错如下：

 

![[VMware-排错_VSAN_排错_010_vGPU插件问题_001.png]]

 

报错信息：

无法初始化 vGPU"nvidia_a16-1b"的插件"libnvidia-vgx.so"。无法启动虚拟机，模块

 

于是用户与我们的工程师联系排查问题，我们的虚拟化工程师第一反应会不会是GPU卡驱动没有安装好，经过排查确认驱动正常，可以使用nvidia-smi命令：

![[VMware-排错_VSAN_排错_010_vGPU插件问题_002.png]]

 

接着我们的虚拟化工程师怀疑是不是GPU卡的ECC没关闭导致的异常，经查用户使用的是nVidia A16 GPU卡，在对A16的参数进行查询确认后，发现是支持vGPU模式下开启ECC功能。

 

![[VMware-排错_VSAN_排错_010_vGPU插件问题_003.png]]

 

具体关于ECC的描述可以参考官方文档：

[https://docs.nvidia.com/grid/latest/grid-software-quick-start-guide/index.html#disabling-enabling-ecc-memory](https://docs.nvidia.com/grid/latest/grid-software-quick-start-guide/index.html#disabling-enabling-ecc-memory)

NVIDIA Virtual GPU Software Packaging,Pricing, and Licensing Guide中也有对ECC的描述：

[https://www.nvidia.com/content/dam/en-zz/Solutions/design-visualization/solutions/resources/documents1/Virtual-GPU-Packaging-and-Licensing-Guide.pdf](https://www.nvidia.com/content/dam/en-zz/Solutions/design-visualization/solutions/resources/documents1/Virtual-GPU-Packaging-and-Licensing-Guide.pdf)

接着考虑到用户之前使用的GPU卡型号比较老，ESXi版本较低，我们也对虚拟机的兼容性进行了升级，但是仍然相同错误，无法启动。

在对ESXi及vCenter进行一些列检查后并未发现问题，突然我们一位工程师提到会不会是SR-IOV没有开，在NVIDIA vGPU 12.0版本以后部署安装vGPU需要启用SR-IOV，可以参考官方文档：

[https://docs.nvidia.com/grid/12.0/grid-vgpu-user-guide/index.html#prereqs-vgpu](https://docs.nvidia.com/grid/12.0/grid-vgpu-user-guide/index.html#prereqs-vgpu)

在对BIOS设置进行检查后，果然SR-IOV功能处于disable状态。

![[VMware-排错_VSAN_排错_010_vGPU插件问题_004.png]]

 

我们将其打开并重新引导ESXi操作系统后，安装了vGPU的虚拟机终于正常运行了起来！

 

![[VMware-排错_VSAN_排错_010_vGPU插件问题_005.png]]

 

 

 

 

已使用 OneNote 创建。
