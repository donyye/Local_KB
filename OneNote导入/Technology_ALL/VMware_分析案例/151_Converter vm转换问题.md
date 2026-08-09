Converter vm转换问题

2023年3月14日

16:45

  -------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       RE: IC 162743425 Query
  发件人     Chen, Jesse
  收件人     Ouyang, Zhanglun; Ye, Dony
  抄送       Zhang, Ji Fu; lai, yongmei; Lin, Sybil
  发送时间   2023年3月14日 16:38
  -------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Hi  Ouyang,

 

我们lab测试无问题，供用户自行参考，如依旧存在异常，请用户寻求rocky linux的支持资源。 谢谢。

 

 

Hyper-V上安装的Rocky linux 8.6

 

![[Technology_ALL_VMware_分析案例_151_Converter vm转换问题_001.png]]

 

 

 

通过converter转换的参数设置：

![[Technology_ALL_VMware_分析案例_151_Converter vm转换问题_002.png]]

 

 

任务成功

![[Technology_ALL_VMware_分析案例_151_Converter vm转换问题_003.png]]

 

 

在esxi上调整客户机版本后开机，会出现引导失败：

 

![[Technology_ALL_VMware_分析案例_151_Converter vm转换问题_004.png]]

 

 

重新生成initramfs后正常：

 

![[Technology_ALL_VMware_分析案例_151_Converter vm转换问题_005.png]]

 

 

 

![[Technology_ALL_VMware_分析案例_151_Converter vm转换问题_006.png]]

 

