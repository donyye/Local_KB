紫屏case，还没解决

Monday, August 22, 2016

2:21 PM

- ::: 
    -------------------------------------- ----------------------------------------------------------
    主题       RE: OOO N1\|SR#934454249
    发件人     Lai, Flying
    收件人     Zhan, Yanbin
    抄送       CN ENT ProSupport; Lin, Qiang; hu, Changping; Sun, LiBin
    发送时间   Saturday, August 20, 2016 6:31 PM
    附件       \<\<操作之前的TSR.zip\>\>
    -------------------------------------- ----------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

  Yanbin

   

  麻烦帮忙继续跟进下，谢谢

   

  问题现象：

  VMware 5.5 U3（[http://downloads.dell.com/FOLDER03737368M/1/VMware-VMvisor-Installer-5.5.0.update03-3568722.x86_64-Dell_Customized-A06.iso](http://downloads.dell.com/FOLDER03737368M/1/VMware-VMvisor-Installer-5.5.0.update03-3568722.x86_64-Dell_Customized-A06.iso) ）部署Windows2003，格式化第二个分区的时候死机，VMware主机紫屏如下：

  ![[Technology_ALL_VMware_分析案例_034_紫屏case，还没解决_001.png]]

   

  远程检查结果如下：

  1.  收集TSR log（见附件），硬件报错如下：

  ![[Technology_ALL_VMware_分析案例_034_紫屏case，还没解决_002.jpg]]

  1.  驱动已经是正确的megaraid_perc9，当前固件为25.4.0.0015
  2.  更新与25.4.0.0015对应的驱动版本6.903.55.00，一样紫屏报错
  3.  BIOS为2.1.5（最新的为2.1.7），iDRAC为2.30.30.30（已经是最新）
  4.  根据KB：SLN300647 做了如下动作：

  <!-- -->

  a.  更新BIOS到最新的2.1.7，并且调整System Profile Settings为Performance
  b.  更新阵列卡驱动为：6.902.73.00               固件为：25.3.0.0015
  c.  再进行测试，一样紫屏报错
  d.  重新部署Windows2003之后再进行测试，还是一样紫屏报错

  <!-- -->

  1.  据客户反馈测试了四台R730XD，就这一台有问题，目前获取到一台正常R730XD的驱动信息如下

  ![[Technology_ALL_VMware_分析案例_034_紫屏case，还没解决_003.jpg]]

  1.  客户目前无法连到正常R730XD的iDRAC上，无法获取到正常R730XD的TSR/TTY log，无法进行对比。
  2.  目前等待客户收集最新的TSR/TTY log
  3.  刚刚一直联系客户，联系不上，不确定客户晚上是否还要继续处理。

   

  麻烦帮忙继续联系客户确认最新的TSR/TTY log，谢谢

   

  Best Regards!

  Flying Lai

  Enterprise Engineer

  Dell \| Enterprise Support Services

   

  \-\-\-\--Original Message\-\-\-\--

  From: Sun, LiBin

  Sent: Saturday, August 20, 2016 9:32 AM

  To: Lai, Flying

  Cc: CN ENT ProSupport ; Lin, Qiang ; Zhan, Yanbin ; hu, Changping

  Subject: 答复: OOO N1\|SR#934454249

   

  Hi Flying

   

  Please help follow this case, thanks.

   

   

   

   

  LiBin_Sun

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

   

   

  \-\-\-\--邮件原件\-\-\-\--

  发件人: hu, Changping 

  发送时间: 2016年8月19日 18:08

  收件人: hu, Changping 

  抄送: CN ENT ProSupport ; Lin, Qiang ; Zhan, Yanbin 

  主题: OOO N1\|SR#934454249

   

  what to do

  客户问题描述：

  1、esxi 5.5\\6.0,都试过，5.5是dell的，当前6.0非dell系统

  2、部署虚拟机win2003 格式化D盘时无法格式化

  3、或客户离开服务器两次，有两次直接关机，没有报错，再开机时也可以正常开机

  4、上周四由于这台服务器到凌晨4点都还没有部署好，就更换了另一台同型号机器正常部署了虚拟机

  5、建议客户收集tsr日志，系统下查看当前阵列驱动，

   

  客户反馈能收邮件无法发送邮件，建议客户上传fileexchange：Username: 1P80PD2 

   

  客户周六可以配合操作

  1. 先收集TSR\\TTY

  2. 格式化硬盘的错误信息，是否能够有个截图

  3. 让周六的TS 尽快确认驱动版本信息 \# esxcli storage core adapter list，就及时更新成正确的测试，避免日志收集后拖到下周一搞。

   

  EU单位：南京途牛科技有限公司

  联系人信息：陈先生/13382185155

  RM:Lin,Qiang

   

  when to do

  8/20 9：30

 

已使用 OneNote 创建。
