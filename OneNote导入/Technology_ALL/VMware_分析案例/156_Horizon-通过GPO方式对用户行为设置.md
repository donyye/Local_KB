Horizon-通过GPO方式对用户行为设置

2023年10月19日

10:23

\[T640\] \[IC: 178086387 \] \[PC: 178006012 \] \[ST: 6QR38K3 \]

1. 把 horizon 策略导入到AD域里。

<https://docs.vmware.com/en/VMware-Horizon/2306/horizon-remote-desktop-features/GUID-450CF9D6-E8C4-4DA2-B8DF-3DDE32AE4BEC.html>

 

下载 GPO[  ][https://my.vmware.com/web/vmware/downloads](https://my.vmware.com/web/vmware/downloads) 然后拷贝到AD 系统里。

注意你 horizon的版本，不同版本使用不同版本的 GPO

![[Technology_ALL_VMware_分析案例_156_Horizon-通过GPO方式对用户行为设置_001.png]]

 

 

![[Technology_ALL_VMware_分析案例_156_Horizon-通过GPO方式对用户行为设置_002.png]]

 

![[Technology_ALL_VMware_分析案例_156_Horizon-通过GPO方式对用户行为设置_003.png]]

 

下载完成后是一个 Extras Bundle 的文件，解压后如下：

![[Technology_ALL_VMware_分析案例_156_Horizon-通过GPO方式对用户行为设置_004.png]]

 

然后把 " \*.admx"域 en-US 文件拷贝到AD系统的 %systemroot%\\PolicyDefinitions 目录下，还有zh-CN。

 

2. 建立测试

此策略是通过控制登录的域账户来实现的，所以vm是需要有加域的。\
然后打开组策略管理，在组策略对象那里新建一个策略，右键选编辑，就到了组策略管理编辑器那里，能看到 horizon 的一些策略。

![[Technology_ALL_VMware_分析案例_156_Horizon-通过GPO方式对用户行为设置_005.png]]

 

 

Vmware Horizon Client 配置 \--\> 对瘦客户机的配置

Vmware View Agent \--\> 是对安装在虚拟桌面里的

 

下面是一个配置不允许用截屏的策略设置：

通过创建对应的GPO来配置某些权限

![[Technology_ALL_VMware_分析案例_156_Horizon-通过GPO方式对用户行为设置_006.png]]

像这个禁用VDI截屏的GPO，生效后  就没办法对VDI进行截屏操作

针对域里面的特定账户组或者账户做限制

譬如对所有VDI账户做限制 ，都是要在域控里面配置

 

 

 

 

已使用 OneNote 创建。
