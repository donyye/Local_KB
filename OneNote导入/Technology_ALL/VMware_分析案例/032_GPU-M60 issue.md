GPU-M60 issue

Friday, August 05, 2016

9:36 AM

- ::: 
    -------------------------------------- -----------------------------------------------------------------------------------------------------------------------
    主题       RE: R730\|Product query\|PSP\|SR:933685801
    发件人     Zheng, Major
    收件人     Yin, Guoxun
    抄送       CN XMN TS ENT L2 SME; Lin, Yongliang
    发送时间   Friday, August 05, 2016 9:32 AM
    附件       \<\<转发 VMWARE 对GPU支持的问题、VMware 的vGPU 虚拟化只兼容到了NVIDIA 的GRID K1K2，Tesla M6M60，不支持CUDA 。.msg\>\>
    -------------------------------------- -----------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

  Hi   Guoxun

   

  1、具体报错有让客户提供，但是目前客户只知道VMware给出的判断是显卡有问题，以及VMware开的case号，我会后续沟通尽量拿到；

  2、找的驱动确实是在[http://www.dell.com/support/home/us/en/19/Drivers/DriversDetails?driverId=917X9](http://www.dell.com/support/home/us/en/19/Drivers/DriversDetails?driverId=917X9)这里的没错，升级时log中也已经提到：查询官网，在nVidia VMware ESXi 6 VIB driver的支持列表中没有M60；

  3、关于vGPU的支持，附件是之前查到的share邮件，看起来M60的卡是不支持vGPU的，如果客户业务需求得使用vGPU，可能无法支持；

   

  From: Yin, Guoxun

  Sent: Friday, August 05, 2016 9:18 AM

  To: Zheng, Major \<Major_Zheng@Dell.com\>

  Cc: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>; Lin, Yongliang \<Yongliang_Lin@Dell.com\>

  Subject: RE: R730\|Product query\|PSP\|SR:933685801

   

  如果说有报错，最好提供说报了什么错，不然说报错等于白说

  根据下面的描述，客户需要的不是"直通"模式， 直通GPU到VM中只支持一台VM，不支持直通到多台VM中。客户需要的是VGPU模式，

  需要装的驱动是下面这个

  ![[Technology_ALL_VMware_分析案例_032_GPU-M60 issue_001.jpg]]

   

  问题是按照上面链接点击进去，你会惊奇的发现，英伟达根本就没提供ESXi 6.X的驱动下载链接，最新只有5.5的

                 

  ![[Technology_ALL_VMware_分析案例_032_GPU-M60 issue_002.png]]

   

  回来看DELL的nvidia Driver，我猜测你用的是下面这个

  [http://www.dell.com/support/home/us/en/19/Drivers/DriversDetails?driverId=917X9](http://www.dell.com/support/home/us/en/19/Drivers/DriversDetails?driverId=917X9)

   

  那么你是否注意过下面这个介绍呢？这个可有M60的影子？

  ![[Technology_ALL_VMware_分析案例_032_GPU-M60 issue_003.jpg]]

   

  所以我的建议是：

  1.  不要考虑所谓的"直通" ，要在ESXi上装VGPU Driver，然后by VM的去设置VGPU共享模式，
  2.  请客户向VMWARE咨询下，为什么vmware网站没有把nvidia的驱动放上去，为什么英伟达明明没提供驱动下载，还在HCL网站上提供跳转？VMware是否能够提供驱动

   

   

  另外我会试着找下内部渠道看有没有办法拿到驱动，请等待update

   

  From: Zheng, Major

  Sent: 2016年8月5日 8:48

  To: Yin, Guoxun \<<guoxun_Yin@Dell.com>\>

  Cc: CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>; Lin, Yongliang \<<Yongliang_Lin@Dell.com>\>

  Subject: RE: R730\|Product query\|PSP\|SR:933685801

   

  Dell - Internal Use - Confidential 

  Hi   Guoxun

   

  1.该GPU是否要给VM使用：是的

  2.计划要给多少VM使用：至少10台

  3.该GPU要给什么应用软件使用：水表识别程序，对显卡有要求

  4.用户期望的GPU的工作模式或者工作方式的简述：VM通过直通模式使用ESXI服务器上的显卡。

   

  客户表示已经联系了Vmware开了case，已经将戴尔官网的驱动装上，并确认打开显卡直通模式成功；

  但是客户尝试开了一台虚拟机，虚机开机就报错，将直通模式拉掉，显卡配置改回原来的配置，就能正常；

  收了系统日志给Vmware分析，VMware工程师目前判断是显卡问题，建议Dell协助检查；

   

  VNware Support Request 16202194408

   

  建议客户导出TSR日志，目前还没收集到。

   

  From: Yin, Guoxun

  Sent: Wednesday, August 03, 2016 4:36 PM

  To: Lin, Yongliang \<<Yongliang_Lin@Dell.com>\>; Zheng, Major \<<Major_Zheng@Dell.com>\>

  Cc: CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>

  Subject: RE: R730\|Product query\|PSP\|SR:933685801

   

  Major,

  M60被ESXi 6.X支持，所以直通不是问题， 但是直通不是唯一的用法，所以问题在于客户他想达到什么样的应用效果，这是必须弄清楚，请用户提供以下问题的具体回答，之后我们才可以给予相关答复。

  另外请注意检查是否为OEM software，不然我们只提供硬件相关的支持。

   

  需要明确的问题：

  1.  该GPU是否要给VM使用？
  2.  计划要给多少VM使用？
  3.  该GPU要给什么应用软件使用？
  4.  用户期望的GPU的工作模式或者工作方式的简述

   

   

  From: Lin, Yongliang

  Sent: 2016年8月3日 16:15

  To: Zheng, Major \<<Major_Zheng@Dell.com>\>; Yin, Guoxun \<<guoxun_Yin@Dell.com>\>

  Cc: CN XMN TS ENT L2 SME \<<CN_XMN_TS_ENT_L2_SME@Dell.com>\>

  Subject: 答复: R730\|Product query\|PSP\|SR:933685801

   

  Dell - Internal Use - Confidential 

  Hi guoxun:

   

  Help check it at first .

  Thanks & Best Regards.

  Yongliang Lin

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

   

   

  \-\-\-\--邮件原件\-\-\-\--

  发件人: Zheng, Major 

  发送时间: 2016年8月3日 16:02

  抄送: CN XMN TS Server Escalation ; Zheng, Major 

  主题: R730\|Product query\|PSP\|SR:933685801

   

  a\. Detail Symptom Descriptions

   

  R730，装ESXi 6.0，客户买了NVIDIA Tesla M60 GPU，找我们要GPU驱动安装，然后在ESXI 6.0下开启M60显卡直通功能，但是目前对于M60是否能支持显卡直通以及怎么在ESXi下安装驱动无法确认，需要L2协助。

   

  b\. Troubleshooting Steps

   

  1、R730机器，ESXi 6.0，客户要加速显卡性能，咨询了VMware，客户配置GPU之后启用显卡直通功能；

  2、客户采购了Tesla M60的卡，装了OEM的ESXi 6.0，没有GPU，来电咨询，查询资料告知客户VMware 的vGPU 虚拟化只兼容到了NVIDIA 的GRID K1&K2，对于Tesla M6&M60，不支持CUDA的加速特性；

  3、客户又来电反馈Sales这边告知可以支持显卡直通模式；

  4、Sales邮件提供文档链接：[http://www.vmware.com/resources/compatibility/pdf/vi_sptg_guide.pdf，从文档Shared](http://www.vmware.com/resources/compatibility/pdf/vi_sptg_guide.pdf，从文档Shared) Pass-Through Graphics Guide来看，Tesla M6&M60是可以支持显卡直通的，但是固件要求84.04.85.00.xx；

  5、查询CUDA和显卡直通模式，貌似不是一回事，需要L2协助确认；

  6、查询官网，在nVidia VMware ESXi 6 VIB driver的支持列表中没有M60，支持的GPU列表：

  K1、K10、K2、K20、K20C、K20X、NVIDIA GRID K2 Active、

  nVIDIA Quadro K2000、nVIDIA Quadro K4000、

  NVIDIA Tesla K40c、NVIDIA Tesla K40M、NVIDIA Tesla K80；

  7、需要升级L2确认M60的卡对显卡直通模式的支持情况，以及如果能支持，客户不懂得安装GPU驱动，也需要协助；

 

已使用 OneNote 创建。
