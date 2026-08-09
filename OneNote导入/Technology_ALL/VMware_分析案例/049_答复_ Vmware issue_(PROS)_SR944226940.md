答复: Vmware issue\|(PROS)\|SR944226940 

Wednesday, February 22, 2017

2:46 PM

  -------------------------------------- ------------------------------------------------------------------------------------------------------------
  主题       答复: Vmware issue\|(PROS)\|SR944226940 
  发件人     Lin, Yongliang
  收件人     Huang, Zhenxiong; Yin, Guoxun; Li, Zhiyong
  抄送       CN XMN TS ENT L2 SME
  发送时间   Wednesday, February 22, 2017 2:17 PM
  -------------------------------------- ------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Hi guoxun:

 

help

 

Thanks & Best Regards.

Yongliang Lin

Enterprise Product Engineer

Dell \| Enterprise Support Services

 

发件人: Huang, Zhenxiong 

发送时间: 2017年2月22日 13:46

收件人: CN XMN TS Server Escalation \<CNXMNTSServerEscalation@DELL.com\>; Li, Zhiyong \<Zhiyong_Li@Dell.com\>

抄送: Huang, Zhenxiong \<Zhenxiong_Huang@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>

主题: Vmware issue\|(PROS)\|SR944226940 

 

Dell - Internal Use - Confidential 

Dear L2 Team

 

 

a.     Detail Symptom Descriptions

详细的故障现象描述:（请务必详细描述故障现象，例如诊断灯状态，显示器上的报错内容，软件报错信息或截图，故障时间点，频率(对于Unstable Case)，故障前情况等等。） 

Hi Jacky,

以下是新的FTP 信息，感谢。

 

Homepage:         <https://fileexchanger.dell.com>

Username:

877792231 

Password:

4pQxFlzBGm

 

 

Hi Zhenxiong，

我们又遇到了新的VSAN问题，不同于以往，请创建个新的SR给我，感谢。

 

 

BR.

Guoxun.

From: Jacky Chiang \[[mailto:jacky.chiang@azurewave.com](mailto:jacky.chiang@azurewave.com)\]

Sent: 2017年2月21日 13:32

To: Yin, Guoxun \<<guoxun_Yin@Dell.com>\>

Cc: Chern, Jeff \<<Jeff_Chern@Dell.com>\>; Huang, Zhenxiong \<<Zhenxiong_Huang@Dell.com>\>

Subject: RE: Dell Technical Support (order:890326164 )

 

Hi Guoxun

 

  三點時間沒問題，謝謝。

 

 

海華科技股份有限公司  資訊暨智能工程部

AzureWave Technologies, Inc.

[TEL:886-2-55995599](tel:886-2-66289888) ext.5595

FAX:886-2-66289666

Mail: Jacky.Chiang[\@azurewave.com](mailto:JackyChiang@azurewave.com)

 

 

