ESXI CPU错误

2017年10月19日

13:30

- ::: 
    -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       Discussion about CPU MCE/IERR
    发件人     Han, Ruyang
    收件人     Wu, Yuan; Xu, Xiaofeng; Zhang, Tao; Lee, Aidy
    抄送       CN XMN TS ENT L2 SME; Wang, Xing Fang; Lim, Kiah Tat
    发送时间   2017年10月19日 12:03
    -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

   

  Dear IPS，

   

  本周二下午提过的关于CPU报错的问题，在此我简单汇总一下当前TS遇到的困惑，期待IPS同事的宝贵建议，以帮助TS提高判断类似故障的准确度。

   

  背景：

                  12G/13G Server上的CPU告警相比更早期Server明显上升且表现为不稳定症状，而CPU的cost很高，各级TS Manager对此问题非常关注。

                  备件部门抱怨TS给客户换下来的CPU大多都是正常的。

  TS已经在降低CPU派单方面做了大量工作，比如要求L1在CPU派单前必须完成一系列的前提条件，如BIOS更新及设置，需要L2再次检查确认，优先更换其它备件等。

   

  痛点： 

  目前我们没有诊断工具能提供较高的准确度，导致一些解决方案很难让客户/TAM/SALES信服。

  服务器毕竟是企业级产品，承载着客户的关键应用，无法提供root cause的情况下请客户接受不包含更换CPU的方案有很大阻力。

                 

                 

  新发现：

                  1，数个月前13G发布了某一版BIOS（现在官网找不到了）说明会屏蔽一部分不必要的CPU MCE error，

  2，IDRAC 2.40以后的版本会更改CPU IERR报警的描述和级别：

                  <https://kb.dell.com/infocenter/index?page=content&id=QNA44038&actp=SEARCH&viewlocale=en_US&searchid=1503470864355>

  ![[Technology_ALL_VMware_分析案例_074_ESXI CPU错误_001.png]]

   

  问题：

  1.  既然研发有办法确认某些CPU MCE/IERR是可以安全忽略的，是否可以透露一些判断方法，以帮助TS判断老版本的BIOS下CPU报错问题的准确度？
  2.  在一些case中发现ESXi PSOD，SEL和lifecycle log中没有任何CPU报错，但是PSOD画面指向CPU hardware error，只有更换CPU才能解决，如下面案例。如果存在误判是否有途径反馈给BIOS TEAM做改进？
  3.  iDRAC 2.40以前的CPU IERR会指出具体哪一个CPU报错，但是iDRAC 2.40以后就只有information级别的提示且不指向具体CPU。有些间歇性死机Case中这一条信息和故障时间完全吻合，我们不得不降级iDRAC后等下一次出故障再看是哪一颗CPU在报错。是否还有方法在不降级iDRAC的情况下抓到包含CPU位置的 "additional log"？

  ![[Technology_ALL_VMware_分析案例_074_ESXI CPU错误_002.png]]

   

   

  案例（仅做讨论参考，不是个例）：

   

                  这是我昨天刚处理完的Case，新出厂的机器装系统后PSOD画面指向CPU1 MCE error，开机后10分钟内可以复现。

  在老版本BIOS时都可以在硬件日志中看到相应的硬件报警，但是此Case中没有任何硬件报警。

  用DELL的高版本 OEM installation image重装系统后问题依旧，DSP上门后断电重插CPU和内存后问题依旧，更换CPU1后问题解决。

  故障时的TSR和vmsupport log在FTP,ID: 955342609：

  [http://fileexchangerinside.dell.com/tech/DefaultTechView.aspx](http://fileexchangerinside.dell.com/tech/DefaultTechView.aspx)

  ![[Technology_ALL_VMware_分析案例_074_ESXI CPU错误_003.png]]

   

   

                  另一个印象深刻的富士康的Case，PSOD指向CPU L0 Cache MCE error，但硬件没有任何报错，重装系统无效，更换CPU后解决。

  硬件有故障但是没有任何报警更容易使问题复杂化，这个Case TAM Director/AE Director/RM Manager都有介入。

               

  ![[Technology_ALL_VMware_分析案例_074_ESXI CPU错误_004.jpg]]

   

  Best Regards

  Ruyang Han

   

 

已使用 OneNote 创建。
