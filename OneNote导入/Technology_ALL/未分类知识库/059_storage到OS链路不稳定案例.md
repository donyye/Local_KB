storage到OS链路不稳定案例

Monday, December 12, 2016

9:19 AM

- ::: 
    -------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: SR# 940138035 & 940225732 (925872147) \| ST# 4KM8G42& 4J1N0Z1\| EU: 公牛集团 \-\-- update
    发件人     Pei, Jeff
    收件人     Xie, Tian; Ma, XiaoFei
    抄送       Xu, Lucas; Sun, LiBin; Chen, Jesse; Chen, Paul; Wang3, Mark; Han, Ruyang; Lin, Ken; Pan, Xinglong; Li, Coco; Shi, Leslie; Shi, Alex; Zhu, Steven BJ; Wang, Xing Fang; Xie, YuXuan; Chen, Johnson Lw; Zhou, MingMing; Yin, Guoxun; Ye, Dony; Ruan, Garuda; APJ Ent Resolution Managers China; Y, Henry; H, Gavin; Zhao, Lei
    发送时间   Friday, December 09, 2016 5:53 PM
    -------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

   

  Issue:

  CML FC port down/up very frequently.

   

  Analysis:

  Check PH log, FC port restarted caused by two HBA cards.

  LogOutPortID  Logout Fabric Port Type=14 failed for N_PortHandle=0x00AC PortId=0x0B0203 ioParameter0=0x0000000A \-- Restarting Device 

  LogOutPortID  Logout Fabric Port Type=14 failed for N_PortHandle=0x00B9 PortId=0x0B0303 ioParameter0=0x0000000A \-- Restarting Device 

  Check WWN with "portloginshow 2" and "portloginshow 3" in Brocade300 switch, found two WWN belong to Broadcom 57810 CNA card in blade 5 and blade 6, shutdown them issue fixed.

   

  Conclusion:

  CNA card in server caused FC port restarted, CNA card firmware had been updated and power on again, so far is good.

  Customer should keep monitor it.

   

  Comments:

  SCOS7.x could mitigate this issue, but not 100% sure, anyway this is HBA card issue not CML.

   

   

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

  FYI。经过一整天多位Ｌ２－Ｌ４各种ＳＭＥ的工程师及精英上门工程师的反复检查测试，找到问题根源问题解决。根据全程参与的Ｌ２－Ｌ４高级工程师提供的总结，更新大家如下。

   

  首先从客户处了解到7点多故障高发的背景是客户的员工7点半上班, 在这个时间段业务的IO比较高.

  ![[Technology_ALL_未分类知识库_059_storage到OS链路不稳定案例_001.png]]

   

   

   

  故障现象:

  之前的问题今早还在而且出现的更加频繁,FCOE链路更频繁的自动断开和恢复,和这一套系统的光纤交换机有级联关系的另一套系统也出现了链路降级的问题.

   

  故障分析:

           1,检查ESXi和RHEL的kernel log报错和以前一样没有新的线索, 硬件日志看物理链路没有中断.

           2,远程时发现SC8000存储的部分控制器FC端口链路频繁断开恢复.Ｌ４赵高工通过Phone Home日志发现这些FC端口在频繁重启, 并且发生在昨天晚上进行了MXL和HBA QMD8262-k的更新之后,

           在更新之前也有FC端口重启但是频率很低.

           3,从phone home中进一步发现从未出现过问题的slot5和slot6的HBA收到一些异常信息,关闭这两个刀片后一直持续了数个小时的所有故障都消失,(这两个刀片之前未出过问题而且用的是另一种固件已经较新的HBA BCOM57810。对此卡 昨天晚上没有安排重启更新固件。显然昨晚其他ＨＢＡ卡的固件升级促进了问题的被暴露)。在客户可以停机时精英工程师把HBA升级到VMware HCL要求的版本后再启动测试问题到目前为止一直再没有出现.

           <http://www.dell.com/support/home/us/en/04/Drivers/DriversDetails?driverId=WR5R5>

           <http://www.vmware.com/resources/compatibility/detail.php?deviceCategory=io&productid=41625&deviceCategory=io&details=1&releases=275&keyword=57810&page=1&display_interval=10&sortColumn=Partner&sortOrder=Asc>

   

  目前各设备及业务均运行正常。我们会继续和客户一起观察数日.

   

  感谢数位留名和没留名的Ｌ２到Ｌ４及一线的工程师多至几个通宵的支持！

   

   

  Jeff Pei

  Resolution Manager

  Dell \| Global Support & Development (GSD)

  Mobile：+86 181 1647 3020 / (China Toll Free): +86 950 4049 7843

  Voicemail: +86 592 818 7422

  [Jeff_Pei@Dell.com](mailto:Jeff_Pei@Dell.com)

   

   

   

   

   

   

  From: Xie, Tian

  Sent: Friday, December 09, 2016 1:50 AM

  To: Pei, Jeff \<Jeff_Pei@Dell.com\>; Ma, XiaoFei \<XiaoFei_Ma@Dell.com\>

  Cc: Xu, Lucas \<Lucas_Xu@DELL.com\>; Sun, LiBin \<LiBin_Sun@DELL.com\>; Chen, Jesse \<Jesse_chen@Dell.com\>; Chen, Paul \<Paul_Chen@DELL.com\>; Wang3, Mark \<mark_wang3@Dell.com\>; Han, Ruyang \<Ruyang_Han@Dell.com\>; Lin, Ken \<Ken_Lin@DELL.com\>; Pan, Xinglong \<Xinglong_Pan@Dell.com\>; Li, Coco \<Coco_Li@Dell.com\>; Shi, Leslie \<Leslie_Shi@Dell.com\>; Shi, Alex \<Alex_Shi@DELL.com\>; Zhu, Steven BJ \<Steven_BJ_Zhu@Dell.com\>; Wang, Xing Fang \<Xing_Fang_Wang@DELL.com\>; Xie, YuXuan \<YuXuan_Xie@Dell.com\>; Chen, Johnson Lw \<Johnson_Lw_Chen@DELL.com\>; Zhou, MingMing \<MingMing_Zhou@Dell.com\>; Yin, Guoxun \<guoxun_Yin@Dell.com\>; Ye, Dony \<dony_ye@Dell.com\>; Ruan, Garuda \<Garuda_Ruan@DELL.com\>; APJ Ent Resolution Managers China \<APJ_Ent_Resolution_Managers_China@Dell.com\>; Y, Henry \<Henry_y@DELL.com\>; H, Gavin \<Gavin_H@Dell.com\>

  Subject: RE: SR# 940138035 & 940225732 (925872147) \| ST# 4KM8G42& 4J1N0Z1\| EU: 公牛集团 \-\-- update

   

  Dears

              辛苦Jeff了，辛苦各位。按以往情况来看每天早上7点多是故障高发时间，还需要精英工程师明早再辛苦一下做现场保障，如果没问题，我们是否后续需要考虑其他虚机的固件版本升级？

   

   

   

   

   

  ::: 
  +-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
  | Regards                                                                                                                                                                       |                                                                                                      |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  |                                                                                                                                                                               |                                                                                                      |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 谢  天                                                                         | Tian Xie                                                                                             |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 客户经理                                                                                                                                                                                                  | Account executive                                                                                                                |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | [戴尔]\| 中国公共事业部与大企业部 |                                                                                                                                  |
  |                                                                                                                                                                                                           | office +86 571 8765 2899, Mobile: +86 13362877996, fax +86 571 8765 2898 |
  | 电话 +86 571 8765 2899 , [手机] +86 13362877996, 传真 +86 571 8765 2898                                                                                               | Customer feedback \| How am I doing? Please contact my manager [Leslie_Shi@Dell.com](mailto:Leslie_Shi@Dell.com)  |
  | 戴尔中国有限公司 中国杭州市杭大路15号嘉华国际商务中心1515室 310007                                                                                                                      |                                                                                                                                  |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 客户反馈\| 我表现如何？请联系我的经理 [Leslie_Shi@Dell.com](mailto:Leslie_Shi@Dell.com)                                                                  |                                                                                                                                  |
  +-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
  :::

   

   

  From: Pei, Jeff

  Sent: 2016年12月9日 1:47

  To: Ma, XiaoFei \<<XiaoFei_Ma@Dell.com>\>; Xie, Tian \<<Tian_Xie@Dell.com>\>

  Cc: Xu, Lucas \<<Lucas_Xu@DELL.com>\>; Sun, LiBin \<<LiBin_Sun@DELL.com>\>; Chen, Jesse \<<Jesse_chen@Dell.com>\>; Chen, Paul \<<Paul_Chen@DELL.com>\>; Wang3, Mark \<<mark_wang3@Dell.com>\>; Han, Ruyang \<<Ruyang_Han@Dell.com>\>; Lin, Ken \<<Ken_Lin@DELL.com>\>; Pan, Xinglong \<<Xinglong_Pan@Dell.com>\>; Li, Coco \<<Coco_Li@Dell.com>\>; Shi, Leslie \<<Leslie_Shi@Dell.com>\>; Shi, Alex \<<Alex_Shi@DELL.com>\>; Zhu, Steven BJ \<<Steven_BJ_Zhu@Dell.com>\>; Wang, Xing Fang \<<Xing_Fang_Wang@DELL.com>\>; Xie, YuXuan \<<YuXuan_Xie@Dell.com>\>; Chen, Johnson Lw \<<Johnson_Lw_Chen@DELL.com>\>; Zhou, MingMing \<<MingMing_Zhou@Dell.com>\>; Yin, Guoxun \<<guoxun_Yin@Dell.com>\>; Ye, Dony \<<dony_ye@Dell.com>\>; Ruan, Garuda \<<Garuda_Ruan@DELL.com>\>; APJ Ent Resolution Managers China \<<APJ_Ent_Resolution_Managers_China@Dell.com>\>; Y, Henry \<<Henry_y@DELL.com>\>; H, Gavin \<<Gavin_H@Dell.com>\>

  Subject: RE: SR# 940138035 & 940225732 (925872147) \| ST# 4KM8G42& 4J1N0Z1\| EU: 公牛集团 \-\-- update

   

  Dell - Internal Use - Confidential 

  FYI。精英工程师下午3：30左右到达客户现场工作到近午夜，操作及结果小结如下：

   

  交换机方面：

  两台MXL交换机固件升级成功（到9.9.0.0P18）；

  SYSLOG服务器设好；

  生成树优先级改为61440；

   

  刀片机方面：

  7、8两个Linux刀片机系统为redhat 6.5。因10版本的驱动是适用redhat 6.6以上版本。所以重新下载安装了一个较低的9.0的驱动版本。6台ESXi刀片机的qla2xxx的驱动也已经更新。

  刀箱的qlogic的卡固件都有更新到：02.10.38。驱动更新为：8.07.00.17.06.0

   

  客户业务方面：

  作交换机切换测试时发现客户的运行在7、8两个Linux刀片机上的Oracle Rac数据库并不能做冗余切换。怀疑配置有需要改正的地方。客户连夜找来他们数据库的工程师一起重新调试。调试结果暂时未知。但精英工程师离开现场时，客户业务都已恢复正常。

   

  戴尔精英工程师今晚住在当地，和客户一起观察一白天再离开。已嘱咐离开前再全面收集一遍日志发来。

   

   

  Jeff Pei

  Resolution Manager

  Dell \| Global Support & Development (GSD)

  Mobile：+86 181 1647 3020 / (China Toll Free): +86 950 4049 7843

  Voicemail: +86 592 818 7422

  [Jeff_Pei@Dell.com](mailto:Jeff_Pei@Dell.com)

   

   

   

  From: Pei, Jeff

  Sent: Thursday, December 08, 2016 10:46 AM

  To: Ma, XiaoFei \<<XiaoFei_Ma@Dell.com>\>; Xie, Tian \<<Tian_Xie@Dell.com>\>

  Cc: Xu, Lucas \<<Lucas_Xu@DELL.com>\>; Sun, LiBin \<<LiBin_Sun@DELL.com>\>; Chen, Jesse \<<Jesse_chen@Dell.com>\>; Chen, Paul \<<Paul_Chen@DELL.com>\>; Wang3, Mark \<<Mark_Wang3@Dell.com>\>; Han, Ruyang \<<Ruyang_Han@Dell.com>\>; Lin, Ken \<<Ken_Lin@DELL.com>\>; Pan, Xinglong \<<Xinglong_Pan@Dell.com>\>; Li, Coco \<<Coco_Li@Dell.com>\>; Shi, Leslie \<<Leslie_Shi@Dell.com>\>; Shi, Alex \<<Alex_Shi@DELL.com>\>; Zhu, Steven BJ \<<Steven_BJ_Zhu@Dell.com>\>; Wang, Xing Fang \<<Xing_Fang_Wang@DELL.com>\>; Xie, YuXuan \<<YuXuan_Xie@Dell.com>\>; Chen, Johnson Lw \<<Johnson_Lw_Chen@DELL.com>\>; Zhou, MingMing \<<MingMing_Zhou@Dell.com>\>

  Subject: RE: SR# 940138035 & 940225732 (925872147) \| ST# 4KM8G42& 4J1N0Z1\| EU: 公牛集团 \-\-- update

   

  Dell - Internal Use - Confidential 

  FYI。客户告知AE改变主意今天白天可以就上门做原定周六才作的各种操作。RM，网络TS和L2紧急联系原安排的精英工程师李工和精英协调，重新安排另一个精英工程师黄工今天上门。具体时间正在和客户及黄工确认中。

   

   

  Jeff Pei

  Resolution Manager

  Dell \| Global Support & Development (GSD)

  Mobile：+86 181 1647 3020 / (China Toll Free): +86 950 4049 7843

  Voicemail: +86 592 818 7422

  [Jeff_Pei@Dell.com](mailto:Jeff_Pei@Dell.com)

   

   

   

   

  From: Pei, Jeff

  Sent: Thursday, December 08, 2016 9:45 AM

  To: Ma, XiaoFei \<<XiaoFei_Ma@Dell.com>\>; Xie, Tian \<<Tian_Xie@Dell.com>\>

  Cc: Xu, Lucas \<<Lucas_Xu@DELL.com>\>; Sun, LiBin \<<LiBin_Sun@DELL.com>\>; Chen, Jesse \<<Jesse_chen@Dell.com>\>; Chen, Paul \<<Paul_Chen@DELL.com>\>; Wang3, Mark \<<Mark_Wang3@Dell.com>\>; Han, Ruyang \<<Ruyang_Han@Dell.com>\>; Lin, Ken \<<Ken_Lin@DELL.com>\>; Pan, Xinglong \<<Xinglong_Pan@Dell.com>\>; Li, Coco \<<Coco_Li@Dell.com>\>; Shi, Leslie \<<Leslie_Shi@Dell.com>\>; Shi, Alex \<<Alex_Shi@DELL.com>\>; Zhu, Steven BJ \<<Steven_BJ_Zhu@Dell.com>\>; Wang, Xing Fang \<<Xing_Fang_Wang@DELL.com>\>; Xie, YuXuan \<<YuXuan_Xie@Dell.com>\>; Chen, Johnson Lw \<<Johnson_Lw_Chen@DELL.com>\>; Zhou, MingMing \<<MingMing_Zhou@Dell.com>\>

  Subject: SR# 940138035 & 940225732 (925872147) \| ST# 4KM8G42& 4J1N0Z1\| EU: 公牛集团 \-\-- update

   

  Dell - Internal Use - Confidential 

  FYI。

   

  今早7点左右客户又发现数台交换机出现网路中断现象并告知AE。RM，TAM，AE，网络TS及硬件网络两位L2 SME立即共同con-call客户，指导客户配合搭建远程链接后，网络L2远程登陆CMC及交换机管理界面，仅仅重启相关网络端口后所有链接及业务恢复。因为根除此问题需要完成按客户预约的本周六已确定安排了精英工程师上门才能做的各个操作（包括升级交换机固件），今明后三天如重新发生类似情况，已嘱咐客户暂时用同样办法处理并提供了TS的直接联系方式。其他后续进展我会及时更新大家。

   

   

  Jeff Pei

  Resolution Manager

  Dell \| Global Support & Development (GSD)

  Mobile：+86 181 1647 3020 / (China Toll Free): +86 950 4049 7843

  Voicemail: +86 592 818 7422

  [Jeff_Pei@Dell.com](mailto:Jeff_Pei@Dell.com)

   

   

   

   

   

  From: Pei, Jeff

  Sent: Wednesday, December 07, 2016 11:23 AM

  To: Ma, XiaoFei \<<XiaoFei_Ma@Dell.com>\>; Xie, Tian \<<Tian_Xie@Dell.com>\>; Chen, Johnson Lw \<<Johnson_Lw_Chen@DELL.com>\>

  Cc: Xu, Lucas \<<Lucas_Xu@DELL.com>\>; Sun, LiBin \<<LiBin_Sun@DELL.com>\>; Chen, Jesse \<<Jesse_chen@Dell.com>\>; Chen, Paul \<<Paul_Chen@DELL.com>\>; Wang3, Mark \<<Mark_Wang3@Dell.com>\>; Han, Ruyang \<<Ruyang_Han@Dell.com>\>; Lin, Ken \<<Ken_Lin@DELL.com>\>; Pan, Xinglong \<<Xinglong_Pan@Dell.com>\>; Li, Coco \<<Coco_Li@Dell.com>\>; Shi, Leslie \<<Leslie_Shi@Dell.com>\>; Shi, Alex \<<Alex_Shi@DELL.com>\>; Zhu, Steven BJ \<<Steven_BJ_Zhu@Dell.com>\>; Wang, Xing Fang \<<Xing_Fang_Wang@DELL.com>\>; Xie, YuXuan \<<YuXuan_Xie@Dell.com>\>; Zhou, MingMing \<<MingMing_Zhou@Dell.com>\>

  Subject: RE: 重要！！！！！RE: SR# 940138035 \| COMPELLENT SC8000 \|ST# 4KM8G42 \| EU: 公牛集团 \-\-- update

   

  Dell - Internal Use - Confidential 

   

  FYI。刚才和客户电话会议核实。客户数据库业务没有中断，只是客户管理数据库的链接还有问题。而且客户选择只能到周末才要工程师去处理操作。所以通知了精英工程师协调安排周六上门。具体上门精英工程师正在确认中。

   

  另外客户的OS什么版本及是否为戴尔OEM客户还不能确定。但客户按L2电话中指示已在收OS日志。发来后就可确认版本。

   

  （不好意思刚才邮箱满了一直发不出去，用RM_CN发的）。

   

   

  Jeff Pei

  Resolution Manager

  Dell \| Global Support & Development (GSD)

  Mobile：+86 181 1647 3020 / (China Toll Free): +86 950 4049 7843

  Voicemail: +86 592 818 7422

  [Jeff_Pei@Dell.com](mailto:Jeff_Pei@Dell.com)

   

   

  From: RM_CN

  Sent: Wednesday, December 07, 2016 10:40 AM

  To: Ma, XiaoFei \<<XiaoFei_Ma@Dell.com>\>; Xie, Tian \<<Tian_Xie@Dell.com>\>; Chen, Johnson Lw \<<Johnson_Lw_Chen@DELL.com>\>

  Cc: Xu, Lucas \<<Lucas_Xu@DELL.com>\>; Sun, LiBin \<<LiBin_Sun@DELL.com>\>; Chen, Jesse \<<Jesse_chen@Dell.com>\>; Chen, Paul \<<Paul_Chen@DELL.com>\>; Wang3, Mark \<<Mark_Wang3@Dell.com>\>; Han, Ruyang \<<Ruyang_Han@Dell.com>\>; Lin, Ken \<<Ken_Lin@DELL.com>\>; Pan, Xinglong \<<Xinglong_Pan@Dell.com>\>; Li, Coco \<<Coco_Li@Dell.com>\>; Shi, Leslie \<<Leslie_Shi@Dell.com>\>; Shi, Alex \<<Alex_Shi@DELL.com>\>; Zhu, Steven BJ \<<Steven_BJ_Zhu@Dell.com>\>; Wang, Xing Fang \<<Xing_Fang_Wang@DELL.com>\>; Xie, YuXuan \<<YuXuan_Xie@Dell.com>\>; Zhou, MingMing \<<MingMing_Zhou@Dell.com>\>

  Subject: RE: 重要！！！！！RE: SR# 940138035 \| COMPELLENT SC8000 \|ST# 4KM8G42 \| EU: 公牛集团 \-\-- update

   

  Dell - Internal Use - Confidential 

  Dell - Internal Use - Confidential 

  Dell - Internal Use - Confidential 

  Dell - Internal Use - Confidential 

   FYI.  刚才RM，AE， 硬件SEM L2， 网络SME L2 开紧急电话会议讨论。提出一些初步整体建议的发现如升级落后很久的固件和驱动。另外因为客户数据库受影响是新出现的问题需要另外搜集数据库服务器的OS日志请客户尽量配合。AE提供客户所有相关设备ST清单及订单资料并追踪最初GETS部署资料。以便确定是否戴尔软件SME介入还是请客户联系Linux厂家及VMware协助分析。RM申请精英上门工程师中。如果精英工程师来不及，会先派DSP立即上门。我会跟进更新各位。

   

   

   

  Jeff Pei

  Resolution Manager

  Dell \| Global Support & Development (GSD)

  Mobile：+86 181 1647 3020 / (China Toll Free): +86 950 4049 7843

  Voicemail: +86 592 818 7422

  [Jeff_Pei@Dell.com](mailto:Jeff_Pei@Dell.com)

   

  From: Ma, XiaoFei

  Sent: Wednesday, December 07, 2016 10:28 AM

  To: Xie, Tian \<<Tian_Xie@Dell.com>\>; Chen, Johnson Lw \<<Johnson_Lw_Chen@DELL.com>\>; Pei, Jeff \<<Jeff_Pei@Dell.com>\>

  Cc: Xu, Lucas \<<Lucas_Xu@DELL.com>\>; Sun, LiBin \<<LiBin_Sun@DELL.com>\>; Chen, Jesse \<<Jesse_chen@Dell.com>\>; Chen, Paul \<<Paul_Chen@DELL.com>\>; Wang3, Mark \<<mark_wang3@Dell.com>\>; Han, Ruyang \<<Ruyang_Han@Dell.com>\>; Lin, Ken \<<Ken_Lin@DELL.com>\>; Pan, Xinglong \<<Xinglong_Pan@Dell.com>\>; Li, Coco \<<Coco_Li@Dell.com>\>; Shi, Leslie \<<Leslie_Shi@Dell.com>\>; Shi, Alex \<<Alex_Shi@DELL.com>\>; Zhu, Steven BJ \<<Steven_BJ_Zhu@Dell.com>\>; Wang, Xing Fang \<<Xing_Fang_Wang@DELL.com>\>; Xie, YuXuan \<<YuXuan_Xie@Dell.com>\>; Zhou, MingMing \<<MingMing_Zhou@Dell.com>\>

  Subject: 答复: 重要！！！！！RE: SR# 940138035 \| COMPELLENT SC8000 \|ST# 4KM8G42 \| EU: 公牛集团 \-\-- update

   

  Dell - Internal Use - Confidential 

  Dear  Jeff,

   

  如con call讨论，多谢各位同事分析制定action plan。

   

  提醒一下，

  1.  以往的case中出现几次未能在约定时间联系客户的情况，请尽量避免；
  2.  进展及时update给客户/ AE/ TAM。 

   

  非常感谢。

   

   

  Best Regards

  Xiaofei

   

  发件人: Xie, Tian 

  发送时间: 2016年12月7日 9:38

  收件人: Chen, Johnson Lw \<[Johnson_Lw_Chen@DELL.com](mailto:Johnson_Lw_Chen@DELL.com)\>; Pei, Jeff \<[Jeff_Pei@Dell.com](mailto:Jeff_Pei@Dell.com)\>; Ma, XiaoFei \<[XiaoFei_Ma@Dell.com](mailto:XiaoFei_Ma@Dell.com)\>

  抄送: Xu, Lucas \<[Lucas_Xu@DELL.com](mailto:Lucas_Xu@DELL.com)\>; Sun, LiBin \<[LiBin_Sun@DELL.com](mailto:LiBin_Sun@DELL.com)\>; Chen, Jesse \<[Jesse_chen@Dell.com](mailto:Jesse_chen@Dell.com)\>; Chen, Paul \<[Paul_Chen@DELL.com](mailto:Paul_Chen@DELL.com)\>; Wang3, Mark \<[mark_wang3@Dell.com](mailto:mark_wang3@Dell.com)\>; Han, Ruyang \<[Ruyang_Han@Dell.com](mailto:Ruyang_Han@Dell.com)\>; Lin, Ken \<[Ken_Lin@DELL.com](mailto:Ken_Lin@DELL.com)\>; Pan, Xinglong \<[Xinglong_Pan@Dell.com](mailto:Xinglong_Pan@Dell.com)\>; Li, Coco \<[Coco_Li@Dell.com](mailto:Coco_Li@Dell.com)\>; Shi, Leslie \<[Leslie_Shi@Dell.com](mailto:Leslie_Shi@Dell.com)\>; Shi, Alex \<[Alex_Shi@DELL.com](mailto:Alex_Shi@DELL.com)\>; Zhu, Steven BJ \<[Steven_BJ_Zhu@Dell.com](mailto:Steven_BJ_Zhu@Dell.com)\>; Wang, Xing Fang \<[Xing_Fang_Wang@DELL.com](mailto:Xing_Fang_Wang@DELL.com)\>; Xie, YuXuan \<[YuXuan_Xie@Dell.com](mailto:YuXuan_Xie@Dell.com)\>; Zhou, MingMing \<[MingMing_Zhou@Dell.com](mailto:MingMing_Zhou@Dell.com)\>

  主题: RE: 重要！！！！！RE: SR# 940138035 \| COMPELLENT SC8000 \|ST# 4KM8G42 \| EU: 公牛集团 \-\-- update

   

  Dear Jeff

                    烦请尽快确认昨天参与数据抓包及分析工程师，看是否有必要concall，我们需要了解按要求抓了两次包后的分析结果到底是什么，谢谢！ 

   

   

   

   

   

  ::: 
  +-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
  | Regards                                                                                                                                                                       |                                                                                                      |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  |                                                                                                                                                                               |                                                                                                      |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 谢  天                                                                         | Tian Xie                                                                                             |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 客户经理                                                                                                                                                                                                  | Account executive                                                                                                                |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | [戴尔]\| 中国公共事业部与大企业部 |                                                                                                                                  |
  |                                                                                                                                                                                                           | office +86 571 8765 2899, Mobile: +86 13362877996, fax +86 571 8765 2898 |
  | 电话 +86 571 8765 2899 , [手机 ]+86 13362877996, 传真 +86 571 8765 2898                                                                                                           | Customer feedback \| How am I doing? Please contact my manager [Leslie_Shi@Dell.com](mailto:Leslie_Shi@Dell.com)  |
  | 戴尔中国有限公司 中国杭州市杭大路15号嘉华国际商务中心1515室 310007                                                                                                                      |                                                                                                                                  |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 客户反馈\| 我表现如何？请联系我的经理 [Leslie_Shi@Dell.com](mailto:Leslie_Shi@Dell.com)                                                                  |                                                                                                                                  |
  +-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
  :::

   

   

  From: Chen, Johnson Lw

  Sent: 2016年12月7日 9:36

  To: Xie, Tian \<<Tian_Xie@Dell.com>\>; Pei, Jeff \<<Jeff_Pei@Dell.com>\>; Ma, XiaoFei \<<XiaoFei_Ma@Dell.com>\>

  Cc: Xu, Lucas \<<Lucas_Xu@DELL.com>\>; Sun, LiBin \<<LiBin_Sun@DELL.com>\>; Chen, Jesse \<<Jesse_chen@Dell.com>\>; Chen, Paul \<<Paul_Chen@DELL.com>\>; Wang3, Mark \<<mark_wang3@Dell.com>\>; Han, Ruyang \<<Ruyang_Han@Dell.com>\>; Lin, Ken \<<Ken_Lin@DELL.com>\>; Pan, Xinglong \<<Xinglong_Pan@Dell.com>\>; Li, Coco \<<Coco_Li@Dell.com>\>; Shi, Leslie \<<Leslie_Shi@Dell.com>\>; Shi, Alex \<<Alex_Shi@DELL.com>\>; Zhu, Steven BJ \<<Steven_BJ_Zhu@Dell.com>\>; Wang, Xing Fang \<<Xing_Fang_Wang@DELL.com>\>; Xie, YuXuan \<<YuXuan_Xie@Dell.com>\>; Zhou, MingMing \<<MingMing_Zhou@Dell.com>\>

  Subject: RE: 重要！！！！！RE: SR# 940138035 \| COMPELLENT SC8000 \|ST# 4KM8G42 \| EU: 公牛集团 \-\-- update

   

  Hi Jeff,

  As Lucas is on leave today, pls work with our OOO agent Mingming to fix this issue first.

  Tks.

   

  Johnson_Lw_Chen

  Manager of CCC Enterprise ProSupport Storage

  Dell \| Global Support and Deployment

  office +86-592-818-0330

  Mobile +86- 13959222928

  Visit us at [http://support.ap.dell.com](http://support.ap.dell.com/)

   

  From: Xie, Tian

  Sent: Wednesday, December 07, 2016 9:33

  To: Pei, Jeff \<<Jeff_Pei@Dell.com>\>; Ma, XiaoFei \<<XiaoFei_Ma@Dell.com>\>

  Cc: Xu, Lucas \<<Lucas_Xu@DELL.com>\>; Sun, LiBin \<<LiBin_Sun@DELL.com>\>; Chen, Jesse \<<Jesse_chen@Dell.com>\>; Chen, Paul \<<Paul_Chen@DELL.com>\>; Wang3, Mark \<<mark_wang3@Dell.com>\>; Han, Ruyang \<<Ruyang_Han@Dell.com>\>; Lin, Ken \<<Ken_Lin@DELL.com>\>; Pan, Xinglong \<<Xinglong_Pan@Dell.com>\>; Li, Coco \<<Coco_Li@Dell.com>\>; Shi, Leslie \<<Leslie_Shi@Dell.com>\>; Shi, Alex \<<Alex_Shi@DELL.com>\>; Zhu, Steven BJ \<<Steven_BJ_Zhu@Dell.com>\>; Wang, Xing Fang \<<Xing_Fang_Wang@DELL.com>\>; Chen, Johnson Lw \<<Johnson_Lw_Chen@DELL.com>\>; Xie, YuXuan \<<YuXuan_Xie@Dell.com>\>

  Subject: 重要！！！！！RE: SR# 940138035 \| COMPELLENT SC8000 \|ST# 4KM8G42 \| EU: 公牛集团 \-\-- update

   

   

  Dears ALL

                            今早接到客户电话由于存储网络不稳定已经导致ORACLE RAC时效，现在数据库处于瘫痪状态，生产系统停止运行。用户12月5日报修至今经历多次报修，昨晚按照TS要求再次抓包，在反复确认要求当晚得到分析结果判断定位故障原因的情况下至今没有得到反馈结果，烦请尽快告知后续处理方案，我们必须在10点前给到用户解决办法，请务必给予重视立即处理！

                            该客户是我们top account，本周刚下单存储扩展柜，现在因为这事客户威胁要按照DELL存储规定退货，恳请各位老板一定帮忙给予资源support，万分感谢！

   

   

   

   

   

  ::: 
  +-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
  | Regards                                                                                                                                                                       |                                                                                                      |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  |                                                                                                                                                                               |                                                                                                      |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 谢  天                                                                         | Tian Xie                                                                                             |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 客户经理                                                                                                                                                                                                  | Account executive                                                                                                                |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | [戴尔]\| 中国公共事业部与大企业部 |                                                                                                                                  |
  |                                                                                                                                                                                                           | office +86 571 8765 2899, Mobile: +86 13362877996, fax +86 571 8765 2898 |
  | 电话 +86 571 8765 2899 , [手机] +86 13362877996, 传真 +86 571 8765 2898                                                                                               | Customer feedback \| How am I doing? Please contact my manager [Leslie_Shi@Dell.com](mailto:Leslie_Shi@Dell.com)  |
  | 戴尔中国有限公司 中国杭州市杭大路15号嘉华国际商务中心1515室 310007                                                                                                                      |                                                                                                                                  |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 客户反馈\| 我表现如何？请联系我的经理 [Leslie_Shi@Dell.com](mailto:Leslie_Shi@Dell.com)                                                                  |                                                                                                                                  |
  +-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
  :::

   

   

  From: Pei, Jeff

  Sent: 2016年12月6日 14:56

  To: Xie, Tian \<<Tian_Xie@Dell.com>\>; Ma, XiaoFei \<<XiaoFei_Ma@Dell.com>\>

  Cc: Xu, Lucas \<<Lucas_Xu@DELL.com>\>; Sun, LiBin \<<LiBin_Sun@DELL.com>\>; Chen, Jesse \<<Jesse_chen@Dell.com>\>; Li, JiLin \<<JiLin_Li@DELL.com>\>; APJ Ent Resolution Managers China \<<APJ_Ent_Resolution_Managers_China@Dell.com>\>; Wang3, Mark \<<mark_wang3@Dell.com>\>; Han, Ruyang \<<Ruyang_Han@Dell.com>\>; Lin, Ken \<<Ken_Lin@DELL.com>\>; Pan, Xinglong \<<Xinglong_Pan@Dell.com>\>

  Subject: RE: SR# 940138035 \| COMPELLENT SC8000 \|ST# 4KM8G42 \| EU: 公牛集团 \-\-- update

   

  Dell - Internal Use - Confidential 

  Tian，

   

  正好需要帮忙和客户沟通此事。从昨晚DSP重启刀片机解决了客户的链路问题后，今天TS已升级相关的数名从存储到机箱到网络的2-3线工程师为的是给客户分析问题原因。经过几位工程师的逐步排查，基本断定问题属于网络部分。但因为昨晚收集的网络日志不够详细完全，今天需要继续收集一下几个日志或信息：

   

  1）对A1/A2抓一份几个命令行输出，包括查看MXL Firmware 版本。

  2）向客户了解MXL Po1上联的是什么交换机。

   

  刚才网络组的TS和客户电话联系，客户不愿意配合，还是需要戴尔再次派工程师上门。

   

  需要客户理解，客户现有的戴尔支持服务都不包括上门诊断服务，或者说是需要以客户配合戴尔工程师包括远程诊断为前提的。昨天晚上TS已经破例为客户提供了一次不完全诊断的上门服务。但这种破例友情支持是不能一而再的提供的。需要客户理解并配合戴尔工程师。谢谢。

   

   

  Jeff Pei

  Resolution Manager

  Dell \| Global Support & Development (GSD)

  Mobile：+86 181 1647 3020 / (China Toll Free): +86 950 4049 7843

  Voicemail: +86 592 818 7422

  [Jeff_Pei@Dell.com](mailto:Jeff_Pei@Dell.com)

   

   

   

   

  From: Xie, Tian

  Sent: Tuesday, December 06, 2016 2:31 PM

  To: Pei, Jeff \<<Jeff_Pei@Dell.com>\>; Ma, XiaoFei \<<XiaoFei_Ma@Dell.com>\>

  Cc: Xu, Lucas \<<Lucas_Xu@DELL.com>\>; Sun, LiBin \<<LiBin_Sun@DELL.com>\>; Chen, Jesse \<<Jesse_chen@Dell.com>\>; Li, JiLin \<<JiLin_Li@DELL.com>\>; APJ Ent Resolution Managers China \<<APJ_Ent_Resolution_Managers_China@Dell.com>\>

  Subject: RE: SR# 940138035 \| COMPELLENT SC8000 \|ST# 4KM8G42 \| EU: 公牛集团 \-\-- update

   

  Dears

                        客户在开会现场微信联系我，说有同时电话给他要抓包，请问这个case目前什么进度了？有什么需要我们做的？

  谢谢！

   

   

   

   

   

  ::: 
  +-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
  | Regards                                                                                                                                                                       |                                                                                                      |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  |                                                                                                                                                                               |                                                                                                      |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 谢  天                                                                         | Tian Xie                                                                                             |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 客户经理                                                                                                                                                                                                  | Account executive                                                                                                                |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | [戴尔]\| 中国公共事业部与大企业部 |                                                                                                                                  |
  |                                                                                                                                                                                                           | office +86 571 8765 2899, Mobile: +86 13362877996, fax +86 571 8765 2898 |
  | 电话 +86 571 8765 2899 , [手机] +86 13362877996, 传真 +86 571 8765 2898                                                                                               | Customer feedback \| How am I doing? Please contact my manager [Leslie_Shi@Dell.com](mailto:Leslie_Shi@Dell.com)  |
  | 戴尔中国有限公司 中国杭州市杭大路15号嘉华国际商务中心1515室 310007                                                                                                                      |                                                                                                                                  |
  |                                                                                                                                                                                                           |                                                                                                                                  |
  | 客户反馈\| 我表现如何？请联系我的经理 [Leslie_Shi@Dell.com](mailto:Leslie_Shi@Dell.com)                                                                  |                                                                                                                                  |
  +-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------------------------------------------------------------------------------------------------+
  :::

   

   

  From: Pei, Jeff

  Sent: 2016年12月6日 1:02

  To: Ma, XiaoFei \<<XiaoFei_Ma@Dell.com>\>; Xie, Tian \<<Tian_Xie@Dell.com>\>

  Cc: Xu, Lucas \<<Lucas_Xu@DELL.com>\>; Sun, LiBin \<<LiBin_Sun@DELL.com>\>; Chen, Jesse \<<Jesse_chen@Dell.com>\>; Li, JiLin \<<JiLin_Li@DELL.com>\>; APJ Ent Resolution Managers China \<<APJ_Ent_Resolution_Managers_China@Dell.com>\>

  Subject: SR# 940138035 \| COMPELLENT SC8000 \|ST# 4KM8G42 \| EU: 公牛集团 \-\-- update

   

  Dell - Internal Use - Confidential 

  FYI.

   

  DSP Call-in的TS的记录DSP一小时前离开客户现场时客户的一切问题都已解决。但还没有看到WebCST上有最新的PhoneHome上传。TS记录会在今天白天上班后升级L2分析故障原因。到时我再更新大家。

   

   

  Jeff Pei

  Resolution Manager

  Dell \| Global Support & Development (GSD)

  Mobile：+86 181 1647 3020 / (China Toll Free): +86 950 4049 7843

  Voicemail: +86 592 818 7422

  [Jeff_Pei@Dell.com](mailto:Jeff_Pei@Dell.com)

   

   

 

已使用 OneNote 创建。
