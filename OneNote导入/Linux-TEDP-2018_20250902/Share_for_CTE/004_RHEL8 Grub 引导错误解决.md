RHEL8 Grub 引导错误解决

2022年4月24日

14:38

RHEL8 的gpt格式引导

 

测试，删除 rm -rf /boot/efi/EFI/redhat/grub.cfg

 

1. 系统开机进入grub状态

![[Linux-TEDP-2018_2025_Share_for_CTE_004_RHEL8 Grub 引导错误解决_001.png]]

 

2. 手动引导过程

\> ls

\> ls (hd0,gpt2)/

\> Set root=\'hd0,gpt2\'

\> linuxefi /vmlinuz-4.xxxxxxxx root=/dev/sdax

\> linuxefi /initramfs-4.xxxxxxxx.img

\> boot

![[Linux-TEDP-2018_2025_Share_for_CTE_004_RHEL8 Grub 引导错误解决_002.png]]

 

需要注意的是，我知道root 是在sda4上，如下：

![[Linux-TEDP-2018_2025_Share_for_CTE_004_RHEL8 Grub 引导错误解决_003.png]]

 

![[Linux-TEDP-2018_2025_Share_for_CTE_004_RHEL8 Grub 引导错误解决_004.png]]

 

最后 boot 就能成功引导了。

 

如果 root= 这里不知道根目录是那个，那会去到 \# 提示符状态。

 

3. 修复grub.cfg文件

[\[root@rh8a redhat\]# grub2-mkconfig \> grub.cfg]

Generating grub configuration file \...

Adding boot menu entry for EFI firmware configuration

done

 

重新启动系统，正常启动。

 

===第二种方式===

 

进入rescue模式：

选择1

![[Linux-TEDP-2018_2025_Share_for_CTE_004_RHEL8 Grub 引导错误解决_005.png]]

 

直接回车 使用chroot挂载目录

![[Linux-TEDP-2018_2025_Share_for_CTE_004_RHEL8 Grub 引导错误解决_006.png]]

 

 

![[Linux-TEDP-2018_2025_Share_for_CTE_004_RHEL8 Grub 引导错误解决_007.png]]

 

输入两个exit后退出救援模式

 

===================

 

![[Linux-TEDP-2018_2025_Share_for_CTE_004_RHEL8 Grub 引导错误解决_008.png]]

 

 

![[Linux-TEDP-2018_2025_Share_for_CTE_004_RHEL8 Grub 引导错误解决_009.png]]

 

 

 

 

已使用 OneNote 创建。
