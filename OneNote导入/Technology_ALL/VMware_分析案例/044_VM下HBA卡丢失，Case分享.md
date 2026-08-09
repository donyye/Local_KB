VM下HBA卡丢失，Case分享

Tuesday, January 17, 2017

2:29 PM

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       VM下HBA卡丢失，Case分享
  发件人     Yu, Zhouming
  收件人     APJ Ent Resolution Managers China; CN XMN TS ENT L2 SME
  抄送       Yin, Guoxun
  发送时间   Tuesday, January 17, 2017 2:27 PM
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

各位：

 

历时2个月，VM方在升级L3和研发后，终于问题解决，给大家分享；

 

背景：M620上运行ESXI 6.0U2，服务器在重启的时候，HBA卡会间歇性丢失（非物理丢失，此时IDRAC可以识别），是OS下丢失，再次重启或重新扫描后能识别，但重启又可能会发生（不是每次发生）；

更换过OS版本和驱动及硬件FW及预防性更换主板HBA卡，问题定位在驱动识别后无法创建连接虚拟实例（VM这么说），之后一个多月都是围绕此思路展开。

 

Best Regards,

 

ZhouMing Yu  (俞周铭)

GC Resolution Manager

Dell \| Global Support and Deployment (GSD)

office +86-021-2203 0999

 

\-\-\-\--邮件原件\-\-\-\--

发件人: VMware Technical Support \[[mailto:webform@vmware.com](mailto:webform@vmware.com)[\] ]

发送时间: 2017年1月17日 13:50

收件人: caohui@htsc.com

抄送: Wang, Nan \<Nan_Wang@dell.com\>; Yu, Zhouming \<Zhouming_Yu@Dell.com\>; 972125722@qq.com; Han, Ruyang \<Ruyang_Han@Dell.com\>; huzhonghai@htsc.com

主题[: RE:VMware Support Request 16295564111 \[ ref:\_00D409hQR.\_50034t0mBC:ref \]]

 

曹先生:

 

你好!

非常感谢你的反馈.

通过修改FCoE HBA驱动的参数,我们解决了此case中出现的重启后无法自动识别FCoE卡的问题.

我建议在后续的部署中,如果还有类似问题出现,可以通过设置此参数解决.

\-\--

\# esxcli system module parameters set \--module bnx2fc \--parameter-string bnx2fc_autodiscovery=1

\-\--

 

鉴于此,我们就close这个case吧!

 

将来遇到VMware相关的技术问题,欢迎继续与我们联络取得支持.

 

Thanks

Bo Ou Zheng

\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original Message \-\-\-\-\-\-\-\-\-\-\-\-\-\--

From: 曹辉[ \[caohui@htsc.com\]]

Sent: 2017-1-17 PM1:15

To: webform@vmware.com

Cc: nan.wang@dell.com; zhouming.yu@dell.com; 972125722@qq.com; ruyang.han@dell.com; huzhonghai@htsc.com

Subject: 答复[: RE:VMware Support Request 16295564111 \[ \]]

 

Bo Ou ,

 

经过测试，改这个参数是work的，谢谢支持！

 

 

 

Best regards,

 

Cao Hui 曹辉

 

华泰证券股份有限公司\|信息技术部

 

固话:(86 25)83388227 手机:(86)13601743412 邮箱:caohui@htsc.com

 

地址:江苏省南京市江东中路228号

 

网址:https://urldefense.proofpoint.com/v2/url?u=http-3A\_\_www.htsc.com&d=DwIGaQ&c=uilaK90D4TOVoH58JNXRgQ&r=j_pEIlAElPXu_RSdExT8f2TSvbBhIQH9juHxIWG-XZQ&m=njtTC-FtGEmslteokUasC_tug1owX1tLyH8P4iTAfdQ&s=HHGb-dVuqodLlBs70C2MpUKtYOgx7Ahw1VGubEqJ2l8&e= 

 

 

 

\-\-\-\--邮件原件\-\-\-\--

 

发件人: VMware Technical Support \[[mailto:webform@vmware.com](mailto:webform@vmware.com)[\]]

 

