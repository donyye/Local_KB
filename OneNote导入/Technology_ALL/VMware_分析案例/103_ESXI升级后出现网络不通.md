ESXI升级后出现网络不通

2019年6月27日

12:36

  ------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   RE: 答复: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\|[    \ ref:\_00D60IUGi.\_500[    \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]
  From      Yin, Guoxun
  To        Leo Tsai-蔡政宏-精誠軟體-業務二處F; Li1, Yuan
  Cc        Ye, Dony
  Sent      2019年6月27日 12:30
  ------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell Customer Communication - Confidential

 

Hi  Leo,

我检查了ESXi650-201808001那个补丁包，发现里面带的ixgbe驱动版本确实非常旧，升级的时候该旧版本驱动会覆盖之前系统存在的4.5.1的驱动包，

这样会在重启后无法加载ixgbe 从而让网络失去链接，

我们有两个办法解决这个问题：

1：通过VMware发布的工具包image builder CLI将4.5.1的驱动包嵌入到ESXi650-201808001补丁包中，之后拿新生成的补丁包更新ESXi，这样更新完就应该不会遇到断网问题。

                方法如下链接中的示例，只是最后要注意是生成zip包不是ISO包，那个命令要注意下

<https://vmguru.com/2015/04/how-to-build-a-custom-image-with-vsphere-esxi-image-builder-cli/>

 

2：像昨天我在远程中做的，在更新完成后手动的去上传补丁包，然后进行更新，步骤稍繁琐一点，

 

 

![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_001.png]]

 

From: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<leotsai@systexsoftware.com.tw\> 

Sent: 2019年6月27日 8:16

To: Yin, Guoxun; Li1, Yuan

Cc: Ye, Dony

