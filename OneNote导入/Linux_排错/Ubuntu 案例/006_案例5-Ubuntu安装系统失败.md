案例5-Ubuntu安装系统失败

2024年12月6日

17:20

ubuntu安装系统失败： 22.04

 

安装到下面画面无法继续安装。需要格式化VD和初始化一下，在安装就可以了。

 

<https://askubuntu.com/questions/1452905/22-04-server-installer-crashes-when-probing-block-devices>

 

 

![[Ubuntu 案例_006_案例5-Ubuntu安装系统失败_001.png]]

 

把那个安装错误日志拿出来看：

InstallerLogInfo:

2024-10-08 12:13:12,595 INFO subiquity:130 Starting Subiquity TUI revision 5495 of snap /snap/subiquity/5495

2024-10-08 12:13:12,596 INFO subiquity:131 Arguments passed: \[\'/snap/subiquity/5495/lib/python3.10/site-packages/subiquity/\_\_main\_\_.py\'\]

2024-10-08 12:13:12,625 INFO subiquity/ErrorReporter/1728389579.593991518.ui/load:105 start: 

2024-10-08 12:13:13,043 INFO subiquity.common.errorreport:415 saving crash report \'Installer UI crashed with TypeError\' to /var/crash/1728389592.636571884.ui.crash

2024-10-08 12:13:13,055 INFO subiquity/ErrorReporter/1728389592.636571884.ui/add_info:105 start:

 

已使用 OneNote 创建。
