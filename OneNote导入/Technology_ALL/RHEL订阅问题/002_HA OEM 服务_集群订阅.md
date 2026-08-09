HA OEM 服务\|集群订阅

2020年9月22日

16:53

在订单里，看到 528开头的一般是OEM服务。AA开头的是S&P服务。

![[Technology_ALL_RHEL订阅问题_002_HA OEM 服务_集群订阅_001.png]]

 

 

另外，有HA license, 但机器没有PS for HA, 所以他只能用HA功能, 

 

OEM OS + ProSupport = Dell Support

OEM HA + ProSupport for HA = Dell Support

 

 

 

有 HA 支持的

  ----------------------------------------------------------------------------------------
  1Yr ProSupport for Software, RHEL, 1-2 Socket, Unlimited VMs, High Availability Add-On
  3Yr ProSupport for Software, RHEL, 1-2 Socket, Unlimited VMs, High Availability Add-On
  5Yr ProSupport for Software, RHEL, 1-2 Socket, Unlimited VMs, High Availability Add-On
  ----------------------------------------------------------------------------------------

 

他有HA license, 但机器没有PS for HA, 所以他只能用HA功能, 但没有HA support service. 这不难理解吧?

 

比较你如果没有RHEL license, 请问你哪里下载的ISO license, 你连到哪里更新包. 如果你能下载到ISO那一定是login通过别人的ID拿到的, 这也是不合法的

 

========

2020.11.13

目前HA的支持有变。邮件 RH-HA order support case

 

After discussion, APJ and Global PM confirm :

1.If the HA order(without PS4SW SKU) used on PSP system, the comprehensive support will be provided by TS/SST until Collaborative Support is required .

2.If the HA order(without PS4SW SKU) used on ProSupprot system, collaborative support will be provied by TS (Customers need to have corresponding service contracts with vendor before TS start to collaborate) .

3.PM has aligned with SST for this process.

 

经过讨论，APJ 和 Global PM 确认：

1.如果HA订单（无PS4SW SKU）用于PSP系统，TS/SST将提供全面的支持，直到需要Collaborative Support。

2.如果在ProSupprot系统上使用的HA订单（无PS4SW SKU），将由TS提供协同支持（TS开始协同前，客户需要与供应商签订相应的服务合同）。

3.PM 已针对此过程与 SST 保持一致。

 

 

邮件： 转发[: \[Work Instruction\] ProSupport Plus]全面软件支持流程（PSP Comprehensive Software Support）

RHEL-HA订单中的产品信息：Red Hat Enterprise Linux,1-2 SKT,1-2VMs,1Year,High Availability Add-On,CUS

销售模式：Sales在销售时，有可能会只卖一个HA（即HA Only），也有可能在卖HA的同时加卖一个ProSupport for Software(PS4SW)的服务（即HA+PS4SW）。

TS/SST服务模式：（注：遇到 HA Only+PSP的组合时，TS在升级时除了提供HA Order外，还需要说明是使用在PSP的主机上及相应ST）

  ------------ -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------
  主机服务     HA Only                                                                                                                                                                                                                                                                                                                                      HA+PS4SW
  Basic        No Support                                                                                                                                                                                                                                                                                                                                   全面软件支持(包括SST)
  ProSupport   协作支持服务                                                                                                                                                                                                                                                                                                                                  
  PSP          先提供全面软件支持(包括SST)，直到SST判断需要引入软件原支持厂商后转协作支持服务    
  ------------ -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------

注意：协作支持服务需要客户与软件厂商之间有相应的支持服务合同。

 

 

=========================================

HA 的订阅，客户会收到邮件，得到一个产品注册号, 用于digital locker（Dell的数字储物柜）注册, 注册后, 从digital locker拿到定阅号, 再到red hat里添加定阅。

这个时间，系统就有了两个订阅，一个是系统自身的订阅，通过ST换的，另外一个就是这个HA的订阅。

 

![[Technology_ALL_RHEL订阅问题_002_HA OEM 服务_集群订阅_002.png]]

 

![[Technology_ALL_RHEL订阅问题_002_HA OEM 服务_集群订阅_003.png]]

 

![[Technology_ALL_RHEL订阅问题_002_HA OEM 服务_集群订阅_004.png]]

 

![Machine generated alternative text: e Huang, Antti 11:25 11:26 digital locker Huang, Antti 11:30 11:33 %-HAB9iTlN, 41\] digital locker Huang, Antti 11:39 E+digital lockerXHff, Adigital hatæäjLlElN ](attachments/Technology_ALL_RHEL订阅问题_002_HA%20OEM%20服务_集群订阅_005.png)

 

 

======================

ST： C3056V3 有服务的

satellite是要买ProSupport for Software Smart Management才有支持的

 

3Yr ProSupport for Software RHEL Smart Management 1-2 Socket

Red Hat Enterprise Linux Smart Management, 1-2 SKT, 3 Year Subs Add-On

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

如果只有下面的，没有 ProSupport for Software Smart Management

Red Hat Enterprise Linux Smart Management, 1-2 SKT, 3 Year Subs Add-On

它只是的 license 需要有那个PS4SW的smart management，跟HA一样

 

 

已使用 OneNote 创建。
