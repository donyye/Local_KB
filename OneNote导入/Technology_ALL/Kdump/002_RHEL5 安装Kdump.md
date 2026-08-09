RHEL5 安装Kdump

Thursday, December 12, 2013

4:37 PM

How to configure kdump of RHEL5 安装Kdump(Installing Kdump)

//检查kexec-tools 是否安装

\# rpm -q kexec-tools

//安装kexec-tools 包

\# rpm -ivh kexec-tools-\<version\>.rpm

GUI 界面配置Kdump( file on disk )

 

 

1 - 使用system-config-kdump 命令启动kdump 配置界面。

或在Applications \--\> System Tools \--\> Kdump 运行。

 

![[Technology_ALL_Kdump_002_RHEL5 安装Kdump_001.png]]

 

 

 

2 - 选择"Enable kdump"，即可编辑相应参数。

\- New kdump Memory：给kdump kernel 分配多少内存。

- Location：指定存放vmcore 的位置。点击"Edit Location"，选择file，默认存放到 /var/crash/。

 

![[Technology_ALL_Kdump_002_RHEL5 安装Kdump_002.png]]

 

 

3 - 配置完毕后，会提示重启，载入新的设定。

![[Technology_ALL_Kdump_002_RHEL5 安装Kdump_003.png]]

 

 

 

4 - 由于配置未生效，所以kdump 启动失败，忽略此FAILED，重启即可。

![[Technology_ALL_Kdump_002_RHEL5 安装Kdump_004.png]]

 

 

 

5 - 检查/boot/grub/grub.conf 文件，发现在kernel 后会自动添加上crashkernel=128M@16M。

![[Technology_ALL_Kdump_002_RHEL5 安装Kdump_005.png]]

 

Note：

crashkernel=128M@16M 会根据grub.conf 的默认引导项的kernel 后边自动添加。

 

 

 

6 - 检查/etc/kdump.conf 文件，由于配置成file:///var/crash/，所以此文件未做任何配置。

![[Technology_ALL_Kdump_002_RHEL5 安装Kdump_006.png]]

Note：

此时启用kdump 失败。如下：

 

 

7 - 系统重启之后，检查kdump 服务。

\# chkconfig \--list \| grep kdump

kdump 0:off 1:off 2:on 3:on 4:on 5:on 6:off

\# service kdump status

Kdump is operational ß 表示kdump 已启用

 

 

8 - Testing Kdump

\- 建缓冲区的数据同步到磁盘上

\# sync

\- 触发crash dump

\# echo c \> /proc/sysrq-trigger

\- 检查生成的dump 文件，服务器自动重启后，在服务器端查询vmcore

\# ll /var/crash/2010-07-13-11:33/vmcore -h

-r\-\-\-\-\-\-\-- 1 root root 640M Jul 13 11:34 vmcore

 

 

建议：

配置dump 环境，是在系统hang 住的情况下，将整个内存中的数据，以文件形式镜像到

指定位置。

所以，磁盘空间至少要保留与内存同样大小的空间。

如果空间不足，小于内存的大小，可以根据实际情况选用以下调整方式：

1 -- 在kernel 中添加参数"mem="指定kernel 能识别的内存大小。

例如：指定kernel 识别10G 内存空间，mem=10240M。

2 -- 可再kdump.conf 文件中添加参数，使在收集dump 文件时，将内存中的空白页剔除。

例如：添加core_collector makedumpfile -d 31 --c，意思是剔除空白页，并压缩。

 

 

 

 

已使用 OneNote 创建。
