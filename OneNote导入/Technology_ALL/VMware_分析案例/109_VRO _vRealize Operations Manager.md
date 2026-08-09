VRO \|vRealize Operations Manager

2020年4月13日

15:22

  ------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   RE: 有两个case需要帮忙 \| VRO IC:54344643
  From      Yin, Guoxun
  To        Ye, Dony
  Sent      2020年4月9日 11:14
  ------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Hi Dony,

从日志看，operation manager的tomcat serverlet组件坏掉了，如下面所附日志片段，这些动态配置无法人工修复，针对目前情况，我们提供的建议总结如下：

一：如果客户有配置operation manager HA，且当前存在好用的其他节点，可以从好用的节点上面按照以下步骤来copy文件到目前损坏的节点上看是否可以拯救该VM，如果仍然无法救回，那么建议重新部署该节点：

                登陆正常好用的节点，按照以下步骤执行：

                                cd /user/lib/vmware-vcops

                                tar czpf  tcwebapp.tgz tomcat-web-app

                拷贝上述打包的tgz文件到损坏的VM的/tmp目录下，依次执行以下命令

                                cd /usr/lib/vmware-vcops

rm -rf tomcat-web-app

tar xzpf /tmp/tcwebapp.tgz

rm -rf tomcat-web-app/work/\*

rm -rf tomcat-web-app/temp/\*

                检查/usr/lib/vmware-vcops/tomcat-web-app/conf/catalina.propertiesif文件中是否存在与当前operation manager 的URL网址不匹配的关键字，并予以修改，如IP或者主机名，如果没有该条目则忽略该步骤。

      shutdown -r now重启该VM   ，等待10分钟后尝试登陆，如果仍然失败，请重新部署该VM。

 

二：针对该VM appliance里面的 偶尔根分区会满的问题，有两种办法解决：

                1：针对8.x, 官方推荐的办法不是扩容，而是重启服务来释放rsyslog的临时文件，具体步骤如下文档所述：

                                <https://kb.vmware.com/s/article/76154?lang=en_US>

 

                2：如果客户想扩容/分区来规避该问题，我们附上了扩容8.X appliance的办法在下面，供用户参考，请看最下面的邮件部分 （注：直接给VM增加VMDK大小只是修改了虚拟磁盘，文件系统不会自动扩容，需要人工干预，如下面所附的办法）

                                =================8.x Appliance根分区/dev/sda4扩容办法：

 

 

====================Error log:

31-Mar-2020 01:04:13.112 INFO \[Distributed system shutdown hook\] org.apache.catalina.loader.WebappClassLoaderBase.checkStateForResourceLoading Illegal access: this web application instance has been stopped already. Could not load \[org.apache.geode.internal.cache.TXFarSideCMTracker\]. The following stack trace is thrown for debugging purposes as well as to attempt to terminate the thread which caused the illegal access.

                java.lang.IllegalStateException: Illegal access: this web application instance has been stopped already. Could not load \[org.apache.geode.internal.cache.TXFarSideCMTracker\]. The following stack trace is thrown for debugging purposes as well as to attempt to terminate the thread which caused the illegal access.

                                at org.apache.catalina.loader.WebappClassLoaderBase.checkStateForResourceLoading(WebappClassLoaderBase.java:1384)

                                at org.apache.catalina.loader.WebappClassLoaderBase.checkStateForClassLoading(WebappClassLoaderBase.java:1372)

                                at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1224)

                                at org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1185)

                                at org.apache.geode.internal.cache.TXCommitMessage.\<clinit\>(TXCommitMessage.java:88)

                                at org.apache.geode.internal.cache.GemFireCacheImpl.close(GemFireCacheImpl.java:2414)

                                at org.apache.geode.distributed.internal.InternalDistributedSystem.disconnect(InternalDistributedSystem.java:1372)

                                at org.apache.geode.distributed.internal.InternalDistributedSystem\$6.run(InternalDistributedSystem.java:2338)

                                at java.lang.Thread.run(Thread.java:748)

 

log4j:ERROR setFile(null,true) call failed.

