案例2： UEFI boot 修复

2023年4月10日

23:32

 

客户CentOS 7.9 系统重启后无法找到Boot引导，停留在下面的界面。

![[Linux引导修复_011_案例2： UEFI boot 修复_001.png]]

 

通过rescue进入，发现boot下面没有任何的文件。使用的是UEFI的模式。

Solution

修复此问题，需要先挂载ISO到 /mnt 。

 

1\. 重新安装boot 目录

\# cd /mnt/Packages

\# rpm -ivh kernel-xxxxxx.rpm \--force

 

2\. 安装UEFI grub2

\# yum reinstall grub2-efi-x64 shim-x64

or

\# rpm -ivh \--replacepkgs \--replacefiles grub2-efi-x64-\<version\>.rpm shim-x64-\<version\>.rpm

 

如果通过YUM 安装，yum配置

/etc/yum.repos.d/media.repo

\[InstallMedia\]

name=CentOS 7.9

mediaid=1521745267.626363

metadata_expire=-1

gpgcheck=0

baseurl=file:///mnt

cost=500

 

3\. 重建grub.cfg

\# grub2-mkconfig -o /boot/efi/EFI/contos/grub.cfg

 

4\. 重启系统问题解决

FYI: 

<https://access.redhat.com/solutions/3486741>

<https://www.unixarena.com/2018/05/rhel7-centos-7-recover-reinstall-grub2-with-uefi.html>

 

已使用 OneNote 创建。
