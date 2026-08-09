SUSE15 进入rescue

2020年12月10日

15:13

 

![[Technology_ALL_SUSE_analyze_005_SUSE15 进入rescue_001.png]]

 

![[Technology_ALL_SUSE_analyze_005_SUSE15 进入rescue_002.png]]

 

![[Technology_ALL_SUSE_analyze_005_SUSE15 进入rescue_003.png]]

 

 

 

![[Technology_ALL_SUSE_analyze_005_SUSE15 进入rescue_004.png]]

 

挂载根目录

想确定那个是根目录，挂载后可以确认一下，是否有 boot、root、proc等这些目录，如下图

![[Technology_ALL_SUSE_analyze_005_SUSE15 进入rescue_005.png]]

 

继续挂载其它目录

\# for i in proc sys dev; do mount \--rbind /\$i /mnt/\$i ; done

![[Technology_ALL_SUSE_analyze_005_SUSE15 进入rescue_006.png]]

 

 

Chroot

![[Technology_ALL_SUSE_analyze_005_SUSE15 进入rescue_007.png]]

 

![[Technology_ALL_SUSE_analyze_005_SUSE15 进入rescue_008.png]]

 

KB： [https://www.suse.com/zh-cn/support/kb/doc/?id=000018770](https://www.suse.com/zh-cn/support/kb/doc/?id=000018770)

 

 

 

 

 

已使用 OneNote 创建。
