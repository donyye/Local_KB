A nutanix Case Study

Wednesday, December 21, 2016

1:44 PM

  -------------------------------------- --------------------------------------------------
  主题       A nutanix Case Study
  发件人     Yin, Guoxun
  收件人     CN XMN TS ENT L2 SME
  抄送       Ye, Dony; Wang, Xing Fang
  发送时间   Wednesday, December 21, 2016 1:37 PM
  -------------------------------------- --------------------------------------------------

 

Team,

下面是来自于售前SC同事的关于Nutanix 的比较重要的事情，建议大家阅读了解下其中标记的部分，有个印象。

 

 

 

 

Hi, team,

 

关于XC的EDT 安装，下面这样的问题已经多次出现了。

最近的就有广发证券的安装，也是同样的问题。

 

目前，我们XC工厂预安装的image只有2种：1，hpyer-V  2，KVM+ESXi

只要不是hpeyer-V，客户收到的都是灌装了KVM+ESXI的XC机器。

我们在安装部署的时候，要按照客户的意愿，选择相应的hypervisor进行部署。

PG comments as below:

Sku# 619-AHSV is the correct for ESXi & KVM, probably quotation was quote prior the changes.

Please refresh/create fresh new quote to capture the latest description.

 

ESXI 5.5, ESXI 6.0, MS Vol, and KVM will all share the same SW mod. 

The deployment team will only deploy the one SW version that the customer wants but it will all be under one mod in the factory

 

619-AHSV

Nutanix OS for KVM/ESX/MS Vol, factory installed   

 

 

但有的EDT工程师到现场后，发现是KVM，客户要ESXI就不知道怎么处理，甚至认为销售做错报价，或者工厂灌装的时候出错。

停止安装，升级case\~\~ 已经不是1,次2次了。

虽然，最终通过解释说明，都解决了。但给客户的影响就是，我们的服务太不专业了。

 

所以，拜托各位，能不能组织下内部的学习或讨论。将XC的安装部署的问题分享下经验。指导下大家应该怎么做。

避免同样的问题，一次次来。

 

另外，请TS的同事，遇到安装时升级上来的case，也请帮忙说明下。

我们的KVM和ESXI出厂时，是在一起给客户的。客户要请EDT工程师部署相应的hypervisor。

不影响未来的售后服务。

只是我们只负责XC的硬件和Nutanix软件的服务。ESXi的找VMWare买license并获得服务。Hyper-V的是找微软。如果买的是OEM的，也是DELL负责支持。

 

谢谢！

 

 

 

已使用 OneNote 创建。