Subject: Re: 答复[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

\[EXTERNAL EMAIL\]

Dear  Gunxun

User這邊希望明天可以將剩下三台已及其他cluster 八台都執行完更新（一樣規格）

請問有什麼建議嗎？？或是做法嗎？

 

 

例如 先執行驅動更新在執行esx更新等等

 

 

取得 [iOS 版 Outlook](https://aka.ms/o0ukef)

 

寄件者: [Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com) \<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>

寄件日期: Wednesday, June 26, 2019 5:55:59 PM

收件者: Leo Tsai-蔡政宏-精誠軟體-業務二處F; [Yuan.Li1@dell.com](mailto:Yuan.Li1@dell.com)

副本: [Dony.Ye@dell.com](mailto:Dony.Ye@dell.com)

主旨: RE: 答复[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \] ]

 

Dell Customer Communication - Confidential

 

Hi Leo,

经过远程检查，我们发现升级后的Node1ESXi主机vswitch0中的没有uplink，然后检查发现ixgbe驱动模块版本较旧且没有加载，

推测升级包中没有该ixgbe驱动模块或者ixgbe驱动模块较旧，导致该问题。

后续我们参考其他主机的ixgbe驱动模块版本，针对Node1使用了同样版本的驱动包更新并重启后，该ESXi主机恢复到工作状态，

目前NODE1主机上的x550网卡的驱动与其他主机版本一致，请知悉。

 

 

BR.

Guoxun.

From: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\> 

Sent: 2019年6月26日 16:53

To: Li1, Yuan

Cc: Ye, Dony; Yin, Guoxun

Subject: RE: 答复[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

\[EXTERNAL EMAIL\]

我剛剛有請GUN XUN直接在該台電腦下載。

 

From: <Yuan.Li1@dell.com> \<<Yuan.Li1@dell.com>\>

Sent: Wednesday, June 26, 2019 4:51 PM

To: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>

Cc: <Dony.Ye@dell.com>; <Guoxun.Yin@dell.com>

Subject: 答复: 答复[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

Dell Customer Communication - Confidential

 

Dear Leo，

煩請下載並安裝如下驅動，謝謝.

<https://my.vmware.com/web/vmware/details?downloadGroup=DT-ESXI60-INTEL-IXGBE-451&productId=491>

 

 

 

 

 

 

 

Yuan Li\|李源

Tech Support Analyst, Great China Customer Support Services

Dell EMC \| Support and Deployment Services

[Yuan_Li1@dell.com](mailto:Yuan_Li1@dell.com)

 

如果您对我的服务有任何意见或建议,也可以联系我的经理[Mars_Zeng@dell.com](mailto:Mars_Zeng@dell.com)

使用快速服務代碼, 直达售後團隊。[获取及了解快速服务代码信息请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)

[![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_002.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Email/TagChange)

 

[![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_003.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Chat/NonConsumerForm?svcTag=8JQMKY1&segment=BSD&chatQueueName=CN.ZH.TS.ENT.CCC&chatQueueClient=UNIFIEDCLIENT&chatQueueMaxWaitTime=1000000&chatQueueUnifiedChatClientTemp=Dell&chatQueueLanguageId=18&productModel=PModel&IsOLIMTag=False)

 

[![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_004.jpg]]](http://www.dell.com/support/article/cn/zh/19/SLN298781)

 

发件人: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\> 

发送时间: 2019年6月26日 15:35

收件人: Li1, Yuan

抄送: Ye, Dony

主题: RE: 答复[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

\[EXTERNAL EMAIL\]

Dear

 

[http://maxlabvm.blogspot.com/2018/05/how-to-install-latest-esxi-65-update-2.html](http://maxlabvm.blogspot.com/2018/05/how-to-install-latest-esxi-65-update-2.html)

我事參考這邊進行操作的

 

另外附上IDRAC登入上的問題

![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_005.png]]

 

From: <Yuan.Li1@dell.com> \<<Yuan.Li1@dell.com>\>

Sent: Wednesday, June 26, 2019 1:45 PM

To: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>

Cc: <Dony.Ye@dell.com>

Subject: 答复: 答复[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

Hi Leo,

我們VM support engineer將會協助您分析VM部分的內容.

目前vCenter識別不到ESXi，有出現什麼提示麼，若有煩請截圖回復，謝謝.

 

 

 

 

 

 

 

Yuan Li\|李源

Tech Support Analyst, Great China Customer Support Services

Dell EMC \| Support and Deployment Services

[Yuan_Li1@dell.com](mailto:Yuan_Li1@dell.com)

 

如果您对我的服务有任何意见或建议,也可以联系我的经理[Mars_Zeng@dell.com](mailto:Mars_Zeng@dell.com)

使用快速服務代碼, 直达售後團隊。[获取及了解快速服务代码信息请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)

[![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_002.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Email/TagChange)

 

[![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_003.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Chat/NonConsumerForm?svcTag=8JQMKY1&segment=BSD&chatQueueName=CN.ZH.TS.ENT.CCC&chatQueueClient=UNIFIEDCLIENT&chatQueueMaxWaitTime=1000000&chatQueueUnifiedChatClientTemp=Dell&chatQueueLanguageId=18&productModel=PModel&IsOLIMTag=False)

 

[![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_004.jpg]]](http://www.dell.com/support/article/cn/zh/19/SLN298781)

 

发件人: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\> 

发送时间: 2019年6月26日 13:36

收件人: Li1, Yuan

主题: RE: 答复[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

\[EXTERNAL EMAIL\]

HI 李源

請問vmware的部份有什麼回硬了嗎?

 

From: <Yuan.Li1@dell.com> \<<Yuan.Li1@dell.com>\>

Sent: Wednesday, June 26, 2019 11:57 AM

To: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>

Subject: 答复: 答复[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

Hi  Leo，

已收到，我將提交，謝謝.

 

 

 

 

 

 

Yuan Li\|李源

Tech Support Analyst, Great China Customer Support Services

Dell EMC \| Support and Deployment Services

[Yuan_Li1@dell.com](mailto:Yuan_Li1@dell.com)

 

如果您对我的服务有任何意见或建议,也可以联系我的经理[Mars_Zeng@dell.com](mailto:Mars_Zeng@dell.com)

使用快速服務代碼, 直达售後團隊。[获取及了解快速服务代码信息请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)

[![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_002.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Email/TagChange)

 

[![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_003.jpg]]](http://www.dell.com/support/incidents/cn/zh/cnbsd1/Chat/NonConsumerForm?svcTag=8JQMKY1&segment=BSD&chatQueueName=CN.ZH.TS.ENT.CCC&chatQueueClient=UNIFIEDCLIENT&chatQueueMaxWaitTime=1000000&chatQueueUnifiedChatClientTemp=Dell&chatQueueLanguageId=18&productModel=PModel&IsOLIMTag=False)

 

[![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_004.jpg]]](http://www.dell.com/support/article/cn/zh/19/SLN298781)

 

发件人: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\> 

发送时间: 2019年6月26日 11:52

收件人: Li1, Yuan

主题: RE: 答复[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

\[EXTERNAL EMAIL\]

Dear

請參考

![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_006.png]]

 

From: <Yuan.Li1@dell.com> \<<Yuan.Li1@dell.com>\>

Sent: Wednesday, June 26, 2019 11:37 AM

To: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>

Subject: 答复: 答复[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

Hi Leo，

通過ESXi確認license的步驟如下，請參閱，謝謝.

![[Technology_ALL_VMware_分析案例_103_ESXI升级后出现网络不通_007.png]]

 

 

 

 

 

 

Yuan Li\|李源

Tech Support Analyst, Great China Customer Support Services

Dell EMC \| Support and Deployment Services

[Yuan_Li1@dell.com](mailto:Yuan_Li1@dell.com)

 

如果您對我的服務有任何意見或建議,也可以聯繫我的經理Mars_Zeng@dell.com

使用快速服務代碼, 直達售後團隊。獲取及瞭解快速服務代碼資訊請點此連結

 

 

\-\-\-\--郵件原件\-\-\-\--

寄件者: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\> 

發送時間: 2019年6月26日 10:36

收件人: Li1, Yuan

主題: RE: 答覆[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

 

\[EXTERNAL EMAIL\]

 

Hi 李源

請問Nutanix工程師有與您聯繫嗎?

 

\-\-\-\--Original Message\-\-\-\--

From: <Yuan.Li1@dell.com> \<<Yuan.Li1@dell.com>\>

Sent: Tuesday, June 25, 2019 4:11 PM

To: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>

Subject: 答覆: 答覆[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

Hi Leo,

我已收到答復，將會在明早分配Chinese support，謝謝.

 

 

 

 

 

 

Yuan Li\|李源

Tech Support Analyst, Great China Customer Support Services Dell EMC \| Support and Deployment Services <Yuan_Li1@dell.com>

 

如果您對我的服務有任何意見或建議,也可以聯繫我的經理Mars_Zeng@dell.com

使用快速服務代碼, 直達售後團隊。獲取及瞭解快速服務代碼資訊請點此連結

 

 

\-\-\-\--郵件原件\-\-\-\--

寄件者: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>

發送時間: 2019年6月25日 15:20

收件人: Li1, Yuan

主題: RE: 答覆[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

 

\[EXTERNAL EMAIL\]

 

麻煩您了

謝謝

 

\-\-\-\--Original Message\-\-\-\--

From: <Yuan.Li1@dell.com> \<<Yuan.Li1@dell.com>\>

Sent: Tuesday, June 25, 2019 3:19 PM

To: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>

Subject: 答覆: 答覆[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

Hi Leo,

我將提交申請，稍後答復您，謝謝.

 

 

 

 

 

 

Yuan Li\|李源

Tech Support Analyst, Great China Customer Support Services Dell EMC \| Support and Deployment Services <Yuan_Li1@dell.com>

 

如果您對我的服務有任何意見或建議,也可以聯繫我的經理Mars_Zeng@dell.com

使用快速服務代碼, 直達售後團隊。獲取及瞭解快速服務代碼資訊請點此連結

 

 

\-\-\-\--郵件原件\-\-\-\--

寄件者: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>

發送時間: 2019年6月25日 15:15

收件人: Li1, Yuan; Li1, Yuan

主題: RE: 答覆[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

 

\[EXTERNAL EMAIL\]

 

Dear yuan

這裡可以請您幫我更換support嗎?

 

\-\-\-\--Original Message\-\-\-\--

From: Sourabh Adhikary \<<sourabh.adhikary@nutanix.com>\>

Sent: Tuesday, June 25, 2019 2:57 PM

To: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>; [yuan_li1@dell.com](mailto:yuan_li1@dell.com); [yuan.li1@dell.com](mailto:yuan.li1@dell.com)

Subject: Re: 答覆[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

 

Hello,

 

I am currently on another webex. Will it be possible for you to call in to the queue, to get the next engineer assigned on this?

 

Thank you,

Sourabh

 

﻿On 25/06/19, 12:24 PM, \"Leo Tsai-蔡政宏-精誠軟體-業務二處F\" \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\> wrote:

 

    I said that those settings are fine, I have checked them.

    And Idrac have not working it can

   

    I have no solution, what should I do, now the service is interrupted, the customer is anxious to fix

    \-\-\-\--Original Message\-\-\-\--

    From: Sourabh Adhikary \<<sourabh.adhikary@nutanix.com>\>

    Sent: Tuesday, June 25, 2019 2:43 PM

    To: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>; Support_Case \<[support_case@nutanix.com](mailto:support_case@nutanix.com)\>; [yuan_li1@dell.com](mailto:yuan_li1@dell.com); [yuan.li1@dell.com](mailto:yuan.li1@dell.com)

    Subject: Re: 答覆[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

   

    Hello,

   

    Since the node is not on network, can you check if IPs have been assigned correctly (along with VLAN)?

    If there is NIC teaming, disable one interface and see if it starts pinging. However, this still does not explain why IDRAC is not working.

   

    Thank you,

    Sourabh

   

    On 25/06/19, 12:04 PM, \"Leo Tsai-蔡政宏-精誠軟體-業務二處F\" \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\> wrote:

   

        his node has been boot completed

        Standby esxi screen (to press f2 to enter the account screen)

       

        \-\-\-\--Original Message\-\-\-\--

        From: Nutanix Support \<<support_case@nutanix.com>\>

        Sent: Tuesday, June 25, 2019 2:04 PM

        To: <yuan_li1@dell.com>; <yuan.li1@dell.com>; <sourabh.adhikary@nutanix.com>

        Cc: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>

        Subject: RE: 答覆[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ ref:\_00D60IUGi.\_500 \[ ref:\_00D60IUGi.\_5000e1ahZVI:ref \]]

       

        Hello,

        

        If you are unable to access the node using iDRAC, please take a crash cart to the node (monitor and keyboard), and see if the node is booting up. If it is not, it needs to be reimaged with ESXi and phoenixed.

       

        Thank you,

        Sourabh

       

        \-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original Message \-\-\-\-\-\-\-\-\-\-\-\-\-\--

        From:  \[yuan.li1@dell.com\]

        Sent: 6/25/2019 9:46 AM

        To: <support_case@nutanix.com>

        Cc: <sourabh.adhikary@nutanix.com>; <leotsai@systexsoftware.com.tw>

        Subject: 答覆[: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\|    \[ \]]

       

        Dear Nutanix team,

        The host is offline from the cluster.

        In manage web can't ping with the host.

        Pls help to solve with it asap, thanks.

       

        

        

        

        

        

        Yuan Li\|李源

        Tech Support Analyst, Great China Customer Support Services Dell EMC \| Support and Deployment Services [Yuan_Li1@dell.com\<mailto:Yuan_Li1@dell.com](mailto:Yuan_Li1@dell.com%3cmailto:Yuan_Li1@dell.com)\>

       

        如果您對我的服務有任何意見或建議,也可以聯繫我的經理Mars_Zeng@dell.com\<[mailto:Mars_Zeng@dell.com](mailto:Mars_Zeng@dell.com)\>

        使用快速服務代碼, 直達售後團隊。獲取及瞭解快速服務代碼資訊請點此連結\<[https://urldefense.proofpoint.com/v2/url?u=http-3A\_\_zh.community.dell.com_support-5Fforums_poweredge_f_251_t_2807&d=DwIGbw&c=s883GpUCOChKOHiocYtGcg&r=Vb_G-NyjEWHi-ESC7cC3g_8HKo9dQWOoPeqtvCBgpFQ&m=yllfOAPfSsED7UTgz9Ee9_vzOB5tCDpvHut_D7QFbH0&s=T6soNDnuKEKQI88yEY45CUuIZ2XqNDzH_JjRVTTIXIc&e= ](https://urldefense.proofpoint.com/v2/url?u=http-3A__zh.community.dell.com_support-5Fforums_poweredge_f_251_t_2807&d=DwIGbw&c=s883GpUCOChKOHiocYtGcg&r=Vb_G-NyjEWHi-ESC7cC3g_8HKo9dQWOoPeqtvCBgpFQ&m=yllfOAPfSsED7UTgz9Ee9_vzOB5tCDpvHut_D7QFbH0&s=T6soNDnuKEKQI88yEY45CUuIZ2XqNDzH_JjRVTTIXIc&e=%20)[\> \[email1\]\<][https://urldefense.proofpoint.com/v2/url?u=http-3A\_\_www.dell.com_support_incidents_cn_zh_cnbsd1_Email_TagChange&d=DwIGbw&c=s883GpUCOChKOHiocYtGcg&r=Vb_G-NyjEWHi-ESC7cC3g_8HKo9dQWOoPeqtvCBgpFQ&m=yllfOAPfSsED7UTgz9Ee9_vzOB5tCDpvHut_D7QFbH0&s=MqSDHVgothwRSqpsQkvdBbcVW9-Clb-vuCPZtfaBMfI&e= ](https://urldefense.proofpoint.com/v2/url?u=http-3A__www.dell.com_support_incidents_cn_zh_cnbsd1_Email_TagChange&d=DwIGbw&c=s883GpUCOChKOHiocYtGcg&r=Vb_G-NyjEWHi-ESC7cC3g_8HKo9dQWOoPeqtvCBgpFQ&m=yllfOAPfSsED7UTgz9Ee9_vzOB5tCDpvHut_D7QFbH0&s=MqSDHVgothwRSqpsQkvdBbcVW9-Clb-vuCPZtfaBMfI&e=%20)[\>\[chat1\]\<][https://urldefense.proofpoint.com/v2/url?u=http-3A\_\_www.dell.com_support_incidents_cn_zh_cnbsd1_Chat_NonConsumerForm-3FsvcTag-3D8JQMKY1-26segment-3DBSD-26chatQueueName-3DCN.ZH.TS.ENT.CCC-26chatQueueClient-3DUNIFIEDCLIENT-26chatQueueMaxWaitTime-3D1000000-26chatQueueUnifiedChatClientTemp-3DDell-26chatQueueLanguageId-3D18-26productModel-3DPModel-26IsOLIMTag-3DFalse&d=DwIGbw&c=s883GpUCOChKOHiocYtGcg&r=Vb_G-NyjEWHi-ESC7cC3g_8HKo9dQWOoPeqtvCBgpFQ&m=yllfOAPfSsED7UTgz9Ee9_vzOB5tCDpvHut_D7QFbH0&s=iDfxliB5lWctYQI2E_D8OH38ceg0egPXOoycqC74pac&e= ](https://urldefense.proofpoint.com/v2/url?u=http-3A__www.dell.com_support_incidents_cn_zh_cnbsd1_Chat_NonConsumerForm-3FsvcTag-3D8JQMKY1-26segment-3DBSD-26chatQueueName-3DCN.ZH.TS.ENT.CCC-26chatQueueClient-3DUNIFIEDCLIENT-26chatQueueMaxWaitTime-3D1000000-26chatQueueUnifiedChatClientTemp-3DDell-26chatQueueLanguageId-3D18-26productModel-3DPModel-26IsOLIMTag-3DFalse&d=DwIGbw&c=s883GpUCOChKOHiocYtGcg&r=Vb_G-NyjEWHi-ESC7cC3g_8HKo9dQWOoPeqtvCBgpFQ&m=yllfOAPfSsED7UTgz9Ee9_vzOB5tCDpvHut_D7QFbH0&s=iDfxliB5lWctYQI2E_D8OH38ceg0egPXOoycqC74pac&e=%20)\>\[問號[\]\<][https://urldefense.proofpoint.com/v2/url?u=http-3A\_\_www.dell.com_support_article_cn_zh_19_SLN298781&d=DwIGbw&c=s883GpUCOChKOHiocYtGcg&r=Vb_G-NyjEWHi-ESC7cC3g_8HKo9dQWOoPeqtvCBgpFQ&m=yllfOAPfSsED7UTgz9Ee9_vzOB5tCDpvHut_D7QFbH0&s=B12pCA70ThKiv3rD1csjrt5nXtHJi60B9eysvmMo5CI&e= ](https://urldefense.proofpoint.com/v2/url?u=http-3A__www.dell.com_support_article_cn_zh_19_SLN298781&d=DwIGbw&c=s883GpUCOChKOHiocYtGcg&r=Vb_G-NyjEWHi-ESC7cC3g_8HKo9dQWOoPeqtvCBgpFQ&m=yllfOAPfSsED7UTgz9Ee9_vzOB5tCDpvHut_D7QFbH0&s=B12pCA70ThKiv3rD1csjrt5nXtHJi60B9eysvmMo5CI&e=%20)\>

       

        寄件者: Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw)\>

        發送時間: 2019年6月25日 12:09

        收件人: Nutanix Support; Li1, Yuan

        抄送: [sourabh.adhikary@nutanix.com](mailto:sourabh.adhikary@nutanix.com)

        主題[: Re: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ \]]

       

        

        \[EXTERNAL EMAIL\]

        可以的話希望藉由遠端協助處理

       

        取得 iOS 版 Outlook\<[https://urldefense.proofpoint.com/v2/url?u=https-3A\_\_aka.ms_o0ukef&d=DwIGbw&c=s883GpUCOChKOHiocYtGcg&r=Vb_G-NyjEWHi-ESC7cC3g_8HKo9dQWOoPeqtvCBgpFQ&m=yllfOAPfSsED7UTgz9Ee9_vzOB5tCDpvHut_D7QFbH0&s=LytRul_GrXjRuKHwUgZO78WTmRktVOesI9RvREQoL_w&e= ](https://urldefense.proofpoint.com/v2/url?u=https-3A__aka.ms_o0ukef&d=DwIGbw&c=s883GpUCOChKOHiocYtGcg&r=Vb_G-NyjEWHi-ESC7cC3g_8HKo9dQWOoPeqtvCBgpFQ&m=yllfOAPfSsED7UTgz9Ee9_vzOB5tCDpvHut_D7QFbH0&s=LytRul_GrXjRuKHwUgZO78WTmRktVOesI9RvREQoL_w&e=%20)\> \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

        寄件者: Leo Tsai-蔡政宏-精誠軟體-業務二處F

        寄件日期: Tuesday, June 25, 2019 12:06:51 PM

        收件者: Nutanix Support; [yuan_li1@dell.com\<mailto:yuan_li1@dell.com](mailto:yuan_li1@dell.com%3cmailto:yuan_li1@dell.com)\>

        副本: [sourabh.adhikary@nutanix.com\<mailto:sourabh.adhikary@nutanix.com](mailto:sourabh.adhikary@nutanix.com%3cmailto:sourabh.adhikary@nutanix.com)\>

        主旨[: Re: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ \]]

       

        Dear yuan

        可以麻煩您協助溝通嗎？？

        這台主機已經完全脫離cluster了

        Manage連不上 ping不通

        Idrac設定都是正常的事沒跑掉

       

        

        \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

        寄件者: Leo Tsai-蔡政宏-精誠軟體-業務二處F

        寄件日期: Tuesday, June 25, 2019 10:13:58 AM

        收件者: Nutanix Support; [yuan_li1@dell.com\<mailto:yuan_li1@dell.com](mailto:yuan_li1@dell.com%3cmailto:yuan_li1@dell.com)\>

        副本: [sourabh.adhikary@nutanix.com\<mailto:sourabh.adhikary@nutanix.com](mailto:sourabh.adhikary@nutanix.com%3cmailto:sourabh.adhikary@nutanix.com)\>

        主旨[: RE: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ \]]

       

        Hardware settings are fine, I checked it.

        Now it is completely out of contact with the host manage and it can\'t be pinged.

       

        

        

        \-\-\-\--Original Message\-\-\-\--

        From: Nutanix Support \<[support_case@nutanix.com\<mailto:support_case@nutanix.com](mailto:support_case@nutanix.com%3cmailto:support_case@nutanix.com)\>\>

        Sent: Monday, June 24, 2019 6:05 PM

        To: [yuan_li1@dell.com\<mailto:yuan_li1@dell.com](mailto:yuan_li1@dell.com%3cmailto:yuan_li1@dell.com)\>; Leo Tsai-蔡政宏-精誠軟體-業務二處F \<[leotsai@systexsoftware.com.tw\<mailto:leotsai@systexsoftware.com.tw](mailto:leotsai@systexsoftware.com.tw%3cmailto:leotsai@systexsoftware.com.tw)\>\>

        Cc: [sourabh.adhikary@nutanix.com\<mailto:sourabh.adhikary@nutanix.com](mailto:sourabh.adhikary@nutanix.com%3cmailto:sourabh.adhikary@nutanix.com)\>

        Subject: NTNX Case \# 00584457 \| P3 - Normal \| XC740xd\|OS issue\|Nutanix\|PROS\| \[ \]

       

        Hello yuan,

       

        This is Sourabh Adhikary from Nutanix Worldwide Support team. I have taken ownership of your case \# 00584457  and will be working with you to resolve the issue.

       

        Problem Description:

        ====

        XC740xd\|OS issue\|Nutanix\|PROS\|

       

        

        Severity:

        ======

        P3 - Normal

       

        

        Action plan:

        =========

        Please check using iDRAC if the host has booted up correctly or not.

        If the host has not booted, or the upgrade failed, retry the upgrade on it.

       

        Should you have any questions, please don\'t hesitate to contact me. Thanks for your co-operation and I look forward to working with you.

       

        Please note: My working hours are shown in my signature at the bottom of this email - if you need urgent assistance outside of my business hours you may wish to call Nutanix Support (<http://www.nutanix.com/support/phone-numbers/>), quoting your case number for immediate assistance.

       

        Regards,

       

        Sourabh Adhikary

        System Reliability Engineer \| Nutanix \| 1-408.686.4929 x 1566 \| [sourabh.adhikary@nutanix.com\<mailto:sourabh.adhikary@nutanix.com](mailto:sourabh.adhikary@nutanix.com%3cmailto:sourabh.adhikary@nutanix.com)\> Business Hours:  Mon - Fri 12:00 - 20:00 GMT +5.30 Customer Support Portal: <http://portal.nutanix.com/> Support Phone Number: <https://www.nutanix.com/support-services/product-support/support-phone-numbers/>

        ref:\_00D60IUGi.\_5000e1ahZVI:ref

       

    

    

 

 

已使用 OneNote 创建。
