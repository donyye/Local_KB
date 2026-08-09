Linux 格式化Fusion IO分区

Friday, August 12, 2016

4:36 PM

  -------------------------------------- ------------------------------------------------------------------
  主题       Fusion IO 卡性能调节分享
  发件人     Chen, David
  收件人     GSD_TAM_APJ_GC; CN XMN TS Server Coach
  发送时间   Friday, August 12, 2016 4:33 PM
  -------------------------------------- ------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Dear All,

 

最近步步高碰到了一个Fusion IO卡性能有问题case，在CentOS 6.5安装了驱动后直接分区格式化，结果性能很不理想。

 

咨询了Scandisk的工程师，在Linux下格式化工具并不能直接格式化Fusion IO，需要用Fusion IO的管理工具进行格式化，推荐512K的区块大小，步骤如下，重新格式化后性能恢复正常。

 

有碰到Fusion IO性能问题的兄弟可以参考一下，或者有别的好的实践也请共享一下，谢。

 

1.查看已安装的Fusion管理包，总共4个。

#rpm -qa\|grep fio

 

![[Technology_ALL_Linux 问题收集_021_Linux 格式化Fusion IO分区_001.jpg]]

 

2. 卸载卡，然后格式化为512K的格式，再重新挂载。

#fio-detach /dev/fct0

#fio-format -b 512 /dev/fct0

#fio-attach /dev/fct0

 

 

 

David Chen

 

Technical Account Manager

Dell \| Global Support Service

![[Technology_ALL_Linux 问题收集_021_Linux 格式化Fusion IO分区_002.jpg]]

Office: 0755-2532 1060 ; Mobile: +86 13178358029

E-mail [David_Chen@dell.com](mailto:David_Chen@dell.com)

How am I doing? Please contact my manager [Victor_Yeung@dell.com](mailto:Victor_Yeung@dell.com)

 

 

已使用 OneNote 创建。
