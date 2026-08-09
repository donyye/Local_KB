案例9：系统启动 Kernel paink

2023年2月20日

9:20

![[Linux引导修复_018_案例9：系统启动 Kernel paink_001.png]]

 

 

进入救援模式后重建initramfs后还是无法正常。

![[Linux引导修复_018_案例9：系统启动 Kernel paink_002.png]]

 

后面干脆重新装了kernel解决了

![[Linux引导修复_018_案例9：系统启动 Kernel paink_003.png]]

 

![[Linux引导修复_018_案例9：系统启动 Kernel paink_004.png]]

 

在更新之后，内核在引导时出现了错误: 

Unable to mount root fs on unknown-block(0,0)

<https://access.redhat.com/solutions/57018>

 

如何在 Red Hat Enterprise Linux 中重新构建初始 ramdisk 映像

<https://access.redhat.com/solutions/1958>

 

已使用 OneNote 创建。
