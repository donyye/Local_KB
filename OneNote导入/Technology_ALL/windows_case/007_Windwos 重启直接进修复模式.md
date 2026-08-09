Windwos 重启直接进修复模式

2021年1月4日

12:50

 

![[Technology_ALL_windows_case_007_Windwos 重启直接进修复模式_001.png]]

 

![[Technology_ALL_windows_case_007_Windwos 重启直接进修复模式_002.png]]

 

![[Technology_ALL_windows_case_007_Windwos 重启直接进修复模式_003.png]]

 

![[Technology_ALL_windows_case_007_Windwos 重启直接进修复模式_004.png]]

 

运行 chkdsk c: /f 查看错误输出。下面是举个例子，这个系统没有错误。

 

![[Technology_ALL_windows_case_007_Windwos 重启直接进修复模式_005.png]]

 

==========================================

恢复注册表：

进入命令提示符

》如果你看到 Windows 文件夹，那么你在正确的驱动器号，如果没有，请尝试其他的盘符。

![[Technology_ALL_windows_case_007_Windwos 重启直接进修复模式_006.png]]

 

 

![[Technology_ALL_windows_case_007_Windwos 重启直接进修复模式_007.png]]

 

 

 

》输入以下命令在 config 文件夹中创建一个文件夹来临时备份文件，该文件夹恰好存储了一个 Registry 副本，然后按 Enter 键:

![[Technology_ALL_windows_case_007_Windwos 重启直接进修复模式_008.png]]

 

》输入以下命令在包含注册表备份的 RegBack 中移动，然后按 Enter:

![[Technology_ALL_windows_case_007_Windwos 重启直接进修复模式_009.png]]

警告: 在运行 dir 命令之后，文件(SYSTEM，SOFTWARE，SAM，SECURITY，DEFAULT)的大小应该与您在屏幕快照中看到的大小相似。如果任何文件显示0，不要继续，因为你将无法修复你的 Windows [ ]安装和你的设备可能停止启动

 

》输入以下命令将文件从 RegBack 文件夹复制到配置文件夹，并在每个问题上按 Enter 和 y 确认:

![[Technology_ALL_windows_case_007_Windwos 重启直接进修复模式_010.png]]

 

 

》从右上角点击关闭(x)按钮。

》一旦你完成了这些步骤，你的计算机将重新启动，Windows 应该能够正确启动。

 

 

FYI：https://pureinfotech.com/restore-registry-backup-windows-10/

 

 

 

 

 

 

 

 

 

 

 

 

====================

安全模式

如果F8进不了安全模式，可以在OS启动时关机，连关三次就可以进入安全模式。

![[Technology_ALL_windows_case_007_Windwos 重启直接进修复模式_011.png]]

 

 

已使用 OneNote 创建。