![[Technology_ALL_VMware_分析案例_151_Converter vm转换问题_007.png]]

 

 

 

 [https://docs.vmware.com/en/vCenter-Converter-Standalone/6.3/vcenter-converter/GUID-377C42CF-B7EE-4D32-9BC5-D0648050B9AE.html](https://docs.vmware.com/en/vCenter-Converter-Standalone/6.3/vcenter-converter/GUID-377C42CF-B7EE-4D32-9BC5-D0648050B9AE.html)

 

![[Technology_ALL_VMware_分析案例_151_Converter vm转换问题_008.png]]

 

 

 

Internal Use - Confidential

From: Ouyang, Zhanglun \<Zhanglun_Ouyang@dell.com\>

Sent: Tuesday, March 14, 2023 4:08 PM

To: Ye, Dony; Chen, Jesse

Cc: Zhang, Ji Fu; lai, yongmei; Lin, Sybil

Subject: 回复: IC 162743425 Query

 

Dear Dony & Jesse

 

这个虚机迁移的case

 

客户反馈centos和RH迁移都成功了，但是rocky linux还是有问题

 

客户测试环境下hyper-v新建的rocky 8.6虚机，迁移到esxi后，dracut -f操作成功，但是依旧无法引导，/etc/fstab下客户有检查系统挂载的路程与UUID匹配

 

不知道是否还有其他操作建议？

 

 

 

 

 

Internal Use - Confidential

发件人: Lin, Sybil \<[Sybil_Lin@Dell.com](mailto:Sybil_Lin@Dell.com)\> 

发送时间: 2023年3月6日, 星期一11:41

收件人: Ye, Dony

抄送: Zhang, Ji Fu; Chen, Jesse; lai, yongmei; Ouyang, Zhanglun

主题: RE: IC 162743425 Query

 

Hi Dony,

 

运行 dracut -f 成功了，没有报错，Kernal成功更新至4.18。

 

我请客户自行从这个方向排查下。多谢！

 

 

 

Sybil Lin \| 林钧映

Service Account Manager

Dell Technologies \| Global Account Management Services

Office: +86-592-2054819

Mobile: +86-15105974529

[Sybil.Lin@dell.com](http://Sybil.Lin@dell.com)

 

 

Internal Use - Confidential

From: Ye, Dony \<<dony_ye@Dell.com>\>

Sent: 2023年3月6日11:37

To: Lin, Sybil

Cc: Zhang, Ji Fu; Chen, Jesse; lai, yongmei; Ouyang, Zhanglun

Subject: 回复: IC 162743425 Query

 

Hi, Sybil

 

运行 dracut -f 成功了吗？有错误吗？

 

从错误看是挂载的系统目录可能有出现异常的情况，这个需要到 /etc/fstab 里检查一下，看系统挂载的路程与UUID是否对的。

 

另外更加详细的信息保存在rdsosreport.txt记录里。

 

![[Technology_ALL_VMware_分析案例_151_Converter vm转换问题_009.png]]

 

 

Dony ye

Principal Engineer \| China - DE Compute & OS

Dell Technologies \| APJC ISG Support Services

Office +86 0592 818 7987 EXT. 8887987

[dony_ye@Dell.com](mailto:dony_ye@Dell.com)

 

 

Internal Use - Confidential

发件人: Lin, Sybil \<[Sybil_Lin@Dell.com](mailto:Sybil_Lin@Dell.com)\> 

发送时间: 2023年3月6日11:25

收件人: Ye, Dony

抄送: Zhang, Ji Fu; Chen, Jesse; lai, yongmei; Ouyang, Zhanglun

主题: IC 162743425 Query

 

Hi Dony,

客户尝试了dracut -f 重建initramfs，重启还是切换到紧急模式， 无法引导。这个有什么建议吗？感谢！

![[Technology_ALL_VMware_分析案例_151_Converter vm转换问题_010.jpg]]

 

![[Technology_ALL_VMware_分析案例_151_Converter vm转换问题_011.jpg]]

 

 

Sybil Lin \| 林钧映

Service Account Manager

Dell Technologies \| Global Account Management Services

Office: +86-592-2054819

Mobile: +86-15105974529

[Sybil.Lin@dell.com](http://Sybil.Lin@dell.com)

 

Internal Use - Confidential

From: Lin, Yongliang \<<Yongliang_Lin@Dell.com>\>

Sent: 2023年3月6日8:49

To: Chen, Jesse; Zhang, Ji Fu; Ye, Dony

Cc: CN XMN TS ENT L2 SME; Wang, Xing Fang; Yin, Guoxun; Lin, Sybil; Sun, Zhengwei; Li, Desheng; Tian, LianGui; Su, Yanmei; W, Chongyang; Zhang, Depei; Jiang, Gary; lai, yongmei; Wang, Zhirun; Ouyang, Zhanglun; Wu, Jianxiong; Wang, Jiarong; Wang, Jay

Subject: 回复: Jesse 3.6\-\--3.7休假case交接

 

Hi Jifu:

 

Help backup Jesse VM case .thanks

 

Thanks and Regards,

 

Yongliang Lin

PrincipalEngineer \| China - ISG Global Network and Compute

Dell Technologies \| APJC ISG Support Services

Office +86 0592 818 8780 EXT. 8888780

[Yongliang_lin@dell.com](mailto:Yongliang_lin@dell.com) 

 

 

Internal Use - Confidential

发件人: Chen, Jesse \<[Jesse_chen@Dell.com](mailto:Jesse_chen@Dell.com)\> 

发送时间: 2023年3月3日18:26

收件人: Lin, Yongliang

抄送: CN XMN TS ENT L2 SME; Wang, Xing Fang; Yin, Guoxun; Lin, Sybil; Sun, Zhengwei; Li, Desheng; Tian, LianGui; Su, Yanmei; W, Chongyang; Zhang, Depei; Jiang, Gary; lai, yongmei; Wang, Zhirun; Ouyang, Zhanglun; Wu, Jianxiong; Wang, Jiarong; Wang, Jay

主题: Jesse 3.6\-\--3.7休假case交接

 

Hi  Yongliang,

 

下周一至周二（3.6\-\--3.7）休假，如下case可能需要follow，有劳安排人员协助，谢谢！

 

==========================================

 

 

SR  161758486\|VMW\|video issue \|PROS \|BS7D493

IC 161758950

L1:jiarong,wang/ yanmei,su

SST:guoxun,yin

Symptom:3rd制图软件在vdi中光标定位问题

Current status:当前跟horizon view层面无关系，建议升级client版本看是否优化，亦请寻求3rd软件厂商的协助。

Next action: 等待更新测试，如有反馈请协助。

 

 

SR  162230198\|R740XD\|HD issue\|PSP\|HKCW3L3

IC 162348901

L1: chongyang,w

Symptom:vsan性能问题

Current status:当前vsan环境存在空间使用问题和兼容问题，已建议进行修复后确保最佳再观察性能是否异常。

Next action: 等待操作后测试，如有反馈请协助。

 

 

SR  163263613\|vmware\|snapshot issue\|CMR2D43（与下个case同客户\-\-\-\--维塔士）

IC 163263850

L1: jianxiong,wu/jay,wang

RM:liangui,tian

SAM:sybli,lin / zhengwei,sun

Symptom:虚拟机触发整合并失败，导致虚拟机宕机

Current status:备份作业产生的快照文件在整合过程中因快照系统机制问题导致空间占满，已提供分析和建议。

Next action: 如有反馈请协助。

 

 

SR  162652196 \|R6525 \|OS issue\|PSP\|FCYFPS3

IC 162743425

L1: ouyang,zhanglun

SAM:sybil,lin

Symptom:converter 转换hpyer-v虚拟机后无法启动

Current status:已提供lab测试后的结果和解决方案给到用户参考。

Next action: 如有反馈请协助。

 

 

SR  163075847\|Vmware\|DSW issue\|PROS\|2D8PMM2

IC 162974655

L1: gary,jiang/zhirun,wang

SST:guoxun,yin

Symptom:VDI访问间歇性掉线或无法连接

Current status:收集日志并升级SST处理中。

Next action: 如有反馈请协助。

 

 

SR  163280607\|R740\|HD issue\|PSP\|5JD7JF3

IC 163352926

L1:yongmei,lai

SAM:desheng,li

Symptom:vsan磁盘故障更换问题

Current status:已协助排查vsan环境无隐患并移除故障硬盘。

Next action: 如有反馈请协助。

 

 

SR  163352867\|R740\|OS issue\|PROS\|JYZMXG3

IC 163363529

L1: depei,zhang

Symptom:主机存储掉线

Current status:FC链路断开导致主机存储APD，已提供分析和建议。

Next action: 如有反馈请协助。

 

 

 

==========================================

 

 

 

 

Thanks and Regards,

 

Jesse Chen

Principal Engineer \| China - ISG Global Network and Compute

Dell Technologies \| APJC ISG Support Services

Office +86 0592 2047753

[jesse_chen@dell.com](mailto:ji_fu_zhang@Dell.com)

 

 

Internal Use - Confidential

 

已使用 OneNote 创建。
