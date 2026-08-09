安全引导-secure boot

2025年2月21日

15:43

客户安装系统使用的 secure boot ，然后新添加网卡，发现开机后出现此问题。

 

 

![[Technology_ALL_VMware_分析案例_164_安全引导-secure boot_001.png]]

 

 

这种就是PCI设备固件信息不在我们BIOS里面的白名单导致\
FW没有对应的key让bios验证 

 

这是danfei的那个香港加X710的吧  好像那些卡固件有问题  升级下FW就好了吧 \
嗯，升级应该是搞定了

 

Guoxun 有参与的

 

已使用 OneNote 创建。
