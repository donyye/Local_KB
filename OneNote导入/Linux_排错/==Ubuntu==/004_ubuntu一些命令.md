ubuntu一些命令

2024年6月17日

15:36

\> 查看系统有多少个kernel\
\# ls /boot \| grep vmlinuz- 

\# sudo dpkg \--get-selections \| grep linux-image

 

\> 查看系统已安装的软件包

\# dpkg-query -l

\# sudo apt list \--installed \| grep \<package_name\>

 

\> 自动删除一下过期的kernel与不需要的软件包

root@user1:\~# sudo apt \--purge autoremove

Reading package lists\... Done

Building dependency tree\... Done

Reading state information\... Done

0 upgraded, 0 newly installed, 0 to remove and 3 not upgraded.

 

 

 

已使用 OneNote 创建。
