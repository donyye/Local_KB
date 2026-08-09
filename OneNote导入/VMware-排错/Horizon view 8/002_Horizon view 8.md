Horizon view 8

2023年9月5日

9:52

\
前期工作：

1. 把系统加入到域里面去。

2. 在本地用户组里的 administrator 里添加在 AD 建的 cloudadmin 账户。

![[VMware-排错_Horizon view 8_002_Horizon view 8_001.png]]

 

完成后的样子

![[VMware-排错_Horizon view 8_002_Horizon view 8_002.png]]

3. 完成后再重启一下系统

\
安装 SQL server

 

![[VMware-排错_Horizon view 8_002_Horizon view 8_003.png]]

 

 

 

![[VMware-排错_Horizon view 8_002_Horizon view 8_004.png]]

 

一直下一步到这里，需要安装 .NET 

![[VMware-排错_Horizon view 8_002_Horizon view 8_005.png]]

 

到系统添角色，然后安装 旧版本的 .NET 。但是 Windows 2019 无法通过常规方式安装 3.5 版本，所有需要下载旧版本安装软件，放到D盘里，在添加角色里安装。

原始文件是 windows_server_2019_sxs.rar ，解压出 sxs 放到 D盘。

![[VMware-排错_Horizon view 8_002_Horizon view 8_006.png]]

 

![[VMware-排错_Horizon view 8_002_Horizon view 8_007.png]]

 

![[VMware-排错_Horizon view 8_002_Horizon view 8_008.png]]

 

 

![[VMware-排错_Horizon view 8_002_Horizon view 8_009.png]]

 

![[VMware-排错_Horizon view 8_002_Horizon view 8_010.png]]

 

![[VMware-排错_Horizon view 8_002_Horizon view 8_011.png]]

 

 

建议安装到 D 盘

![[VMware-排错_Horizon view 8_002_Horizon view 8_012.png]]

 

![[VMware-排错_Horizon view 8_002_Horizon view 8_013.png]]

 

 

 

 

 

已使用 OneNote 创建。
