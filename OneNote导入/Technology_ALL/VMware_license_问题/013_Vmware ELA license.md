Vmware ELA license

2022年12月7日

11:46

ELA license 是属于大批量采购的一种license，有OEM和非OEM两种。一般上了250K美金的都会走ELA。

ELA 是客户+Vmware+Dell 签署的一个合同

 

如果是OEM的，通过order可以看到购买了什么产品。暂时没例子order

如果是非OEM的，是看不到有什么产品的。如order：890616501

 

LKB : 000202503

Support and Subscription renewal for ELA:

 

ELA Type 1

Can be renewed by following the VMware renewal process which end in a new ELA Type 1 DELL order with custom sku

ELA Type 2

Can be renewed by following the VMware renewal process which end in a new ELA Type 2 DELL order and identified with a new VMware Software TAG and a custom SKU with list of products

 

 

ELA 的支持和订阅续订：

 

ELA 类型 1

可以按照 VMware 续订流程进行续订，该流程以带有自定义 sku 的新 ELA Type 1 DELL 订单结束

ELA 类型 2

可以按照 VMware 续订流程进行续订，该流程以新的 ELA 类型 2 DELL 订单结束，并使用新的 VMware 软件标签和带有产品列表的自定义 SKU 进行标识

 

 

====================

如果判断 ELA是否OEM

 

1. 在SFDC里搜索 order 号。

[ ]在下面可以看到有ST，有一个ST是是软件的。

![[Technology_ALL_VMware_license_问题_013_Vmware ELA license_001.png]]

 

2. 到查下面查ST

[7M0HZX2 - Service Tag Lookup \| Qi (dell.com)](https://quality.dell.com/search/?tag=7M0HZX2&at=1670552578607#AERO)

发现 Entitlements 是空的，没有列出产品说明是非OEM的。

 

![[Technology_ALL_VMware_license_问题_013_Vmware ELA license_002.png]]

 

 

3. 就算在order里那到下面的记录，也不一定是OEM的

![[Technology_ALL_VMware_license_问题_013_Vmware ELA license_003.png]]

 

如果有合同，里面会有说是L1 L2 或是 L3支持，如果是L3支持，如果是VMware直接support。

 

 

已使用 OneNote 创建。
