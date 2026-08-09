答复: S\\T:1YQXF02 M620 NIC vmware drivers issue

Friday, June 27, 2014

1:55 PM

- ::: 
    -------------------------------------- ------------------------------------------------------------------------------
    主题       答复: S\\T:1YQXF02 M620 NIC vmware drivers issue
    发件人     Yin, Guoxun
    收件人     Chen, Jianliang
    抄送       CN XMN TS Server Coach; Lv, Zhiwei
    发送时间   Friday, June 27, 2014 1:50 PM
    -------------------------------------- ------------------------------------------------------------------------------
  :::

   

  1.  为什么DELL官方没有针对VMWARE网卡驱动下载，而需要VMWARE提供

  因为VMware虚拟化环境中，物理网卡工作被设计于虚拟交换机的一个端口，工作在二层，驱动是vender另行开发经过VMware签名的，所以发布在VMware网站上。

   

  2.以后如果有新的补丁更新，是否更新后还会出现问题，或者是否有DELL定制的补丁更新，是否能够让VMWARE系统就加载DELL的驱动，而不是DELL自己定制光盘

         补丁的发布由VMware官方管理，Dell不对补丁做任何发布和整理工作。DELL定制光盘也只是把自己的设备的驱动加入其中以方便用户安装，如果不使用DELL的定制光盘，客户需要下载官方的光盘后自行定制光盘添加驱动，除此之外别无他法。

   

   

   

   

  Best Regards

   

  Yin Guo Xun

  Dell \| Enterprise Support Services

  Mail Address:[guoxun_yin@dell.com](http://guoxun_yin@dell.com)

  Certifications: VCP3/4/5 , CCA , HPUX_CSA

   

  How am I doing? Email my manager ([Wang, XingFang](mailto:Xing_Fang_Wang@Dell.com)) with any feedback.

   

  发件人: Lv, Zhiwei 

  发送时间: 2014年6月27日 13:43

  收件人: Yin, Guoxun

  抄送: Chen, Jianliang; CN XMN TS Server Coach

  主题: 答复: S\\T:1YQXF02 M620 NIC vmware drivers issue

   

  Dell - Internal Use - Confidential 

  Guoxun

   

  Please help on this case

   

  Thanks!

   

   

   

   

  Regards

   

  Zhiwei Lv

  Enterprise Product Engineer

   

   

  \-\-\-\--邮件原件\-\-\-\--

  发件人: [No_reply@dell.com](mailto:No_reply@dell.com) \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)[\]]

  发送时间: 2014年6月27日 11:55

  收件人: CN XMN TS Server Escalation

  抄送: Chen, Jianliang; Lv, Zhiwei

  主题: S\\T:1YQXF02 M620 NIC vmware drivers issue

   

  1.客户的问题目前已经解决，由于broadcom的网卡，使用虚拟网卡的时候，机器会出现宕机的情况，VMWARE的有出针对这个问题补丁更新，客户更新后，导致网卡识别不到，客户后续使用之前的DELL的 IMG光盘进行更新操作，现在网卡已经又重新认到，机器已经正常

  2.客户提到两个问题，需要DELL給出解釋

  1.为什么DELL官方没有针对VMWARE网卡驱动下载，而需要VMWARE提供

  2.以后如果有新的补丁更新，是否更新后还会出现问题，或者是否有DELL定制的补丁更新，是否能够让VMWARE系统就加载DELL的驱动，而不是DELL自己定制光盘

 

已使用 OneNote 创建。
