关于VM ESXI8 vcls 开不起来

2023年9月1日

8:48

 

开启 HA 后 vcls 无法正常启动，报错如下：

 

打开虚拟机电源 [虚拟机vCLS-455cea70-1f55-418d-b824-d515bfeb0ab0](javascript:void(0)) 没有与虚拟机兼容的主机。com.vmware.vim.eam0 毫秒2023/09/01 08:56:112023/09/01 08:56:1110 毫秒 [vCenter servervcsa.dcompass.com](javascript:void(0))

Beginning of Expandable row content Screen reader table commands may not work for viewing expanded content, please use your screen reader\'s browse mode to read the content exposed by this button

任务名称

 打开虚拟机电源

状态

 没有与虚拟机兼容的主机。

启动者

 com.vmware.vim.eam

对象

 [虚拟机vCLS-455cea70-1f55-418d-b824-d515bfeb0ab0](javascript:void(0))

服务器

 [vCenter servervcsa.dcompass.com](javascript:void(0))

错误堆栈:

 目标主机不支持虚拟机的当前硬件要求。

 使用已启用增强型 vMotion 兼容性 (EVC) 的集群，在整个集群中创建一个统一的 CPU 功能集，或者使用每虚拟机 EVC 为虚拟机创建一个一致的 CPU 功能集，并允许虚拟机移动到能够支持这个 CPU 功能集的主机。请参见知识库文章 1003212 了解集群 EVC 信息。

 不支持 MWAIT。

 

截图如下：

![[Technology_ALL_VMware_分析案例_155_关于VM ESXI8 vcls 开不起来_001.png]]

 

因为这个环境是测试环境，ESXI8也是vm，所以有mwait 无法开启的问题，因为虚拟BIOS没有这个选项，而物理机器有能开启，所以不会有这个问题。

 

解决方法

 

登录 ESXi web client，找到那个VCLSVM，

点右键，给它升级虚拟硬件版本到最新

然后在VC里，给它开启VM EVC，开了EVC再关闭VM EVC，然后就可以开机了

三个VCLS VM都开机正常了，

那么再开启HA就成功了，

你可以照我这个方法看能不能搞定，

我这里测试成功了

 

 

![[Technology_ALL_VMware_分析案例_155_关于VM ESXI8 vcls 开不起来_002.png]]

 

![[Technology_ALL_VMware_分析案例_155_关于VM ESXI8 vcls 开不起来_003.png]]

 

 

![[Technology_ALL_VMware_分析案例_155_关于VM ESXI8 vcls 开不起来_004.png]]

 

 

 

已使用 OneNote 创建。
