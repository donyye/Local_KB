案例2: VM-Passthru-GPU-CentOS7

2024年2月2日

12:11

 

vsphere8U2 + CentOS7 + passthru GPU

 

1. 设置VM高级参数

  ----------------------------------------------------------- ------
   pciPassthru.use64bitMMIO   TRUE
  pciPassthru.64bitMMIOSizeGB                                 256
  ----------------------------------------------------------- ------

 

![[VMware-排错_VM-方面_006_案例2_ VM-Passthru-GPU-CentOS7_001.png]]

 

 

 

2. 系统使用 EFI 的方式安装

一定要使用 EFI 否则后面 NVIDIA 驱动安装会失败，提示不安全的空间。

![[VMware-排错_VM-方面_006_案例2_ VM-Passthru-GPU-CentOS7_002.png]]

 

 

 

3. 需要给足够大的内存

![[VMware-排错_VM-方面_006_案例2_ VM-Passthru-GPU-CentOS7_003.png]]

 

4. 配置好VM系统的本地源

\[my-yum79\]

name=my-yum79

baseurl=ftp://10.10.40.79/yum_data/yum79

enabled=1

gpgcheck=0

 

\# 安装好所需要的包

yum groups install \"Development Tools\"

yum install kernel-devel

 

5. VM设置，需要把

\> 把系统自带驱动写到black list

\# cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf

blacklist nouveau

options nouveau modeset=0

 

\# cat /etc/modprobe.d/nvidia.conf

options nvidia NVreg_OpenRmEnableUnsupportedGpus=1

 

6. 跟新kernel (可选，做也成功)

\# dracut /boot/initramfs-\$(uname -r).img \$(uname -r)  \--force

 

7. 重启系统后安装驱动

\[root@localhost \~\]# ./cuda_12.2.0_535.54.03_linux.run

===========

= Summary =

===========

 

Driver:   Installed

Toolkit:  Installed in /usr/local/cuda-12.2/

 

Please make sure that

 -   PATH includes /usr/local/cuda-12.2/bin

 -   LD_LIBRARY_PATH includes /usr/local/cuda-12.2/lib64, or, add /usr/local/cuda-12.2/lib64 to /etc/ld.so.conf and run ldconfig as root

 

To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-12.2/bin

To uninstall the NVIDIA Driver, run nvidia-uninstall

Logfile is /var/log/cuda-installer.log

\# 安装成功

 

8. nvidia-smi 输出

![[VMware-排错_VM-方面_006_案例2_ VM-Passthru-GPU-CentOS7_004.png]]

 

 

 

参考： [https://blog.51cto.com/systemhuiyi/7388944](https://blog.51cto.com/systemhuiyi/7388944)

 

 

 

 

 

===================

 

[\[root@localhost \~\]# cat /etc/redhat-release ]

Red Hat Enterprise Linux release 9.2 (Plow)

 

[\[root@localhost \~\]# uname -a]

Linux localhost.localdomain 5.14.0-284.11.1.el9_2.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Apr 12 10:45:03 EDT 2023 x86_64 x86_64 x86_64 GNU/Linux

 

[\[root@localhost \~\]# lspci -vvD \|grep -i nvidia]

0000:03:00.0 3D controller: NVIDIA Corporation GA100 \[A100 PCIe 80GB\] (rev a1)

Subsystem: NVIDIA Corporation Device 1533

 

[\[root@localhost \~\]# cat /etc/modprobe.d/blacklist-nvidia-nouveau.conf ]

blacklist nouveau

options nouveau modeset=0

 

 

 

 

 

 

已使用 OneNote 创建。
