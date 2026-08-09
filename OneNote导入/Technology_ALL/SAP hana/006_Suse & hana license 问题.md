Suse & hana license 问题

2017年12月18日

13:47

Suse license：

 

Q1 .如果客户买了SUSE的订阅，是否会有个license给到客户，已邮件的方式，然后客户使用这个license到SUSE官网注册然，再加到系统里使用吗？

 

是的， 基本上是这个流程。 

用户需要在 [https://scc.suse.com](https://scc.suse.com) 上注册账户，然后把license加到该账户下就可以。

在SCC里，客户可以查看购买的产品类型，下载补丁，和创建serivce ticket.

如果客户系统可以链接外网，把license加入系统后，就可以直接更新软件包了。如果不能，就没有必要

 

 

Q2.那一个license一般能用多少台机器？是否可以查到那个license被那个机器激活过了？

 

这个需要联系一下销售，这个是我们厦门的销售，专门负责Dell的

 

Candy : 13860482417

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

Hana license：

 

Q1. hana的license是以形式给到客户的

 

客户需要有个SAP的账号，这个需要到SAP那边注册，然后用他们的帐号登录去generate（生成）出来，在生成licence之前客户要先填他们的SID, HW ID信息。

 

Q2.那也就是客户需要用他们的SAP账号登录到SAP网站，就能查到license信息。

对

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

OEM hana[  order]：890522319 wt[  \| ]483096867 china

SLES for SAP, x86-64, 1-2 Sockets or 1-2 VMs, L3-Priority Subscription, 1 Year, Customer Kit

这个order是OEM SLES for SAP, 而SLES for SAP本身就自带HA, 不需要额外买subscription.

普通SLES不带HA, 需额外买. SLES for SAP比较贵, 自身带了HA

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\[Rack\] SAP HANA SUSE Physical R940 / R740xd / R840-BMBW12

 

这个 physical 是说明hana是在工厂的时候就定义好的，比如有多少个node，多大的空间，数据什么，已经定好了。无法再改变，比如磁盘空间大小什么的已经没办法改变了。

 

 

 

SLES for SAP,x86-64, 1-2 Sockets or 1-2 VMs, L3-Priority Sub,1YR,CUS,PS4SW Service not included

sales 没卖PS4SW呀，就没有HA support呀

 

已使用 OneNote 创建。
