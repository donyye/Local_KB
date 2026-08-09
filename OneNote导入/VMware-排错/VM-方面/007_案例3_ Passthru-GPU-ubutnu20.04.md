案例3: Passthru-GPU-ubutnu20.04

2023年7月21日

23:05

 

ESXI8.0 + A100 X 2

vm:

ubutnu 20.04

RHEL8.7

 

直通两个GPU

 

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_001.png]]

 

添加给一台 ubutnu 20.04.02 ，另外需要保留内存，内存也不能分配太少。

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_002.png]]

 

UEFI 安全引导一定要去掉勾选，否则会出现很多莫名奇妙问题。后续讲述

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_003.png]]

 

高级设置里需要添加这两项，否则vm启动会报 模块"DevicePowerOn"打开电源失败 无法启动。

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_004.png]]

  ----------------------------- ------
  pciPassthru.use64bitMMIO      TRUE
  pciPassthru.64bitMMIOSizeGB   256
  ----------------------------- ------

在vm vmare.log 看的错误，在没有加这两项的情况下\
2023-07-21T09:40:52.413Z In(05) vmx - PCIPassthru: total number of pages needed (33566722) exceeds limit (917504), failing

FYI: [https://www.dell.com/support/kbdoc/en-us/000199172/pci-passthrough-module-devicepoweron-power-on-failed](https://www.dell.com/support/kbdoc/en-us/000199172/pci-passthrough-module-devicepoweron-power-on-failed)

错误截图

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_005.png]]

 

 

vm驱动安装部分

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_006.png]]

 

添加执行权限

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_007.png]]

 

如果按照过程有错误会让你去检查相应的日志

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_008.png]]

 

检查日志发现缺少一些开发包，通通安装上。

\# apt install build-essential[  \--\> ]安装开发库和工具，或加上 gcc-multilib dkms

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_009.png]]

 

查看gcc版本

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_010.png]]

 

再次运行 ./cuda_12.2.0_535.54.03_linux.run 

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_011.png]]

 

主要这里是包含驱动的，也可以不安装驱动。

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_012.png]]

 

安装完成

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_013.png]]

 

使用 nvidia-smi 测试

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_014.png]]

 

 

 

 

RHEL8.7 vm的安装GPU驱动

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_015.png]]

 

这里是通过本地RPM包进安装

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_016.png]]

 

但是安装到这步的时候出现错误，所以换一种方式。

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_017.png]]

 

 

使用[  ./cuda_12.2.0_535.54.03_linux.run ]进行安装

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_018.png]]

 

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_019.png]]

 

安装完成

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_020.png]]

 

测试：

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_021.png]]

 

 

安装时遇到的问题

运行时提示安装失败，查看log并没有明确的说明

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_022.png]]

 

继续查看 /var/log/nvidia-installer.log 日志，提示是由moude没有被加载。

其实问题出现在vm使用了EFI安全引导导致。切记不能勾选。去掉后就能成功安装驱动了。

 

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_023.png]]

 

 

另外它还会导致 ubutnu 安装成功 nvidia 驱动，就是无法加载问题，如下图：

![[VMware-排错_VM-方面_007_案例3_ Passthru-GPU-ubutnu20.04_024.png]]

 

或者是RHEL8.7，无正确安装驱动。

 

 

 

 

 

 

 

 

 

已使用 OneNote 创建。
