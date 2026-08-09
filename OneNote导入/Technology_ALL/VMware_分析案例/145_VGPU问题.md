VGPU问题

2022年9月7日

10:10

Detail Symptom Descriptions(故障现象): 1. 服务器上将GPU与vSphere虚拟机直连时遇到了虚拟机无法启动的问题.在Ubuntu 20.04 和Windows 10上安装GPU驱动，安装failed. 2. vGPU功能我也在试，已经购买了NVIDIA GRID软件授权，并且已经在ESXi上安装了Host driver，并用vCenter给GPU配置了Shared Direct（见下图）。 但是我在给VM分配vGPU时，发现尽管在添加PCI device时有很多选项（如下图所示），但是只有第一个PCI device (nvidia_a40-1b)可以使用，如果给VM添加其他vGPU Profile，VM会无法启动，报错信息：No graphics device is available for vGPU \'nvidia_a40-1a\'.; Troubleshooting Steps(详细诊断步骤): 1. ESXi系统版本：7.0.3 GPU：NVIDIA Ampere A40 48GB (出厂自带的） NVIDIA GRID软件授权 ORDER NUM:484632230

 

SR: 1103429758

==============================================

1.当前OS没有许可

 

licenseKey = \'00000-00000-00000-00000-00000\',

 

VMware ESXi 7.0.3 build-19898904

VMware ESXi 7.0 Update 3

 

2.当前驱动已经加载

NVIDIA-VMware_ESXi_7.0.2_Driver  510.73.06-1OEM.702.0.0.17630552        NVIDIA   VMwareAccepted    2022-08-15

 

3.请用户确认其许可类型，不同许可支持的vgpu切分方案不同：

 

[https://docs.nvidia.com/grid/14.0/grid-vgpu-user-guide/index.html#vgpu-types-nvidia-a40](https://docs.nvidia.com/grid/14.0/grid-vgpu-user-guide/index.html#vgpu-types-nvidia-a40)

![[Technology_ALL_VMware_分析案例_145_VGPU问题_001.png]]

 

4.最后，nvidia许可和vgpu配置问题，用户可直接邮件联系如下支持邮箱获取支持：

[enterprisesupport@nvidia.com](mailto:enterprisesupport@nvidia.com)

 

已使用 OneNote 创建。
