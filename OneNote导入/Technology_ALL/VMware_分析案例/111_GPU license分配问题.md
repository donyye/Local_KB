GPU license分配问题

2020年4月16日

15:59

 

ESXI上的GPU在分配给VM用时一般是需要一个license server VM，这个VM是windwos或Linux。一般是windwos，上面有安装一个软件，作为分配GPU license用。

 

在VM上，VM会安装vgpu的driver，每当启动一台就会向license server VM请求一个license。

 

![[Technology_ALL_VMware_分析案例_111_GPU license分配问题_001.png]]

 

 

从提供的截图来看，客户有两种GPU license。

分别是 

VApps和VPC两种，分配是30和20个license。而VPC的license已经分配完了。

这个是在license server vm上看到的。

 

![[Technology_ALL_VMware_分析案例_111_GPU license分配问题_002.png]]

 

客户的问题是无法分配到license，这里会有两个问题。

1）虚拟机已经用完了所有的 20 个 license，已经无可用的分配。

2）license server VM 没有释放可用的license，导致可用license是0。

但这个问题与ESXI本身没有关系，因为两者配合的是GPU与VM里GPU驱动问题。

尝试解决的方法是重启一下license server VM，如果还是有相同的问题，需要寻求GPU方面解决。

 

============

VGPU 驱动下载地址，需要有NVDNA的账号才能下载，也可以使用企业邮箱下个90天的测试license。下载链接如下：

[https://www.nvidia.com/en-us/drivers/vgpu-software-driver/](https://www.nvidia.com/en-us/drivers/vgpu-software-driver/)

 

另外VGPU基本没有FW的下载更新

 

 

 

已使用 OneNote 创建。
