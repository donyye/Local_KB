VSAN坏盘定位

2022年9月14日

13:42

问题：全闪存的磁盘VSAN有个SSD换了

 

1. 用记事本记录报错磁盘与磁盘组的 naa 信息

![[Technology_ALL_VMware_分析案例_146_VSAN坏盘定位_001.png]]

 

 

 

![[Technology_ALL_VMware_分析案例_146_VSAN坏盘定位_002.png]]

 

 

2. SSH登录到这个ESXI

Vdq -iH

![[Technology_ALL_VMware_分析案例_146_VSAN坏盘定位_003.png]]

 

 

 

3. 通过改变磁盘灯的状态来定位错误的磁盘

通过下面命令关闭有问题硬盘的状态灯

esxcli storage core device set \--led-state=off -d naa.5002538c406b4bdb

 

注意\# 如果这个坏的磁盘已经完全失去控制，那么这个方法有可能不起作用。

 

4. 如果上述方法失败使用下面方法

1）看命令是否能确认是那个槽位的磁盘。从命令看是 slot2 槽位。

![[Technology_ALL_VMware_分析案例_146_VSAN坏盘定位_004.png]]

 

 

2）进一步的确认

红框是删除了问题的磁盘，做个标记，把它下面那个f4结尾的磁盘关灯。

esxcli storage core device set \--led-state=off -d naa.5002538e1160fbf4

![[Technology_ALL_VMware_分析案例_146_VSAN坏盘定位_005.png]]

另外一种方法是下面5提到的方式。

 

5. 在VC上开启所有正常磁盘的硬盘灯

看是否有一个与其它不一样。

 

![[Technology_ALL_VMware_分析案例_146_VSAN坏盘定位_006.png]]

 

被选中的磁盘的两个灯都会闪烁。

 

6.删除错误磁盘

选中错误的磁盘，然后点击删除磁盘，选不迁移数据。

这时有可能会出现下面的情况，移除按钮是灰色的，无法删除。

![[Technology_ALL_VMware_分析案例_146_VSAN坏盘定位_007.png]]

 

方法：

需要把ESXI上的VM迁走，然后进入维护模式，然后再尝试移除磁盘

如果还是无法移除可以尝试重启ESXI然后再移除

 

全闪存盘的组坏了一个整个磁盘组都需要删除重新做。

 

 

已使用 OneNote 创建。
