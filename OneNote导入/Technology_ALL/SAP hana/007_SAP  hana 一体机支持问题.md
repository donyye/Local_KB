SAP[  hana ]一体机支持问题

2021年11月25日

14:12

分为两种：

1. appliance[  ]（一体机）

这种是属于工厂预先安装，客户拿到机器开机是需要一个初始化过程，客户需要填入相关的信息。里面的所有优化和空间大小都是固定好的，分区那些不能修改。

如果这种出现问题，那是按照流程来。

提交CASE到SAP，SAP会判断问题，如果系统有问题SAP那边会直接找SUSE那边处理。

判断是否一体机如下图，看是否有 Rack 标记。

![[Technology_ALL_SAP hana_007_SAP  hana 一体机支持问题_001.png]]

搜索 ： Rack

 

2.另外一种就是客户有购买了OS的OEM服务的，非一体机方式，客户自己负责去部署或是让EDT的人部署hana，这种就是有系统问题先找我们，SAP不管。

但是这种方式如果hana有性能问题什么的，我们是不管。

 

FYI :

<https://www.dell.com/support/kbdoc/en-au/000187272>

 

<https://dellservices.lightning.force.com/lightning/r/Lightning_Knowledge__kav/ka06P0000004O4TQAU/view>

 

 

 

两个ST 

3DSNGD3[  有]RACK

\[Rack\] SAP HANA SUSE Physical R940 / R740xd / R840-BMBW12

 

21CBVH3 没有 RACK，所以没有上面的标识。

 

 

 

SAP应该转给我们德国负责一体机的同事

 

![[Technology_ALL_SAP hana_007_SAP  hana 一体机支持问题_002.png]]

 

 

![[Technology_ALL_SAP hana_007_SAP  hana 一体机支持问题_003.png]]

 

![[Technology_ALL_SAP hana_007_SAP  hana 一体机支持问题_004.png]]

 

===================

\<\<red_hat_enterprise_linux-8-configuring_and_managing_networking-zh-cn.pdf\>\>

 

 

===================

Enterprise Linux OS, Non Factory Installed, Requires Subscription Selection

工厂没有预先安装

 

一体机都是预装好Dell imaged OS with SAP HANA的，用的是Dell engineering测试过的调试好的镜像在工厂就装好全部东西才发给客户的

 

![[Technology_ALL_SAP hana_007_SAP  hana 一体机支持问题_005.png]]

 

一体机是Dell engineering定制的镜像，经过认证的，问题比较少。像这两个是客户或部署团队装的，不是一体机，问题会比较多，SAP那边他们就不管的

 

一体机，开机后只要回答几个问题就会装好了，OS和SAP HANA全在里面了，相当于开机即用的

 

已使用 OneNote 创建。
