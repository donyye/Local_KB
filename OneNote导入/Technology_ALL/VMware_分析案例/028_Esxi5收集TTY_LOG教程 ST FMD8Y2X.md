Esxi5收集TTY_LOG教程 ST FMD8Y2X

Tuesday, August 12, 2014

9:16 AM

  -------------------------------------- ---------------------------------------------------------------------------------------------------------
  主题       Re: 转发: Esxi5收集TTY_LOG教程 ST FMD8Y2X
  发件人     [chris.lu@cn.atlascopco.com](mailto:chris.lu@cn.atlascopco.com)
  收件人     Wen, Jeffery
  发送时间   Wednesday, March 05, 2014 1:25 PM
  附件       \<\<perc_0305.7z\>\>
  -------------------------------------- ---------------------------------------------------------------------------------------------------------

您好，附件为日志收集脚本，解压密码为dell,请参照下列步骤收集TTY_log，并将收集好的日志文件回复本邮箱，谢谢！

 

1、  开启Esxi服务器SSH远程登录端口：

![[Technology_ALL_VMware_分析案例_028_Esxi5收集TTY_LOG教程 ST FMD8Y2X_001.jpg]]

 

2、  将附件脚本上传到Esxi服务器/tmp目录

上传脚本方法1：使用SSH上传工具，如WinSCP软件,WinSCP. [http://winscp.net/eng/index.php](http://winscp.net/eng/index.php) 

登录服务器： 

 

![[Technology_ALL_VMware_分析案例_028_Esxi5收集TTY_LOG教程 ST FMD8Y2X_002.png]]

   

 

浏览到存储脚本文件的目录然后将文件拷贝到Esxi主机上，也可以直接将文件拖拽到右边的服务器/tmp目录下：

![[Technology_ALL_VMware_分析案例_028_Esxi5收集TTY_LOG教程 ST FMD8Y2X_003.png]]

 

上传方法2：使用vSphere客户端上传到存储器后移动到/tmp目录：

浏览存储器：

![[Technology_ALL_VMware_分析案例_028_Esxi5收集TTY_LOG教程 ST FMD8Y2X_004.jpg]]

点击上传文件，浏览到脚本文件存放目录后上传：

![[Technology_ALL_VMware_分析案例_028_Esxi5收集TTY_LOG教程 ST FMD8Y2X_005.jpg]]

 

使用putty远程登录Esxi服务器，附件有putty软件压缩包，解压密码为dell：

![[Technology_ALL_VMware_分析案例_028_Esxi5收集TTY_LOG教程 ST FMD8Y2X_006.jpg]]

 

上传的脚本文件存放路径/vmfs/volumes/datastore_name目录下，建议将脚本移动到/tmp：

![[Technology_ALL_VMware_分析案例_028_Esxi5收集TTY_LOG教程 ST FMD8Y2X_007.jpg]]

 

 

 

3、  赋予脚本可执行权限，然后运行脚本，生成日志：

#cd /tmp

#chmod a+x esxi5_TTY.sh

#./esxi5_TTY.sh

![[Technology_ALL_VMware_分析案例_028_Esxi5收集TTY_LOG教程 ST FMD8Y2X_008.png]]

 

将生成的日志存放在/tmp目录下，可以通过WinSCP或vSphere客户端拷到本地：

![[Technology_ALL_VMware_分析案例_028_Esxi5收集TTY_LOG教程 ST FMD8Y2X_009.png]]

 

已使用 OneNote 创建。
