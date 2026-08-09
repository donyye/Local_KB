Kernel paink

2023年2月20日

9:20

![[Technology_ALL_Linux 问题收集_085_Kernel paink_001.png]]

 

 

重建initramfs不行

![[Technology_ALL_Linux 问题收集_085_Kernel paink_002.png]]

 

后面干脆重新装了kernel解决了

![[Technology_ALL_Linux 问题收集_085_Kernel paink_003.png]]

 

![[Technology_ALL_Linux 问题收集_085_Kernel paink_004.png]]

 

在更新之后，内核在引导时出现了错误: 

Unable to mount root fs on unknown-block(0,0)

<https://access.redhat.com/solutions/57018>

 

如何在 Red Hat Enterprise Linux 中重新构建初始 ramdisk 映像

<https://access.redhat.com/solutions/1958>

 

已使用 OneNote 创建。