发送时间: 2017年1月16日 15:29

 

收件人: 曹辉 \<caohui@htsc.com\>

 

抄送: nan.wang@dell.com; zhouming.yu@dell.com; 972125722@qq.com; ruyang.han@dell.com; 胡仲海 \<huzhonghai@htsc.com\>

 

主题[: RE:VMware Support Request 16295564111 \[ \]]

 

 

 

曹先生:

 

 

 

你好!

 

不知道修改FCoE HBA驱动默认参数是否可以解决此问题?

 

\-\-\--

 

3.设置FCoE的autodiscover参数,重启生效后进行测试.

 

 

 

\# esxcli system module parameters set \--module bnx2fc \--parameter-string bnx2fc_autodiscovery=1

 

\-\-\--

 

 

 

如果仍有问题,我建议收集最新日志,发送给研发继续协助定位问题.

 

 

 

有何问题,随时联络

 

 

 

Thanks

 

Bo Ou Zheng

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original Message \-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

From: 曹辉[ \[caohui@htsc.com\]]

 

Sent: 2017-1-6 PM4:57

 

To: webform@vmware.com

 

Cc: nan.wang@dell.com; zhouming.yu@dell.com; 972125722@qq.com; ruyang.han@dell.com; huzhonghai@htsc.com

 

Subject: 答复[: RE:VMware Support Request 16295564111 \[ \]]

 

 

 

谢谢，我们下周开始测试。

 

 

 

Best regards,

 

Cao Hui 曹辉

 

华泰证券股份有限公司\|信息技术部

 

 

 

固话:(86 25)83388227 手机:(86)13601743412 邮箱:caohui@htsc.com

 

 

 

地址:江苏省南京市江东中路228号

 

 

 

网址:https://urldefense.proofpoint.com/v2/url?u=http-3A\_\_www.htsc.com&d=DgIGaQ&c=uilaK90D4TOVoH58JNXRgQ&r=j_pEIlAElPXu_RSdExT8f2TSvbBhIQH9juHxIWG-XZQ&m=9VUz4PmtoHnDzIS2KJq3UZxIm4wViYdVwKSvSDH_9-M&s=94b-CnHCqnrACTAfHLTsKYazZk4wsckWSPN341hDtd8&e=

 

 

 

 

 

 

 

\-\-\-\--邮件原件\-\-\-\--

 

 

 

发件人: VMware Technical Support \[[mailto:webform@vmware.com](mailto:webform@vmware.com)[\]]

 

 

 

发送时间: 2017年1月6日 15:26

 

 

 

收件人: 曹辉 \<caohui@htsc.com\>

 

 

 

抄送: nan.wang@dell.com; zhouming.yu@dell.com; 972125722@qq.com; ruyang.han@dell.com; 胡仲海 \<huzhonghai@htsc.com\>

 

 

 

主题[: RE:VMware Support Request 16295564111 \[ \]]

 

 

 

 

 

 

 

曹先生:

 

 

 

 

 

 

 

你好!

 

 

 

根据刚才电话沟通,重新安装DELL OEM 6.0U2 ,A03版本无法消除症状.

 

 

 

我建议尽快测试产品研发的建议:

 

 

 

 

 

 

 

\-\--

 

 

 

3.设置FCoE的autodiscover参数,重启生效后进行测试.

 

 

 

 

 

 

 

\# esxcli system module parameters set \--module bnx2fc \--parameter-string bnx2fc_autodiscovery=1

 

 

 

\-\--

 

以上命令在ESXi CLI运行即可.

 

如果重启后测试,仍有问题,建议收集最新的日志进行检查.

 

有何问题,随时联络

 

Thanks

 

Bo Ou Zheng

Technical Support Engineer

Global Support Services, VMware Inc.

China (Mainland): 8009151919, 4008160688 Hong Kong :81018178, Taiwan: 00801863109

 

Global Support Phone Numbers: <http://www.vmware.com/support/contacts/us_support>

Satisfied customers are our top priority.

\-\-\-\-\-\-\-\--

ref:\_00D409hQR.\_50034t0mBC:ref

 

已使用 OneNote 创建。
