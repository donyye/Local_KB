Vmware_Powershell

2024年5月13日

12:49

Vmware Powershell 安装

FYI: <https://developer.broadcom.com/powercli/installation-guide>

 

1. 下载 VMware-PowerCLI-13.1.0-21624340

C:\\\> \$env:PSModulePath

C:\\Users\\Administrator\\Documents\\WindowsPowerShell\\Modules;C:\\Program Files\\WindowsPowerShell\\Modules;C:\\Windows\\system32\\WindowsPowerShell\\v1.0\\Modules

\# 通过这个命令找到 C:\\Program Files\\WindowsPowerShell\\Modules

\# 把软件解压到这个目录下 C:\\Program Files\\WindowsPowerShell\\Modules 目录下

 

2. 安装Vmware powershell 组件

Get-ChildItem \* -Recurse \| Unblock-File[  \<\--]解锁文件

Get-Module -Name VMware.PowerCLI -ListAvailable[  \<\--]执行以下命令以验证PowerCLI模块是否可用

![[VMware-排错_Other_001_Vmware_Powershell_001.png]]

安装完成

 

================================================

通过一个powershell 脚本来克隆vm

脚本部分： vm-clon.ps1

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--\
Import-Module VMware.VimAutomation.Core

\# Connect to vCenter Server

Connect-VIServer -Server \"10.10.40.249\" -User \"administrator@vsphere.local\" -Password \"P@ssw0rd!\"

 

\$vmName = \'RHEL7_9_1\'          

 

\$cloneName = \"\$(\$vmName)-clone-\$((Get-Date).ToString(\'MMddyyyy\'))\"

 

\$ds = Get-Datastore \| where \|

 

    Sort-Object -Property FreeSpaceGB -Descending \| select -First 1

 

\$esx = Get-Cluster -VM \$vmName \| Get-VMHost \| Get-Random

 

 

New-VM -VM \$vmName -Name \$cloneName -Datastore \$ds -VMHost \$esx

 

\# Disconnect from vCenter Server

Disconnect-VIServer -Server \"10.10.40.249\" -Confirm:\$false

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

运行克隆vm脚本，能看到一个进度条

![[VMware-排错_Other_001_Vmware_Powershell_002.png]]

 

完成后

![[VMware-排错_Other_001_Vmware_Powershell_003.png]]

 

 

 

 

 

已使用 OneNote 创建。
