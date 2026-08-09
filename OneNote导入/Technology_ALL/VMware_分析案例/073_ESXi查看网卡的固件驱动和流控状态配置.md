ESXi查看网卡的固件驱动和流控状态配置

2018年3月23日

14:35

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       [FW: \[Tech Shared\]ESXi]查看网卡的固件驱动和流控状态配置
  发件人     Yin, Guoxun
  收件人     CN XMN TS ENT L2 SME
  抄送       Wang3, Mark; Lian, Wenxiang
  发送时间   2018年3月23日 14:10
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Forward to L2 team,

 

Thanks Wen Xiang!

 

From: Lian, Wenxiang

Sent: Friday, March 23, 2018 2:07 PM

To: CCC_RS_DE \<CCC_RS_DE@Dell.com\>

Cc: Yin, Guoxun \<guoxun_Yin@Dell.com\>

Subject:[ \[Tech Shared\]ESXi]查看网卡的固件驱动和流控状态配置

 

Dell - Internal Use - Confidential 

Team,

 

目前新的网卡BCM5741X和Intel X550无法使用ethtool工具进行流控更改，大家遇到后请使用下面的方法进行验证和配置。

 

esxcfg-nics --l          查看网络驱动名称

![[Technology_ALL_VMware_分析案例_073_ESXi查看网卡的固件驱动和流控状态配置_001.png]]

 

esxcli network nic get --n vmnic0      查看网卡的固件驱动和流控状态

![[Technology_ALL_VMware_分析案例_073_ESXi查看网卡的固件驱动和流控状态配置_002.png]]

esxcli network nic pauseParams set --n vmnic4 --a 0                 

esxcli network nic pauseParams set --n vmnic4 --t 1

esxcli network nic pauseParams set --n vmnic4 --r 1

0代表disable，1代表enable

![[Technology_ALL_VMware_分析案例_073_ESXi查看网卡的固件驱动和流控状态配置_003.png]]

 

 

esxcli system module parameters list --module tg3查看网卡模块参数

![[Technology_ALL_VMware_分析案例_073_ESXi查看网卡的固件驱动和流控状态配置_004.png]]

 

 

 

Thanks,

 

Best Regards

 

Lian, Wenxiang \| 连文祥

Deployment Senior Engineer

APJ ProDeploy \| Remote Installation Services (RIS)

Dell EMC \| Infrastructure Delivery Services, Great China

[![[Technology_ALL_VMware_分析案例_073_ESXi查看网卡的固件驱动和流控状态配置_005.jpg]]](http://www.dell.com/prodeploy)

 

[![[Technology_ALL_VMware_分析案例_073_ESXi查看网卡的固件驱动和流控状态配置_006.jpg]]](http://www.dell.com/certification)

 

 

 

已使用 OneNote 创建。
