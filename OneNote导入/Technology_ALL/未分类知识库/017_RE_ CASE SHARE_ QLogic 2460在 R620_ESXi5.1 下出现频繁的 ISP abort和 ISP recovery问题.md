RE: CASE SHARE: QLogic 2460在 R620/ESXi5.1 下出现频繁的 ISP abort和 ISP recovery问题

Thursday, October 09, 2014

10:07 AM

  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------
  主题       RE: CASE SHARE: QLogic 2460在 R620/ESXi5.1 下出现频繁的 ISP abort和 ISP recovery问题
  发件人     Yin, Guoxun
  收件人     CN XMN TS Server Coach
  发送时间   Thursday, October 09, 2014 10:05 AM
  附件       \<\<答复 VMware Support Request SR 14538792210     ref_00D409hQR.\_50080bBAZBref .msg\>\>
  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Hi Team

已经证实了这是个已知issue了，KCS新增了文章如附件，谢谢TianQiao提醒。

 

 

 

Best Regards

 

Yin Guo Xun

Dell \| Enterprise Support Services

Mail Address:[guoxun_yin@dell.com](http://guoxun_yin@dell.com)

Certifications: VCP3/4/5 , CCA , HPUX_CSA

 

How am I doing? Email my manager ([Wang, XingFang](mailto:Xing_Fang_Wang@Dell.com)) with any feedback.

 

From: Yin, Guoxun

Sent: 2014年8月27日 11:08

To: CN XMN TS Server Coach

Subject: CASE SHARE: QLogic 2460在 R620/ESXi5.1 下出现频繁的 ISP abort和 ISP recovery问题

 

Dell - Internal Use - Confidential 

如题，这是一个真实的CASE，FC链路全部断开，失去与存储的连接。从日志里面看到的报错如下图，做过了所有的努力后无解。

ESX/ESXi 4.0下面有类似的bug，所以按照相应的解决方式，针对5.1做了相应调整，应用以下命令修改HBA的中断工作模式后，测试了一个月工作正常未复现。算是个workaround吧。

 

esxcfg-module -s \"ql2xenablemsi24xx=1\" qla2xxx  (需重启主机)

 

 

![[Technology_ALL_未分类知识库_017_RE_ CASE SHARE_ QLogic 2460在 R620_ESXi5.1_001.png]]

 

Best Regards

 

Yin Guo Xun

Dell \| Enterprise Support Services

Mail Address:[guoxun_yin@dell.com](http://guoxun_yin@dell.com)

Certifications: VCP3/4/5 , CCA , HPUX_CSA

 

How am I doing? Email my manager ([Wang, XingFang](mailto:Xing_Fang_Wang@Dell.com)) with any feedback.

 

 

已使用 OneNote 创建。
