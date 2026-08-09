Ubuntu-apt使用-与升级

2024年6月12日

17:15

1. 更新软件包列表

sudo apt update

更新软件包列表

 

2. 升级已安装的软件包

sudo apt upgrade

升级系统中所有已安装的软件包到最新的版本。

但它不会删除任何软件包，也不会安装新的软件包，除非这些软件包是已安装软件包的依赖。

 

3. 完成全面升级（可选）

sudo apt dist-upgrade

它会升级所有已安装的软件包，并且，如果需要的话，它会安装新的软件包或者删除已安装的软件包。这使得dist-upgrade能够处理更复杂的升级情况。

 

4. 升级到新的发行版本

sudo apt install update-manager-core[  ]\--\> 个包提供了一些用来升级Ubuntu系统的工具。

sudo do-release-upgrade[   \--\>] 执行之前需要先运行一下 "sudo apt dist-upgrade"

比如20.04 会升级到 22.04

 

 

sudo apt update         更新软件包列表

sudo apt install \<软件名\>         安装软件包

sudo apt remove \<软件名\>         删除软件包

sudo apt upgrade         升级所有已安装的软件包

apt search \<软件名\>         搜索软件包

apt show \<软件名\>         显示软件包的信息

 

 

 

Ubuntu 20.04中创建本地APT软件包仓库

<https://blog.csdn.net/beeworkshop/article/details/116094080>

 

已使用 OneNote 创建。
