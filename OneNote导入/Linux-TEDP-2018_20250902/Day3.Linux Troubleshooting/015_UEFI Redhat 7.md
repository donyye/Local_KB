UEFI Redhat 7

2020年8月6日

12:30

- 在 /boot/efi/EFI/redhat/ 目录下的 grub.cfg 文件损坏或丢失。 

 

系统重启如下：

![[Linux-TEDP-2018_2025_Day3.Linux Troubleshootin_015_UEFI Redhat 7_001.png]]

 

进入安全模式，挂载chroot, 再使用grub2-mkconfig生成一下grub.cfg后，重启系统可以解决。

![[Linux-TEDP-2018_2025_Day3.Linux Troubleshootin_015_UEFI Redhat 7_002.png]]

 

\-\-\-\-\-- done \-\-\-\-\-\--

 

 

 

 

- [ /]boot/efi/EFI/ 的 redhat 目录被误删除情况下

 

出现问题的时候系统会不停的重启。

需要进入安全模式下，挂载iso配置一个本地的yum。

然后使用yum重新安装一下这三个包

yum reinstall grub2-efi shim grub2-tools -y

 

![Machine generated alternative text: bash-4 .Ztt yum reinstall grubZ-efi shim grubZ-tools Loaded plugins: langpacks, product-id, search-d isabled-repos, subscription-mnager This system is not registered with an entitlemnt sert.er. You can use subscription-mnager to register . Resolving Dependencies \--\> Running transaction check x86 64 x86 64 x86 64 \-\--\> Package \-\--\> Package \-\--\> Package Finished Dependenc i es Package Reinstall ing : grubZ-ef i-x64.x86_64 will be reinstalled grubZ-tools . x86 \_64 1:Z.BZ-B .65.e17_4.Z will be reinstal led shim-x64.x86 64 a:1z-1.e17 will Dependency Resolution Reso I t.æd be reinstal led (hrs i on grubZ-ef i -x64 grubZ-tools sh i m-x64 Transact ion Sunmry Reinsta 1 1 3 Packages Total download size: 3.5 M Installed size: 13 M Is this ok \[y/d/N\]: y Download ing packages : Tota I Running transaction check Running transaction test Transact ion test succeeded Running transaction Install ing . 1 :grubZ-t001s-Z .65.e17_4.Z.x86 64 Install ing . 1 :grubZ-ef .65.e17_4.Z.x86 64 Install ing : shim-x64-1Z-1.e17.x86 64 (hr i fyi ng . 1 :grubZ-ef .65.e17_4.Z.x86 64 (hr i fyi ng : shim-x64-1Z-1.e17.x86 64 (hr i fyi ng . 1 :grubZ-t001s-Z .65.e17_4.Z.x86 64 Instal led : grubZ-ef i -x64.x86 64 1:z.az-a.65.e17 4.2 bash-4 . Zit Repos i tory RHEL75 RHEL75 RHEL75 8B MB\'s 1.1M 1.8M 667 k 3.5 MB 1:z.az-a .65.e17_4.Z shim-x64.x86 64 a:1z-1.e17 ](attachments/Linux-TEDP-2018_2025_Day3.Linux%20Troubleshootin_015_UEFI%20Redhat%207_003.png)

安装完成后的截图

 

安装完成后可以看到"redhat"这个目录就会有了。

![[Linux-TEDP-2018_2025_Day3.Linux Troubleshootin_015_UEFI Redhat 7_004.png]]

 

检查"grubx64.efi"是否存在。然后就可以出现重启系统

![[Linux-TEDP-2018_2025_Day3.Linux Troubleshootin_015_UEFI Redhat 7_005.png]]

 

\-\-\-\-\-- done \-\-\-\-\-\--

 

 

已使用 OneNote 创建。