java.io.FileNotFoundException: locktrace.log (Permission denied)

                at java.io.FileOutputStream.open0(Native Method)

                at java.io.FileOutputStream.open(FileOutputStream.java:270)

                at java.io.FileOutputStream.\<init\>(FileOutputStream.java:213)

                at java.io.FileOutputStream.\<init\>(FileOutputStream.java:133)

                at org.apache.log4j.FileAppender.setFile(FileAppender.java:294)

                at org.apache.log4j.RollingFileAppender.setFile(RollingFileAppender.java:207)

                at org.apache.log4j.FileAppender.activateOptions(FileAppender.java:165)

                at org.apache.log4j.config.PropertySetter.activate(PropertySetter.java:307)

                at org.apache.log4j.config.PropertySetter.setProperties(PropertySetter.java:172)

                at org.apache.log4j.config.PropertySetter.setProperties(PropertySetter.java:104)

                at org.apache.log4j.PropertyConfigurator.parseAppender(PropertyConfigurator.java:842)

                at org.apache.log4j.PropertyConfigurator.parseCategory(PropertyConfigurator.java:768)

                at org.apache.log4j.PropertyConfigurator.parseCatsAndRenderers(PropertyConfigurator.java:672)

                at org.apache.log4j.PropertyConfigurator.doConfigure(PropertyConfigurator.java:516)

                at org.apache.log4j.PropertyConfigurator.doConfigure(PropertyConfigurator.java:580)

                at org.apache.log4j.helpers.OptionConverter.selectAndConfigure(OptionConverter.java:526)

 

 

 

 

 

=================8.x Appliance根分区/dev/sda4扩容办法：

 

首先请检查当前VM是否存在任何快照，务必清理干净所有快照，如果有提示该VM需要整合快照，请务必执行快照整合并等待整合完成后再执行下面步骤

 

增加appliance VM 的第一个虚拟磁盘的容量，该磁盘为linux系统的sda，确认vmdk磁盘扩容，请对该VM做个快照用于意外恢复

 

下面gparted live cd，用于/dev/sda4扩容：

[https://gparted.org/livecd.php](https://gparted.org/livecd.php)

 

上传gparted到vmfs,  编辑该VM的设置，设置虚拟光驱指向上传的gparted iso. 确保下面两个红圈后都有勾选

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_001.png]]

 

同时编辑下图该属性，指定下一次重启直接进入虚拟机的BIOS，

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_002.png]]

 

然后正常重启该VM，如shutdown -r now等，不要执行power off之类的危险命令，   之后VM自动进入BIOS设置如下界面，请将CD-ROM移动到启动位置第一位，后按F10保存退出。

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_003.png]]

 

该项目默认即可，回车继续

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_004.png]]

 

 

语言可以选择33英语，或者其他想要的语言，请输入对应的数字代码

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_005.png]]

 

 

显示模式，此处直接回车继续即可

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_006.png]]

 

 

 

之后进入桌面会自动启动gparted 工具，会出现下面提示，请选择"Fix"

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_007.png]]

 

 

然后可以看到已经sda4后面有新增加的空间如下

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_008.png]]

 

在sda4上点右键，选择resize/move

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_009.png]]

 

 

把红圈处的箭头符号拖动到最后面，

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_010.png]]

 

 

然后选择Edit，apply all operations，保存设置，等待任务完成，

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_011.png]]

任务完成后，尾部会显示有部分未用空间，此空间不满足slice分配要求，没有影响，忽略即可。

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_012.png]]

 

然后退出gparted工具，双击桌面上的"exit"按钮，屏幕会提示如下，请将该虚拟机的虚拟光驱处更改为"host device"即移除挂载的ISO，然后再回到此界面按回车键即可重启该VM。

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_013.png]]

 

之后请等待 vm appliance重启完成，第一次完成后，在UI登陆界面，会有个data retriever初始化的过程，大概需要5分钟，请耐心等待。

请务必记得该VM还存在一个快照，验证当前扩容没问题后请记得删除该快照 (注：快照不是用来做长期备份使用的)

 

此时登陆系统执行命令即可看到sda4的扩容后的大小 (以下的容量仅做示例，实际扩容请按自己的datastore空余空间大小参考取值)

![[Technology_ALL_VMware_分析案例_109_VRO _vRealize Operations Manager_014.png]]

 

 

 

 

 

Best Regards

Guoxun

From: Ye, Dony \<dony_ye@Dell.com\>

Sent: 2020年4月8日 15:45

To: Yin, Guoxun

Subject: RE: 有两个case需要帮忙 \| VRO IC:54344643

 

Hi, Guoxun

 

客户提供了日志文件，我试了可以下载。

[http://106.15.35.13/s/NgxSpPGp3mxDzmN](http://106.15.35.13/s/NgxSpPGp3mxDzmN)

 

 

B R

Dony

 

 

 

已使用 OneNote 创建。
