答复: Tech share ：How to config  SSD cache for MD array

Wednesday, November 26, 2014

9:24 AM

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------
  主题       答复: Tech share ：How to config[  SSD cache for MD array ]
  发件人     Wu, Rongjun
  收件人     Yang, Jack; CCC XMN Enterprise ProSupport Storage; CCC XMN Enterprise ProSupport Storage 2
  抄送       CN XMN TS Server Coach
  发送时间   Wednesday, November 26, 2014 9:03 AM
  附件       \<\<新版MDSM管理软件常见设置\--脚本工具; 动态池; SSD cache的激活创建分配取消性能测试等.docx\>\>
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------

 

多谢jack，另外找到一个文档，share给大家。

 

 

发件人: Yang, Jack 

发送时间: 2014年11月26日 9:00

收件人: CCC XMN Enterprise ProSupport Storage; CCC XMN Enterprise ProSupport Storage 2; Wu, Rongjun

抄送: CN XMN TS Server Coach

主题: Tech share ：How to config SSD cache for MD array 

 

 

Team

有些同学询问怎样配置 SSD cache , 反馈没有实践文档，下面制作一个简介文档，同学可以参考。

1、添加SSD 硬盘， 在 unconfigured capacity  上点右键"create SSD cache .

![[Technology_ALL_未分类知识库_028_答复_ Tech share ：How to config  SSD cache_001.jpg]]

2、参数配置，可以选择不同的文件系统类型， 同时可以配置 SSD 容量，根据实际情况进行配置。

不过 最大容量 1862GB , 同时可以选择 enable 已有的 virtual disk ，或者根据实际需要自己选择配置 enable 那些 virtual disk 。

 

![[Technology_ALL_未分类知识库_028_答复_ Tech share ：How to config  SSD cache_002.jpg]]

 

3、配置 virtual disk  enable  SSD cache 

![[Technology_ALL_未分类知识库_028_答复_ Tech share ：How to config  SSD cache_003.jpg]]

 

![[Technology_ALL_未分类知识库_028_答复_ Tech share ：How to config  SSD cache_004.jpg]]

 

4、检查 SSD 配置参数， maximum capacity allowed 18620GB 

我配置的 是 3 块 700GB SSD 物理硬盘，配置为 SSD cache 。SSD cache type   read only , 只可以增加virtual disk read 性能。

![[Technology_ALL_未分类知识库_028_答复_ Tech share ：How to config  SSD cache_005.jpg]]

 

5、可以配置  添加或者移除 ssd 物理硬盘。

![[Technology_ALL_未分类知识库_028_答复_ Tech share ：How to config  SSD cache_006.png]]

 

 

Regards and Best Regards

 

Yang Jack

Senior ProSupport Engineer

Dell \| Global Support & Deployment

Office  +86 592  818-6585

Fax       +86 592  819-6585

How am I doing? 

Email my manager John_O\'hare@Dell.com with any feedback

 

![[Technology_ALL_未分类知识库_028_答复_ Tech share ：How to config  SSD cache_007.png]]

 

 

已使用 OneNote 创建。
