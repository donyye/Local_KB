Ubuntu-引导

2024年9月1日

11:25

ubuntu 引导出现问题检查与修复

===============================================

在Ubuntu 16.04.4上的测试成功：

1\. grub\> ls

2\. grub\> ls (hd0,msdos1)/

3. grub\> set root=(hd0,msdos1)

4. grub\> linux16 /vmlinuz-4.13.0.0-36-generic root=/dev/mapper/ubuntu\--vg-root

5. grub\> initrd16 /initrd.img-4.13.0-36-generic

6. grub\> boot

7\. 系统启动成功

================================================

 

========== 过时 ============

1. 找到boot分区在什么地方：

   会罗列所有的磁盘分区信息，比方说：

     (hd0,1),(hd0,5),(hd0,3),(hd0,2)

grub rescue\> ls (hd0,5)/

\...\....

2.  调用如下命令

grub rescue\> set prefix=(hd0,5)/boot/grub

grub rescue\> set root=(hd0,5)

grub rescue\> insmod (hd0,5)/boot/grub/linux.mod

grub rescue\> linux /vmlinuz root=dev/sda5 root

grub rescue\> initrd /initrd.img

grub rescue\> boot

![[__Ubuntu___009_Ubuntu-引导_001.png]]

 

重启引导后：

![[__Ubuntu___009_Ubuntu-引导_002.png]]

 

3. 不过不要高兴，如果这时重启，问题依旧存在，我们需要进入Linux中，对grub进行修复。

    进入Linux之后，在命令行执行：

    sudo update-grub

    sudo grub-install /dev/sda

    （sda是你的硬盘号码，千万不要指定分区号码，例如sda1，sda5等都不对）

 

4. 重启测试是否已经恢复了grub的启动菜单？ 恭喜你恢复成功！

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

grub rescue\>insmod /boot/grub/normal.mod

在修复win7和ubuntu的时候出错的情况，提示 normal.mod找不到

可以试试：

grub rescue\>insmod normal.mod

成功解决

 

已使用 OneNote 创建。
