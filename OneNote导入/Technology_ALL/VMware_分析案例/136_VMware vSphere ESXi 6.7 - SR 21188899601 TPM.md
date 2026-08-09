VMware vSphere ESXi 6.7 - SR 21188899601 TPM

2022年3月25日

8:50

  ------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   回复: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC[  6.7    ESXi   6.7  \问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错[  \问题参数[\]]：[  \相关操作[\]]： \[辅助资源[\]]：提[    \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]
  From      Yin, Victor
  To        Zeng, Jackie; Li, Feelyn; Yao, Gavyn (c); He, Renbin
  Cc        Ye, Dony; Ruan, Garuda
  Sent      2021年1月26日 12:25
  ------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell Customer Communication - Confidential

 

Hi Jackie

 

是的，目前中国区的TPM无法提供EK，而且从供应商层面我们也无法做出什么调整。

中国区TPM不提供EK目前对所有操作系统都是这种情况，硬件层面没有什么方法可调整。

 

 

Best Regards

Victor Yin

Senior Engineer \| China - ISG Global Network and Compute

Dell Technologies \| APJC ISG Support Services

Office +86 21 220 30848 EXT. 8830848

[victor_yin@dell.com](mailto:victor_yin@dell.com)  

![[Technology_ALL_VMware_分析案例_136_VMware vSphere ESXi 6.7 - SR 21188899601_001.jpg]]

How am I doing? Please contact my manager <Xing_Fang_Wang@DELL.com> to provide feedback. Thanks!

 

 

 

发件人: Zeng, Jackie \<Jackie_Zeng@DELL.com\> 

发送时间: 2021年1月26日 8:53

收件人: Yin, Victor; Li, Feelyn; Yao, Gavyn (c); He, Renbin

抄送: Ye, Dony; Ruan, Garuda

主题: 回复: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提 [\[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

Dell Customer Communication - Confidential

 

Hi Victor ,

 

多谢你的说明， 根据你的邮件反馈，是否我们可以认为以下几点：

 

1， VMWARE ESXI 主机如果要使用TPM 进行信任验证等功能需要TPM 2.0 且有Endorsement Key 证书？ 

2，中国版本的TPM 2.0是中国法律规定使用不同于国外的TPM 2.0版本，此中国版TPM2.0 不提供/或是包含Endorsement Key 证书。

3，在中国使用的TPM2.0中国版本，从硬件层面无法解决该问题(无Endorsement Key 证书) 。

 

谢谢！

 

[\@He, Renbin](mailto:Renbin_He@Dell.com)  还请再等待L2的回复 ，但是基于L2前一封邮件的说明，很可能你要设置好客户的期望值，我们DELL在中国必须符合中国法律，中国范围内该TPM的EK问题应该是从硬件角度无法解决，从售后无法再进一步协助处理了。

 

请你和售前团队/marketing 团队再咨询看看是否有成功案例，如果不行的话，可能要做相应的商务决策，看是让客户退货还是准备按合同法规处理(建议咨询法务部)。

 

谢谢！

 

Jackie Zeng

Resolution Manager, Support Resolution Team

Dell Technologies \| DT Services

office [+86-592-8186135](tel:+86-592-8186135) 

[Jackie.Zeng@DELL.com](mailto:Jackie.Zeng@DELL.com) 

 

发件人: Yin, Victor \<[Victor_Yin@Dell.com](mailto:Victor_Yin@Dell.com)\> 

发送时间: 2021年1月25日 19:57

收件人: Zeng, Jackie; Li, Feelyn; [webform@vmware.com](mailto:webform@vmware.com); Yao, Gavyn (c)

抄送: Ye, Dony; Ruan, Garuda; He, Renbin

主题: 回复: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

Dell Customer Communication - Confidential

 

HI All

 

和VMware engineer沟通，怀疑用户当前设置不符合以下VMware要求

[https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-9957A9F4-1529-4128-8700-0C43CCA58C65.html](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-9957A9F4-1529-4128-8700-0C43CCA58C65.html) 

![[Technology_ALL_VMware_分析案例_136_VMware vSphere ESXi 6.7 - SR 21188899601_002.png]]

 

 

已经和IPS确认TPM（China）的芯片供应商为NationZ(NTZ),  NTZ 为政府指定芯片供应商，此安全芯片设计符合TPM 2.0要求，但是此芯片确实不提供endorsement key (EK) certificate.

NTZ 芯片不提供EK 是work by design，厂商并不计划在后续修改此问题，目前china TPM 对所有系统都一样不提供EK，硬件层面没有临时解决方案。

 

目前就我们所知，所有国内使用的硬件都只能使用TPM China NTZ芯片。

 

 

Best Regards

Victor Yin

Senior Engineer \| China - ISG Global Network and Compute

Dell Technologies \| APJC ISG Support Services

Office +86 21 220 30848 EXT. 8830848

[victor_yin@dell.com](mailto:victor_yin@dell.com)  

![[Technology_ALL_VMware_分析案例_136_VMware vSphere ESXi 6.7 - SR 21188899601_001.jpg]]

How am I doing? Please contact my manager <Xing_Fang_Wang@DELL.com> to provide feedback. Thanks!

 

 

 

 

发件人: Zeng, Jackie \<[Jackie_Zeng@DELL.com](mailto:Jackie_Zeng@DELL.com)\> 

发送时间: 2021年1月25日 16:43

收件人: Li, Feelyn; [webform@vmware.com](mailto:webform@vmware.com); Yin, Victor; Yao, Gavyn (c)

抄送: Ye, Dony; Ruan, Garuda

主题: 回复: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

Dell Customer Communication - Confidential

 

在我们的技术沟通讨论邮件中暂时先移除客户，以便讨论一些详细的技术细节，待有讨论结果后再将结果告知客户。 谢谢！

 

 

[\@Yin, Victor](mailto:Victor_Yin@Dell.com) [\@Yao, Gavyn (c)](mailto:gyao@vmware.com)  不好意思，我这一周在参加培训，没有办法及时查邮件或是回复各位的邮件。

 

请问一下今天早上远程检查的情况如何？ Victor 从硬件的角度检查BIOS中关于TPM 的设置是否已经正确设置和开启呢？ Gavyn是否也有帮客户检查一下ESXI的相关配置？ 目前我们这边有什么发现吗？

 

如同上周我们沟通的情况，Victor 能否帮从IPS(L3/研发)处了解到TPM (China) 的规格(芯片厂商、芯片名称、支持的加密协议) ， Gavyn也与VMWARE L3 /研发帮确认一下客户使用的ESXI 6.7 是否能支持中国的TPM 2.0 版本？

 

另外，Victor 还请帮忙就Gavyn 之前邮件描述的VMWARE 兼容列表中2.9.4 BIOS 支持TPM 1.2 + TXT 的情况和IPS再确认一下，看看是否有资料描述该版本BIOS是否支持TPM 2.0 or TPM 2.0 China 还是如同VMWARE官网描述一样仅支持TPM 1.2 TXT ?

 

谢谢！

 

Jackie Zeng

Resolution Manager, Support Resolution Team

Dell Technologies \| DT Services

office [+86-592-8186135](tel:+86-592-8186135) 

[Jackie.Zeng@DELL.com](mailto:Jackie.Zeng@DELL.com) 

 

发件人: [Feng.Xue@stihl.cn](mailto:Feng.Xue@stihl.cn) \<[Feng.Xue@stihl.cn](mailto:Feng.Xue@stihl.cn)\> 

发送时间: 2021年1月25日 16:24

收件人: Li, Feelyn; Zeng, Jackie; [webform@vmware.com](mailto:webform@vmware.com)

抄送: [technical_support@help.dell.com](mailto:technical_support@help.dell.com); Ye, Dony; Ruan, Garuda; Yin, Victor

主题: RE: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

\[EXTERNAL EMAIL\]

Unable to provision Endorsement Key on TPM 2.0 device: No RSA Endorsement Key certificate found in TPM 2.0 device\'s non-volatile memory.

![[Technology_ALL_VMware_分析案例_136_VMware vSphere ESXi 6.7 - SR 21188899601_003.png]]

 

Best Regards

 

Jackey

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Taicang Andreas Stihl Powertools Co., Ltd.

太仓安德烈.斯蒂尔动力工具有限公司

No.7, East Ningbo Road, Taicang City, Jiangsu

江苏省，太仓市，宁波东路7号

P. R. of CHINA

                       

Phone: +86 512 53670930

Mobile: +86 13962626542

Telefax: +86 512 53670967

E-Mail: <feng.xue@stihl.cn>

Web: [www.stihl.cn](http://www.stihl.cn/)

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

 

From: <Feelyn.Li@dell.com> \<<Feelyn.Li@dell.com>\>

Sent: Monday, January 25, 2021 4:18 PM

To: CS/OIS Xue, Jackey \<<Feng.Xue@stihl.cn>\>; <Jackie.Zeng@dell.com>; <webform@vmware.com>

Cc: <technical_support@help.dell.com>; <Dony.Ye@dell.com>; <Garuda.Ruan@dell.com>; <Victor.Yin@Dell.com>

Subject: 回复: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提 [\[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

Dell Customer Communication - Confidential

 

薛先生：您好\~ 

              麻烦您帮忙提供下当前的详细的报错信息，我找工程师协助分析。 谢谢\~

 

 

 

Best regards！

Feelyn_li李輝琳

 

 

 

发件人: [Feng.Xue@stihl.cn](mailto:Feng.Xue@stihl.cn) \<[Feng.Xue@stihl.cn](mailto:Feng.Xue@stihl.cn)\> 

发送时间: 2021年1月25日 16:00

收件人: Zeng, Jackie; [webform@vmware.com](mailto:webform@vmware.com)

抄送: Li, Feelyn; [technical_support@help.dell.com](mailto:technical_support@help.dell.com); Ye, Dony; Ruan, Garuda; Yin, Victor

主题: RE: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提 [\[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

重要性: 高

 

\[EXTERNAL EMAIL\]

Dear Sirs,

 

It shows the same error message after we change the setting and add the ESXi host back to Vcenter.

 

Would you please kindly help us to check more details ASAP!!!

 

Please inform us if you need more information.

 

Thank you very much for your support!

 

 

Best Regards

 

Jackey

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Taicang Andreas Stihl Powertools Co., Ltd.

太仓安德烈.斯蒂尔动力工具有限公司

No.7, East Ningbo Road, Taicang City, Jiangsu

江苏省，太仓市，宁波东路7号

P. R. of CHINA

                       

Phone: +86 512 53670930

Mobile: +86 13962626542

Telefax: +86 512 53670967

E-Mail: <feng.xue@stihl.cn>

Web: [www.stihl.cn](http://www.stihl.cn/)

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

 

From: Zeng, Jackie \<<Jackie.Zeng@dell.com>\>

Sent: Friday, January 22, 2021 5:32 PM

To: CS/OIS Xue, Jackey \<<Feng.Xue@stihl.cn>\>; <webform@vmware.com>

Cc: Li, Feelyn \<<Feelyn.Li@dell.com>\>; <technical_support@help.dell.com>; Ye, Dony \<<Dony.Ye@dell.com>\>; Ruan, Garuda \<<Garuda.Ruan@dell.com>\>; Yin, Victor \<<Victor.Yin@Dell.com>\>

Subject: 回复: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

Internal Use - Confidential

 

Hi 薛先生：

 

可以的，周一我们一起约个时间远程检查一下看看BIOS中的设置，然后一起沟通讨论 一下吧。

 

谢谢！

 

Jackie Zeng

Resolution Manager, Support Resolution Team

Dell Technologies \| DT Services

office [+86-592-8186135](tel:+86-592-8186135) 

[Jackie.Zeng@DELL.com](mailto:Jackie.Zeng@DELL.com) 

 

发件人: [Feng.Xue@stihl.cn](mailto:Feng.Xue@stihl.cn) \<[Feng.Xue@stihl.cn](mailto:Feng.Xue@stihl.cn)\> 

发送时间: 2021年1月22日 17:11

收件人: [webform@vmware.com](mailto:webform@vmware.com)

抄送: Li, Feelyn; Zeng, Jackie; [technical_support@help.dell.com](mailto:technical_support@help.dell.com)

主题: RE: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

\[EXTERNAL EMAIL\]

您好，Gavyn

 

非常感谢您的协助！

 

\@feelyn.li@dell.com; <jackie.zeng@dell.com>

请看下下周一什么时间方便，我们一起远程同时看下设置，并沟通讨论一下问题？

 

谢谢！

 

 

 

Best Regards

 

Jackey

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Taicang Andreas Stihl Powertools Co., Ltd.

太仓安德烈.斯蒂尔动力工具有限公司

No.7, East Ningbo Road, Taicang City, Jiangsu

江苏省，太仓市，宁波东路7号

P. R. of CHINA

                       

Phone: +86 512 53670930

Mobile: +86 13962626542

Telefax: +86 512 53670967

E-Mail: <feng.xue@stihl.cn>

Web: [www.stihl.cn](http://www.stihl.cn/)

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

 

From: VMware Technical Support \<<webform@vmware.com>\>

Sent: Friday, January 22, 2021 5:01 PM

To: CS/OIS Xue, Jackey \<<Feng.Xue@stihl.cn>\>

Cc: <feelyn.li@dell.com>; <jackie.zeng@dell.com>; <technical_support@help.dell.com>

Subject: RE: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

薛先生，您好

 

我是VMware工程师Gavyn，感谢反馈。

 

我这边下周一暂时没有其它计划，如果方便我们一起看一下，具体时间您可以来安排，我会尽量配合您的时间。

 

另外，

请TPM芯片厂商提前确认是否支持SHA-256？

请DELL提前确认BIOS 2.9.4版本是否添加了TPM2.0支持？

 

BR

Gavyn Yao

\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original Message \-\-\-\-\-\-\-\-\-\-\-\-\-\--

From: \[feng.xue@stihl.cn\]

Sent: 1/22/2021 4:11 PM

To: <webform@vmware.com>

Cc: <jackie.zeng@dell.com>; <technical_support@help.dell.com>; <feelyn.li@dell.com>

Subject: RE: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

你好，

 

鉴于该case比较特殊，设计到两个不同供应商，在此，我把相关人员都抄送一下，便于大家的认知一致；

目前按照VMware技术工程师建议的设置，之前应该已有测试的，结果是一致的；

\@VMware technical support

@ Jackie

请看下是否还需要再尝试做以下设置，来再次验证一下？

我建议大家一起开个远程看一下，可以吗？不要来回再浪费大家时间，可以吗？

 

 

 

Best Regards

 

Jackey

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Taicang Andreas Stihl Powertools Co., Ltd.

太仓安德烈.斯蒂尔动力工具有限公司

No.7, East Ningbo Road, Taicang City, Jiangsu

江苏省，太仓市，宁波东路7号

P. R. of CHINA

                       

Phone: +86 512 53670930

Mobile: +86 13962626542

Telefax: +86 512 53670967

E-Mail: <feng.xue@stihl.cn>

Web: [www.stihl.cn](https://nam04.safelinks.protection.outlook.com/?url=http%3A%2F%2Fwww.stihl.cn%2F&data=04%7C01%7Cwebform%40vmware.com%7C31eb270df19242ed181708d8bead570b%7Cb39138ca3cee4b4aa4d6cd83d9dd62f0%7C0%7C0%7C637468999000568942%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000&sdata=iuTsxKBAl%2Fx3VBLu2KPOKFkvYtgEnoGvGnR8IEZ4A2o%3D&reserved=0)

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

 

From: VMware Technical Support \<<webform@vmware.com>\>

Sent: Friday, January 22, 2021 2:34 PM

To: CS/OIS Xue, Jackey \<<Feng.Xue@stihl.cn>\>

Subject: RE: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

薛先生,您好

 

我是VMware工程师Gavyn，我收到前线工程师的反馈，关于无法正常使用TPM的问题。

 

为了进一步了解当前的配置是否满足要求，我这边汇总一些资料如下：

 

DELL反馈的邮件中提到的通信协议，我的理解应该是HASH算法。

 

配置TPM2.0的前提条件：

1、TPM硬件设备需要支持SHA-256算法，China TPM chip vendor 一般可能会采用 SM3，据我了解DELL的Firmware可能无法支持SM3。

2、BIOS需要设置为UEFI安全启动；

3、BIOS需要设置SHA-256哈希认证；

4、如果可用，还必须将其设置为使用IS/FIFO（先进先出）接口，而不是CRB（命令响应缓冲区）

5、根据我们之前的案例，采用过Dell PowerEdge R630进行过测试，配置部分截图如下：外观可能有所变化

\[Image is no longer available\]

\[Image is no longer available\]

 

6、另外根据目前我们收到DELL合作伙伴的认证资料来看，当前R740使用的BIOS版本2.9.4，仅提供了TPM1.2的支持，由于认证信息更新可能有延迟，请协DELL助确认2.9.4版本是否添加了TPM2.0支持？

\[Image is no longer available\]

 

请协助确认以上前提条件是否满足要求，同时需要提供第5步骤中的配置截图信息。另外也请帮忙反馈一下当前使用的TPM 2.0芯片的供应商。

 

希望可以帮助到您，期待您回复。

 

BR

Gavyn Yao

\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original Message \-\-\-\-\-\-\-\-\-\-\-\-\-\--

From: \[feng.xue@stihl.cn\]

Sent: 1/22/2021 12:53 PM

To: <webform@vmware.com>

Subject: RE: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

 

您好，

 

已沟通联系Dell 公司，请看下他们的解释，看下目前的通讯问题是什么情况，如何解决？

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--以下为转发信息\-\-\-\-\-\-\-\-\-\-\-\-\-\--

薛先生：

 

您好！

 

非常感谢您提供相关设置信息，根据您的PDF文档显示在服务器BIOS中已经设置好TPM 相关设置，理论上硬件TPM功能已经启用和打开 。

 

在您提供的ESXI () kernel日志中发现中无法使用且OS中显示已经加载TPM驱动并识别到TPM  ，但是似乎两者之间的协议通讯存在问题，所以需要您和VMWARE方确认是否您安装的VMware ESXi 6.7.0 build-17167734 支持中国境内唯一允许销售和使用的TPM 2.0 (China)版本。

 

\[Image is no longer available\]

 

另外，您服务器上无购买操作系统且您提供的ESXI日志显示您当前使用的VMWARE 许可已经过期，请您检查并确认许可有效后联系VMware 工程师一起支持和确认为何ESXI 6.7 不能与中国境内使用的TPM 2.0(TCM) 协议通讯问题。

 

\[Image is no longer available\]

 

 

Best Regards

 

Jackey

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Taicang Andreas Stihl Powertools Co., Ltd.

太仓安德烈.斯蒂尔动力工具有限公司

No.7, East Ningbo Road, Taicang City, Jiangsu

江苏省，太仓市，宁波东路7号

P. R. of CHINA

                       

Phone: +86 512 53670930

Mobile: +86 13962626542

Telefax: +86 512 53670967

E-Mail: <feng.xue@stihl.cn>

Web: [www.stihl.cn](https://nam04.safelinks.protection.outlook.com/?url=http%3A%2F%2Fwww.stihl.cn%2F&data=04%7C01%7Cwebform%40vmware.com%7C31eb270df19242ed181708d8bead570b%7Cb39138ca3cee4b4aa4d6cd83d9dd62f0%7C0%7C0%7C637468999000568942%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000&sdata=iuTsxKBAl%2Fx3VBLu2KPOKFkvYtgEnoGvGnR8IEZ4A2o%3D&reserved=0)

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

 

From: VMware Technical Support \<<webform@vmware.com>\>

Sent: Thursday, January 21, 2021 3:11 PM

To: CS/OIS Xue, Jackey \<<Feng.Xue@stihl.cn>\>

Subject: RE: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

尊敬的用户，

 

我是VMware售后技术工程师Ada Zhang，

 

您开完会回复我，我尽快联系您

 

您环境中的BIOS对应TPM查询截图，如下：

\[Image is no longer available\]

如果BIOS支持TPM2.0，显示如下：

\[Image is no longer available\]

 

 

如果有其它问题，可以随时邮件回复我

 

 

此致！

 

Ada Zhang

技术支持工程师

VMware大中华区技术支持中心 \| VMware Inc.

工作时间：周一至周五 8:00AM -- 6:00PM CST

支持热线：800 915 1919 \\ 400 816 0688 （大陆）\| 8101 8178 (香港) \| 080 0091（澳门）\| 0080 1863109 （台湾）

 

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

重要通知：

由于ESXi 6.0和vCenter Server 6.0等产品的一般技术支持已于2020年3月12日截止，针对这些产品的服务会在一定范围内受限，只提供web支持。为了不影响贵公司的支持服务，建议尽快升级到更高版本。

另外，对于ESXi 5.5、vCenter Server 5.5等产品即将于2020年9月19日停止任何形式的技术支持，届时您将只能通过自助资源来获取帮助。

关于产品支持周期详细信息请参考链接： [http://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/support/product-lifecycle-matrix.pdf](http://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/support/product-lifecycle-matrix.pdf) 

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original Message \-\-\-\-\-\-\-\-\-\-\-\-\-\--

From: VMware Technical Support \[webform@vmware.com\]

Sent: 1/21/2021 2:55 PM

To: <feng.xue@stihl.cn>

Subject: RE: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

尊敬的用户，

 

我是VMware售后技术工程师Ada Zhang，感谢您开启支持请求，目前由我来负责协助您处理此问题。

 

[https://VMware.zoom.com.cn/j/4970532811?pwd=RzV4cDdscyt4eFJBRUFMTlJYSGV6dz09](https://nam04.safelinks.protection.outlook.com/?url=https%3A%2F%2Fvmware.zoom.com.cn%2Fj%2F4970532811%3Fpwd%3DRzV4cDdscyt4eFJBRUFMTlJYSGV6dz09&data=04%7C01%7Cwebform%40vmware.com%7C31eb270df19242ed181708d8bead570b%7Cb39138ca3cee4b4aa4d6cd83d9dd62f0%7C0%7C0%7C637468999000578908%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000&sdata=k0jLRjP6iY1FlHBIvYn3brluYSxyTrOr7YFNZeLvUCk%3D&reserved=0)

 

 

 

如果有其它问题，可以随时邮件回复我

 

 

此致！

 

Ada Zhang

技术支持工程师

VMware大中华区技术支持中心 \| VMware Inc.

工作时间：周一至周五 8:00AM -- 6:00PM CST

支持热线：800 915 1919 \\ 400 816 0688 （大陆）\| 8101 8178 (香港) \| 080 0091（澳门）\| 0080 1863109 （台湾）

 

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

重要通知：

由于ESXi 6.0和vCenter Server 6.0等产品的一般技术支持已于2020年3月12日截止，针对这些产品的服务会在一定范围内受限，只提供web支持。为了不影响贵公司的支持服务，建议尽快升级到更高版本。

另外，对于ESXi 5.5、vCenter Server 5.5等产品即将于2020年9月19日停止任何形式的技术支持，届时您将只能通过自助资源来获取帮助。

关于产品支持周期详细信息请参考链接： [http://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/support/product-lifecycle-matrix.pdf](http://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/support/product-lifecycle-matrix.pdf) 

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original Message \-\-\-\-\-\-\-\-\-\-\-\-\-\--

From: \[feng.xue@stihl.cn\]

Sent: 1/21/2021 11:24 AM

To: <webform@vmware.com>

Cc: <jackeyxue@126.com>

Subject: RE: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

 

您好，版本信息如下：

vSphere Client version 6.7.0.46000

VMware ESXi, 6.7.0, 17167734 

 

 

Best Regards

 

Jackey

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

Taicang Andreas Stihl Powertools Co., Ltd.

太仓安德烈.斯蒂尔动力工具有限公司

No.7, East Ningbo Road, Taicang City, Jiangsu

江苏省，太仓市，宁波东路7号

P. R. of CHINA

                       

Phone: +86 512 53670930

Mobile: +86 13962626542

Telefax: +86 512 53670967

E-Mail: <feng.xue@stihl.cn>

Web: [www.stihl.cn](https://nam04.safelinks.protection.outlook.com/?url=http%3A%2F%2Fwww.stihl.cn%2F&data=04%7C01%7Cwebform%40vmware.com%7C31eb270df19242ed181708d8bead570b%7Cb39138ca3cee4b4aa4d6cd83d9dd62f0%7C0%7C0%7C637468999000578908%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C1000&sdata=njUKGK3lC3Aal5UfT2Ot05UvCryY49tt7s7xaat3Y5k%3D&reserved=0)

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

 

From: Jackey \<<jackeyxue@126.com>\>

Sent: Thursday, January 21, 2021 11:18 AM

To: CS/OIS Xue, Jackey \<<Feng.Xue@stihl.cn>\>

Subject: Fw:VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

 

 

 

 

 

 

 

\-\-\-\-\-\-\-- 转发邮件信息 \-\-\-\-\-\-\--

发件人：\"VMware Technical Support\" \<[webform@vmware.com](mailto:webform@vmware.com)\>

发送日期：2021-01-21 10:33:41

收件人：\"[jackeyxue@126.com](mailto:jackeyxue@126.com)\" \<[jackeyxue@126.com](mailto:jackeyxue@126.com)\>

主题：VMware vSphere ESXi 6.7 - SR 21188899601 - \[客户环境[\]]： VC 6.7 ESXi 6.7 \[问题描述[\]]：安装完戴尔740服务器后，在VC上出现报错 \[问题参数[\]]： \[相关操作[\]]： \[辅助资源[\]]：提[ \[ ref:\_00Df43u6t.\_5005Gbi25Q:ref \]]

尊敬的用户，

 

我是VMware售后技术工程师Ada Zhang，

 

1、根据日志内容，指向以下KB：[https://kb.vmware.com/s/article/52771?lang=zh_cn](https://kb.vmware.com/s/article/52771?lang=zh_cn)

根据KB内容，建议您对TPM的模式进行更改：

a、从 BIOS 禁用 TPM

b、切换到 TPM 1.2 模式

 

2、日志内容：

2021-01-20T02:53:41.868Z info hostd\[2099213\] \[Originator@6876 sub=Hostsvc.HostTpmManager\] Creating HostTPMManager\...

2021-01-20T02:53:41.871Z info hostd\[2099213\] \[Originator@6876 sub=Hostsvc.TpmEventLogProvider\] TpmEventLogProvider created

2021-01-20T02:53:41.957Z info hostd\[2099213\] \[Originator@6876 sub=Hostsvc.Tpm20Provider\] Preprovisioned endorsement key not found at 0x81010001

2021-01-20T02:53:41.977Z error hostd\[2099213\] \[Originator@6876 sub=Hostsvc.Tpm20Provider\] NV_ReadPublic: (0x18b) Unknown

2021-01-20T02:53:41.977Z info hostd\[2099213\] \[Originator@6876 sub=Hostsvc.Tpm20Provider\] Unable to read default RSA EK certificate!

2021-01-20T02:53:41.977Z error hostd\[2099213\] \[Originator@6876 sub=Hostsvc.Tpm20Provider\] Unable to provision default rsa endorsement key.

2021-01-20T02:53:41.977Z info hostd\[2099213\] \[Originator@6876 sub=Hostsvc.Tpm20Provider\] Raised TPM Config Issue: (vim.event.EventEx)

 

3、还需要与您确认，目前VC是什么版本，具体到build号码

 

 

此致！

 

Ada Zhang

技术支持工程师

VMware大中华区技术支持中心 \| VMware Inc.

工作时间：周一至周五 8:00AM -- 6:00PM CST

支持热线：800 915 1919 \\ 400 816 0688 （大陆）\| 8101 8178 (香港) \| 080 0091（澳门）\| 0080 1863109 （台湾）

 

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

重要通知：

由于ESXi 6.0和vCenter Server 6.0等产品的一般技术支持已于2020年3月12日截止，针对这些产品的服务会在一定范围内受限，只提供web支持。为了不影响贵公司的支持服务，建议尽快升级到更高版本。

另外，对于ESXi 5.5、vCenter Server 5.5等产品即将于2020年9月19日停止任何形式的技术支持，届时您将只能通过自助资源来获取帮助。

关于产品支持周期详细信息请参考链接： [http://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/support/product-lifecycle-matrix.pdf](http://www.vmware.com/content/dam/digitalmarketing/vmware/en/pdf/support/product-lifecycle-matrix.pdf) 

\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

 

\--

Ada Zhang

TSE

Global Support Services, VMware Inc.

Office Hours:

If you wish to provide feedback on the quality of support provided, please contact my direct manager, Bing Ai, at <bai@vmware.com>. Satisfied customers are our top priority.

\*\*Flash is going EOL in Dec 2020, find out what it will mean for vSphere web Client\*\*

[https://kb.vmware.com/s/article/78589](https://kb.vmware.com/s/article/78589)

 

Review your Support Requests Online: <http://support.vmware.com/selfsupport/> Existing Service Request: 1-877-4-VMWARE (1-877-486-9273), Option 4 to speak to customer support

Global Support Phone Numbers: <http://www.vmware.com/support/phone_support.html>

 

 

![[Technology_ALL_VMware_分析案例_136_VMware vSphere ESXi 6.7 - SR 21188899601_004.jpg]]

 

ref:\_00Df43u6t.\_5005Gbi25Q:ref

 

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

 

If you are not the legitimate recipient, please send the e-mail back and delete it

on your system. Any unauthorized use or transfer of confidential information

may have legal consequences.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

 

If you are not the legitimate recipient, please send the e-mail back and delete it

on your system. Any unauthorized use or transfer of confidential information

may have legal consequences.

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

 

 

If you are not the legitimate recipient, please send the e-mail back and delete it

on your system. Any unauthorized use or transfer of confidential information

may have legal consequences.

 

已使用 OneNote 创建。
