FW: 【Case share】Brocade 300报错：FABOS stop & fabos not yet initialized

Saturday, October 08, 2016

12:16 PM

  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       FW: 【Case share】Brocade 300报错：FABOS stop & fabos not yet initialized
  发件人     Wang, Xing Fang
  收件人     CN XMN TS ENT L2 SME; CN XMN TS Networking
  发送时间   Saturday, October 08, 2016 11:21 AM
  附件       \<\<Fabric OS Troubleshooting.pdf\>\>
  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------

 

Fyi

 

XingFang Wang

Technical Support Manager

Dell \| Global Customer Support Services

office +86-592-818-5846

Email [Xing_Fang_Wang@Dell.com](mailto:Your%20name@Dell.com)

Visit us at [http://support.ap.dell.com](http://support.ap.dell.com/)

How am I doing?E-mail my manager at <Ernest_Lee@dell.com>

 

From: Lai, Flying

Sent: Saturday, October 08, 2016 10:29 AM

To: CCC Ent ProSupport Storage Agent Group \<CCC_Ent_ProSupport_Storage_Agent_Group@Dell.com\>

Cc: Guo, Yueliang \<Yueliang_Guo@dell.com\>; Wu, Bingyang \<Bingyang_Wu@Dell.com\>; Chen, Johnson Lw \<Johnson_Lw_Chen@DELL.com\>

Subject: 【Case share】Brocade 300报错：FABOS stop & fabos not yet initialized

 

Dell - Internal Use - Confidential 

Dear All

 

以前也有碰到过这个问题，不过当时是派单解决的，这一次碰到两台出厂不到20天的机器也是同样问题，就想说研究下，好在解决了，所以Share出来，希望能帮到大家。

 

机型：Brocade300

问题：两台Brocade300节假日正常关机，10/7开机要开始使用，却发现两台交换机的状态灯都是黄灯常亮，用串口输任何命令都无效会报错，相关提示如下两图：

              

![[Technology_ALL_未分类知识库_058_FW_ 【Case share】Brocade 300报错：FABOS stop_001.jpg]]

 

![[Technology_ALL_未分类知识库_058_FW_ 【Case share】Brocade 300报错：FABOS stop_002.jpg]]

 

诊断步骤：

1．关机断电，放静电等待5m，还是一样

               2．查询OKB，无果

               3．查询Brocade FABOS troubleshooting guide，无果（见附件）

               4．通过Google查询到如下方法：

               <http://community.brocade.com/t5/Fibre-Channel-SAN/Brocade-300-switch-fabos-not-yet-initialized-1/td-p/27230>

解决方法：

1．重启机器，按照如下方式进入command shell

![[Technology_ALL_未分类知识库_058_FW_ 【Case share】Brocade 300报错：FABOS stop_003.jpg]]

 

![[Technology_ALL_未分类知识库_058_FW_ 【Case share】Brocade 300报错：FABOS stop_004.jpg]]

 

2．输入printenv，查看当前的环境变量，主要查看OSRootPartition，默认值是hda1;hda2

![[Technology_ALL_未分类知识库_058_FW_ 【Case share】Brocade 300报错：FABOS stop_005.jpg]]

3．设置OSRootPartition为hda2;hda1，之后保存，然后再次启动

               =\> setenv OSRootPartition "hda2;hda1"

![[Technology_ALL_未分类知识库_058_FW_ 【Case share】Brocade 300报错：FABOS stop_006.jpg]]

 

![[Technology_ALL_未分类知识库_058_FW_ 【Case share】Brocade 300报错：FABOS stop_007.jpg]]

4．启动之后，会以hda2的配置来启动，并且去修复hda1（由于客户权限问题，无法拿到串口输出），修复完成之后就正常能启动了

 

PS：据客户反馈丢了几条Zone的配置，不过绝大部分配置还是在的，具体原因就不清楚了，如果有同学有其他方法的话也可以分享下。J

赖志明 Lai, Flying

企业级产品工程师

戴尔\|企业级技术支持  

客户反馈\| 我表现如何？请联系我的经理[Johnson_LW_Chen@dell.com](mailto:Johnson_LW_Chen@dell.com)

减少致电转接，使用快速服务代码，直达售后团队，[点此获取快速服务代码](http://www.dell.com/support/troubleshooting/cn/zh/cnbsd1/expressservice?c=cn&l=zh&s=gen)。 [了解更多请点此链接](http://zh.community.dell.com/support_forums/poweredge/f/251/t/2807)。

24小时 服务热线： 800-858-0613或  400-886-8616            戴尔技术支持官方网站：[www.dell.com.cn/home](http://www.dell.com.cn/home)

[![[Technology_ALL_未分类知识库_058_FW_ 【Case share】Brocade 300报错：FABOS stop_008.jpg]]](http://www.dell.com/prodeploy)

  

[![[Technology_ALL_未分类知识库_058_FW_ 【Case share】Brocade 300报错：FABOS stop_009.jpg]]](http://www.dell.com/certification)

 

 

已使用 OneNote 创建。
