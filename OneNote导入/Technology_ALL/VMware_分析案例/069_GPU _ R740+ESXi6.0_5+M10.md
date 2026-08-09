GPU : R740+ESXi6.0/5+M10

2017年9月13日

14:34

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       FW: Case Share : R740+ESXi6.0/5+M10
  发件人     Wang, Xing Fang
  收件人     CN XMN TS ENT L2 SME; CN XMN TS ENT L2 Coach; APJ Ent Resolution Managers China
  发送时间   2017年9月13日 14:18
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

From: Song, Shubao

Sent: Wednesday, September 13, 2017 2:16 PM

To: CCC Ent PRO MGMT \<CCC_Ent_PRO_MGMT@Dell.com\>

Cc: Zhu, Hill \<Hill_Zhu@Dell.com\>; Zheng, Xin \<Xin_Zheng@Dell.com\>; Xie, YuXuan \<YuXuan_Xie@Dell.com\>

Subject: Case Share : R740+ESXi6.0/5+M10

 

Dell - Internal Use - Confidential 

Dear ,All

Share with your team ,Thanks J

Case 硬件软件环境：R740安装NVIDIA M10的GPU 卡，ESX6.0U2/3 or ESXi6.5 U1

问题描述：客户安装系统以及卡的驱动后，无法使用该卡，查询模块加载，发现报错：NVRM: to you NVIDIA device is not supported by the kernel  ; PCI passthrough was not enabled : passthruEnabled=false                                           并且通过命令nvidia-smi查看后有如下报错：\[[root@localhost:\~\] nvidia-smi        ]Failed to initialize NVML: Unknown Error   该报错对于解决问题起到决定性作用

由于服务器，卡，ESX 都是新产品组合，客户之前在R730+M60没有遇到问题，驱动也已更新，硬件没有任何报错，在VMware /VNIDIA 的官方测试列表均为发布最新的硬件测试列表，客户当天在西安出差，要在晚上赶回上海，时间紧急，最终选择找NVIDIA寻求帮助.

工程师根据客户描述的Unknown Error   ，说有一个超微服务器使用GPU 时遇到类似问题，NVIDIA 官方有KB ，客户按照类似的设置更改DELL R740 的设置后问题得以解决。

KB 如下：[http://nvidia.custhelp.com/app/answers/detail/a_id/4119/\~/incorrect-bios-settings-on-a-server-when-used-with-a-hypervisor-can-cause-mmio](http://nvidia.custhelp.com/app/answers/detail/a_id/4119/~/incorrect-bios-settings-on-a-server-when-used-with-a-hypervisor-can-cause-mmio)

客户最终更改BIOS 后的截图如图所示，具体哪一项客户没有描述，本身这图也是让客户回到公司后截取的，没再好意思麻烦客户了J,TS 的同事可对比我们默认设置参考。

 

![[Technology_ALL_VMware_分析案例_069_GPU _ R740+ESXi6.0_5+M10_001.jpg]]

 

Song, Shubao 宋述宝

Technology Service Manager 

Dell \| Global Support and Deployment

Office:0592-8185253 

MP:86-18524112978

Email: <shubao.song@dell.com>

How am I doing? Please contact my manager <Hill_Zhu@Dell.com>

 

 

已使用 OneNote 创建。
