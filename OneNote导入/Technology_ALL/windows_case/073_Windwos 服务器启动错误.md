Windwos 服务器启动错误

2021年4月12日

11:19

 

 

Windows服务无法启动问题常见的排错方法

 

[http://www.etomlink.com/question/daili/2017/0407145.html](http://www.etomlink.com/question/daili/2017/0407145.html)

 

 

 

在微软新闻组里有很多网友咨询有关Windows服务无法启动的问题，例如无法启动"Logical Disk Manager"服务。这类服务出错的现象往往是五花八门，判断起来比较麻烦，而且有些问题还无法通过查看微软知识库文章得到解决。所以这里进行一个简单的小结，帮助初学者解决常见的服务无法启动的问题。 特别提醒在阅读本文的时候，请严格按照故障现象进行比对排错！如果涉及到注册表操作，请务必事先备份相关注册表项，并新建还原点。

如果系统无法顺利启动，请按Reset键重新开机，然后按F8，在Windows高级启动菜单上选择"恢复到最近一次的正确配置"菜单项，这样就可以先前的HKLM\\SYSTEM\\ControlSet00n覆盖错误配置的CurrentControlSet（ControlSet00n中的n由HKLM\\SYSTEM\\Select的LastKnownGood键值指定）。

错误2：系统找不到指定的文件

1．故障现象尝试在"服务"管理单元窗口手动启动服务是，系统提示"错误2：系统找不到指定的文件"(Error 2: The system cannot find the file specified.)，如图1所示。

![[Technology_ALL_windows_case_073_Windwos 服务器启动错误_001.jpg]]

2．原因分析

两种可能：

 

(1) 服务的可执行文件丢失或者被破坏。

(2) 服务相关注册表键值ImagePath的数值数据被篡改，导致SCM无法加载服务的可执行文件。在"服务"管理单元窗口里可以看到每个服务的可执行文件路径，请仔细检查如图2所示的可执行文件所在路径，如果和参照系统的正确配置不符合，说明注册表键值ImagePath的数值数据有误。如果此处的配置没有问题，则说明可执行文件丢失或者被破坏。 

 

![[Technology_ALL_windows_case_073_Windwos 服务器启动错误_002.jpg]]

 

3．解决办法

以"Task Scheduler"服务为例。

如果注册表键值ImagePath的数值数据被篡改，可以定位以下注册表项：

HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\Schedule

在右侧定位到ImagePath键值，将其数值数据修改为正确的值，并重启系统。

或者借助sc命令：

sc config Schedule binpath= \"%SystemRoot%\\System32\\svchost.exe -k netsvcs\"

 

如果是可执行文件丢失或者破坏，请用正确的副本进行替换，并重启系统。对于本例来说，可执行文件是svchost，如果该文件被破坏，系统将无法正常运行。

错误1053：服务没有及时相应启动或控制请求

 

1．故障现象

尝试在"服务"管理单元窗口手动启动服务时，系统提示"错误1053：服务没有及时相应启动或控制请求"，如图3所示。

![[Technology_ALL_windows_case_073_Windwos 服务器启动错误_003.jpg]]

2．原因分析

如图2所示，可执行文件的附加命令参数配置有误，会导致问题。

3．解决办法

参照上述的方法，用sc命令或者注册表编辑器，对附加的命令参数进行排错。

错误1083：配置成在该可执行程序中运行的这个服务不能执行该服务

1．故障现象

尝试在"服务"管理单元窗口手动启动服务时，系统提示"错误1083：配置成在该可执行程序中运行的这个服务不能执行该服务"，如图4所示。 

![[Technology_ALL_windows_case_073_Windwos 服务器启动错误_004.jpg]]

 2．原因分析

 

该故障通常在由svchost服务宿主进程所启动的服务上发生。大家知道Windows XP SP2最多可以启动七个svchost进程实例(实际上启动六个进程实例)，分别负责启动一组服务。每个svchost实例所负责启动的服务由以下注册表项决定：

HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\SvcHost

其下共有七个键值：DcomLaunch、HTTPFilter、imgsvc、LocalService、netsvcs、NetworkService、rpcss和termsvcs。每个键值都定义了一个或者多个服务，也就是对应每个svchost进程实例所能启动的一组服务。

本例中"Task Scheduler"服务的可执行程序参数是"svchost.exe -k netsvcs"，对应的svchost进程在启动该服务之前，会先到HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\SvcHost下的netsvcs键值里查找是否有该服务的定义，如果没有，就会出现该故障现象。

3．解决办法

很简单，首先打开该服务的属性对话框，查看其可执行程序的命令参数（本例是netsvcs），如图2所示。

然后进入以下注册表项：

HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\SvcHost

在右侧定位到对应的键值，本例是netsvcs，在其数值数据里添加该服务名即可，本例是Schedule，如图5所示，并重启系统。

![[Technology_ALL_windows_case_073_Windwos 服务器启动错误_005.jpg]]

 

提示 为什么通常只会启动六个svchost进程实例？都是TermService服务惹的祸！TermService(Terminal Services)这个服务非常另类，不仅仅出现在DcomLaunch组里，同时还独立出现在termsvcs组里，然而在"服务"管理单元窗口里，该服务的命令行为"svchost.exe -k DcomLaunch"，也就是说实际上并没有一个svchost进程实例负责启动termsvcs服务组！

 

错误126：找不到指定的模块

  

1．故障现象

 

尝试在"服务"管理单元窗口手动启动服务时，系统提示"错误126：找不到指定的模块"(Error 126: The specified module could not be found.)，如图6所示。

 

![[Technology_ALL_windows_case_073_Windwos 服务器启动错误_006.jpg]]

2．原因分析 该故障通常在由svchost服务宿主进程所启动的服务上发生。这一类的Windows服务，其实是以dll模块的形式插入某个svchost进程。如果该dll文件被破坏，或者注册表的相关键值被篡改，都可能导致问题。

这类服务所对应的Dll文件，是由HKLM\\SYSTEM\\CurrentControlSet\\Services\\ServiceName\\Parameters注册表项下的ServiceDll键值所定义的(此处的ServiceName是指服务名)，如果该注册表键值出错，或者对应的Dll文件被破坏，就会导致这个问题。在微软新闻组里有不少网友抱怨无法打开"磁盘管理"窗口，寻根溯源发现是"Logical Disk Manager"服务无法启动所导致。其中有一个case是系统被木马PCShare所感染，木马修改了"Logical Disk Manager"服务的注册表键值，把HKLM\\SYSTEM\\CurrentControlSet\\Services\\dmserver\\Parameters注册表项下的键值ServiceDll的数值数据指向木马的文件"%SystemRoot%\\System32\\drivers\\Ybfbqufe.sys"，尽管后来利用杀毒软件杀除木马，但是杀毒软件未能处理被木马篡改注册表键值，导致无法打开"磁盘管理"。

注意 不要将该故障和"错误2：系统找不到指定的文件"相混淆！

 

3．解决办法

 

对于"Logical Disk Manager"服务的问题，在以下的注册表项：

HKLM\\SYSTEM\\CurrentControlSet\\Services\\dmserver\\Parameters

确保将其下ServiceDll键值的数值数据修改为"%SystemRoot%\\System32\\dmserver.dll"。

如果注册表键值没有问题，请确保用正确的文件副本替换原来的dll文件，并重启系统。

    

错误1079：此服务的帐户不同于运行于同一进程上的其他服务的帐户

1．故障现象

尝试在"服务"管理单元窗口手动启动服务时，系统提示"错误1079：此服务的帐户不同于运行于同一进程上的其他服务的帐户"，如图7所示。

![[Technology_ALL_windows_case_073_Windwos 服务器启动错误_007.jpg]]

2．原因分析

该故障通常在由svchost服务宿主进程所启动的服务上发生。前面说过Windows XP SP2最多可以启动七个svchost进程实例，分别负责启动一组服务。一组服务中的每个服务必须和对应的svchost进程实例运行在同一个启动帐户下。

例如Alert服务属于LocalService组的服务，其对应的svchost进程实例运行在Local Service帐户下，如果错误地将Alert服务的启动帐户修改为别的帐户，例如Local System帐户，就会报错。

3．解决办法

 

首先根据该服务的可执行文件路径属性找到其所属的服务组，例如Alert服务属于LocalService的服务组，然后确定同一组的其他服务的启动帐户，将其修改为相同的启动帐户即可。

服务启动失败的原因是多种多样的，但一个普遍的解决方法，通常是注意系统给出的错误提示，以及eventvwr.msc中的系统日志。

 

From \<<http://www.etomlink.com/question/daili/2017/0407145.html>\>

 

 

 

已使用 OneNote 创建。