From: <Guoxun.Yin@dell.com> \[[mailto:Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\]

Sent: Tuesday, February 21, 2017 12:40 PM

To: Jacky Chiang \<<jacky.chiang@azurewave.com>\>

Cc: <Jeff.Chern@dell.com>; <Zhenxiong.Huang@dell.com>

Subject: RE: Dell Technical Support (order:890326164 )

 

Hi  Jacky,

我看到2月13日和2月15日这些是VSAN的health check 后台服务周期性检查主机并产生的纪录，这与我们之前遇到的disk controller兼容问题和 disk group member离线问题不同，看起来我们之前处理的问题已经解决了。

现在所报告的问题和如何安排依次解释如下, 我下午大概3点左右联系您我们远程检查下，请问您是否有时间？

 

 

Network health

需要检查各个主机的历史报警记录看是否有网络问题\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--远程检查

 

All hosts have a virtual SAN vmknc configured

需要确保所有主机都有对应的专用于VSAN的vmkernel 设置\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--远程检查

 

All hosts have matching subnets

VSAN network所用的ip 地址需要在同一个网段\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--远程检查

 

Virtual SAN HCL DB up to date

Virtual SAN HCL health

VSAN内部集成了一个自动从internet上下载兼容性DB的功能，如果和个功能没联网，那么每次VSAN自动做health check的时候都会报告这个信息出来。\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--可忽略或者给予配置使其能够访问网络

 

Multicast assessment based on other checks

组播检查发现错误\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--检查网络是否存在问题并根据具体情况再决定是否收集日志检查，或者继续观察。

 

Overall health summary

只要health check触发任何一个alarm，都会被判断为检查结果有问题，比如HCL DB没联网。

 

![[Technology_ALL_VMware_分析案例_049_答复_ Vmware issue_(PROS)_SR944226940_001.png]]

 

 

BR.

Guoxun.

From: Huang, Zhenxiong

Sent: 2017年2月21日 10:43

To: Yin, Guoxun \<<guoxun_Yin@Dell.com>\>

Cc: Jacky Chiang \<<jacky.chiang@azurewave.com>\>; Chern, Jeff \<<Jeff_Chern@Dell.com>\>

Subject: 答复: Dell Technical Support (order:890326164 )

 

Dell - Internal Use - Confidential 

Hi Guoxun大哥

 

幫忙看下，感謝

 

如有任何問題再請提出並直接回覆此郵件，非常感謝。

 If any problems, please must let me know\...thanks\...

Best Regards

Zhenxiong,Huang\|黃楨雄

Enterprise Engineer

Enterprise Support Services

Dell \| Global Support and Deployment

 

发件人: Jacky Chiang \[[mailto:jacky.chiang@azurewave.com](mailto:jacky.chiang@azurewave.com)[\] ]

发送时间: Tuesday, February 21, 2017 10:39 AM

收件人: Huang, Zhenxiong \<[Zhenxiong_Huang@Dell.com](mailto:Zhenxiong_Huang@Dell.com)\>

抄送: Yin, Guoxun \<[guoxun_Yin@Dell.com](mailto:guoxun_Yin@Dell.com)\>

主题: RE: Dell Technical Support (order:890326164 )

 

Hi Zhenxiong

 

  看起來還是不斷有各種錯誤以及警告訊息出現。

 

![[Technology_ALL_VMware_分析案例_049_答复_ Vmware issue_(PROS)_SR944226940_002.png]]

 

 

海華科技股份有限公司  資訊暨智能工程部

AzureWave Technologies, Inc.

[TEL:886-2-55995599](tel:886-2-66289888) ext.5595

FAX:886-2-66289666

Mail: Jacky.Chiang[\@azurewave.com](mailto:JackyChiang@azurewave.com)

 

 

From: <Zhenxiong.Huang@dell.com> \[[mailto:Zhenxiong.Huang@dell.com](mailto:Zhenxiong.Huang@dell.com)\]

Sent: Monday, February 20, 2017 1:37 PM

To: Jacky Chiang \<<jacky.chiang@azurewave.com>\>

Cc: <Guoxun.Yin@dell.com>

Subject: 答复: Dell Technical Support (order:890326164 )

 

Dell - Internal Use - Confidential 

Hi jacky

 

不好意思打擾下，目前機器運行情況如何？期待您的回信

 

如有任何問題再請提出並直接回覆此郵件，非常感謝。

 If any problems, please must let me know\...thanks\...

Best Regards

Zhenxiong,Huang\|黃楨雄

Enterprise Engineer

Enterprise Support Services

Dell \| Global Support and Deployment

 

b.    Troubleshooting Steps

详细的诊断步骤:

维修记录: (单号/更换的部件/更换后的现象)

Bios/Driver/FW及存储控制器相关FW版本:

 

c.     Current status

客户公司名称:

业务影响:

升级的原因:Pending 在哪里？遇到了什么困难？需要（SERVER/OS/Network/Storage）SME L2帮忙做什么？

RM/TAM:

 

d.     Must Collect Logs

已收集的日志(请上传至SR下，若无法收集，请注明无法收集的原因): 

常见日志类型参考(根据实际情况获取相应日志)：

服务器(参考)：DSET log/system log/SOS Report log/IDRAC log/Capture failure等;

存储(参考)：MD/EQL/NAS/CML/DR/DL log;

 

 

 

Best Regards

Zhenxiong,Huang\|黃楨雄

Enterprise Engineer

Enterprise Support Services

Dell \| Global Support and Deployment

中文官方技術支援網站：[http://support.dell.com.cn](http://support.dell.com.cn/) 

DELL硬體技術支援聊天室：[http://support.dell.com.cn/chat](http://support.dell.com.cn/chat) 

戴爾企業產品技術支援微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell) 

How am I doing? Email my manager <Mars_Zeng@DELL.com> with any feedback. 

[Dell TechDirect](http://www.techdirect.com/) \| 戴爾線上報修門戶網站： 提供線上報修，自主部件派單以及線上管理報修事件

![[Technology_ALL_VMware_分析案例_049_答复_ Vmware issue_(PROS)_SR944226940_003.png]]

 

 

已使用 OneNote 创建。
