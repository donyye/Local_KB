答复: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

2015年1月19日

13:15

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       答复: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633
  发件人     Lv, Zhiwei
  收件人     Ding, Simon; Ye, Dony; Yang, Jack
  抄送       Li, Jiangxiong
  发送时间   2015年1月19日 11:56
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Simon

 

这是一个多路径引起的 performance issue

 

建议可以share给team学习下.

 

Thanks\~

 

 

 

 

Regards

 

Zhiwei Lv

GC Resolution Manager

Dell \| Global Support & Deployment (GSD)

office +86 592-818-8832

 

发件人: Ding, Simon 

发送时间: 2015年1月19日 11:54

收件人: Lv, Zhiwei; Ye, Dony; Yang, Jack

抄送: Li, Jiangxiong

主题: RE: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

Hi ,

最新情况如下

1,RH5.4看起来速度比较正常.

RH5.4 bs=64k 226mb/s

RH5.4 bs=128k 577mb/s

RH5.4 bs=512k 369mb/s

RH5.4 bs=64k oflag=dsync 12.9mb/s

RH5.4 bs=128k oflag=dsync 20mb/s

RH5.4 bs=512k oflag=dsync 40mb/s

 

2,RH6.5刚开始测试速度很慢(客户没有截图),后面参看RH5.4的配置文件,修改了device里面的path_checker为readsector0(原来是rdac)之后,速度如下:

RH6.5 bs=64k 252mb/s

RH6.5 bs=128k 274mb/s

RH6.5 bs=512k 140mb/s

 

3,RH6.5速度恢复正常(修改之前,只有有写入路径就会不固定的failed). 

 

4,FW的问题客户后期会备份数据.后期在看时间来升级.

 

5,客户表示可以close case了

 

 

谢谢各位的帮忙

 

 

 

+-------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| 丁思明                                                                                                                    | Simon Ding                                                                 |
|                                                                                                                                                       |                                                                                                        |
| 企业级产品工程师                                                                                                                                      | Enterprise Product Engineer                                                                            |
|                                                                                                                                                       |                                                                                                        |
| [戴尔]\| 企业级技术支持  |                                                                                                        |
|                                                                                                                                                       |                                                                                                        |
|                                                                                                                                                       |                                                                                                        |
+-------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+

 

 

From: Lv, Zhiwei

Sent: 2015年1月15日 10:22

To: Ding, Simon; Ye, Dony; Yang, Jack

Cc: Li, Jiangxiong

Subject: 答复: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

Dell - Internal Use - Confidential 

我印象, 如果VD的块是128, 那测试就最大写性能就对应在软件或者命令下用128k来测试, 其他的参数,64k,32k,这些都是做参考.

 

之前有问了下SC他们现场帮客户测试数据的话是这样, 针对性的测试.

 

Simon你在继续跟进看看.

 

谢谢dony和jack.

 

 

 

Regards

 

Zhiwei Lv

GC Resolution Manager

Dell \| Global Support & Deployment (GSD)

office +86 592-818-8832

 

发件人: Ding, Simon 

发送时间: 2015年1月15日 10:20

收件人: Ye, Dony; Lv, Zhiwei; Yang, Jack

抄送: Li, Jiangxiong

主题: RE: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

好的.

 

我看了VD的是128k现在正在让客户用128k/512k都测试下看下,并且对比下RH6.5的测试出来的值,然后在看下.

 

谢谢.

 

 

 

+-------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| 丁思明                                                                                                                    | Simon Ding                                                                 |
|                                                                                                                                                       |                                                                                                        |
| 企业级产品工程师                                                                                                                                      | Enterprise Product Engineer                                                                            |
|                                                                                                                                                       |                                                                                                        |
| [戴尔]\| 企业级技术支持  |                                                                                                        |
|                                                                                                                                                       |                                                                                                        |
|                                                                                                                                                       |                                                                                                        |
+-------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+

 

 

From: Ye, Dony

Sent: 2015年1月15日 10:18

To: Lv, Zhiwei; Ding, Simon; Yang, Jack

Cc: Li, Jiangxiong

Subject: 答复: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

BS 参数的大小是会有影响，应为BS是一次性读入或写入字节的大小，具体设置多少没定义，自己测试一下就知道了。 

 

Best Regards

 

Ye Jian Yuan

Dell \| Enterprise Support Services

Mail Address:dony_ye@dell.com

 

发件人: Lv, Zhiwei 

发送时间: 2015年1月15日 9:45

收件人: Ding, Simon; Yang, Jack

抄送: Li, Jiangxiong; Ye, Dony

主题: 答复: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

Dell - Internal Use - Confidential 

Block Size 早一点的那个case我们是设置1M

 

而且1M下测试看起来并没有问题.

 

这个也需要参考他的MD上的LUN, 配置的时候, 条带设置是多大. 

 

Dony哥帮忙确认看看, 如果 LUN的块大小假设是128k, 那linux命令下测试用1M和64k 是不是影响很大?

 

感觉客户之前的测试方法好像不对

 

 

Regards

 

Zhiwei Lv

GC Resolution Manager

Dell \| Global Support & Deployment (GSD)

office +86 592-818-8832

 

发件人: Ding, Simon 

发送时间: 2015年1月15日 9:41

收件人: Lv, Zhiwei; Yang, Jack

抄送: Li, Jiangxiong; Ye, Dony

主题: RE: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

 

下面这个是BS-1M的时候的结果,

 

Dony,

 

您看建议这个BS用多少来测试比较合适的参考值?

 

 

 

![[Technology_ALL_未分类知识库_036_答复_ MD3200I_Performan ce issue(PROS)_SR_F_001.png]]

这个是bs=64k的结果

![[Technology_ALL_未分类知识库_036_答复_ MD3200I_Performan ce issue(PROS)_SR_F_002.png]]

 

+-------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| 丁思明                                                                                                                    | Simon Ding                                                                 |
|                                                                                                                                                       |                                                                                                        |
| 企业级产品工程师                                                                                                                                      | Enterprise Product Engineer                                                                            |
|                                                                                                                                                       |                                                                                                        |
| [戴尔]\| 企业级技术支持  |                                                                                                        |
|                                                                                                                                                       |                                                                                                        |
|                                                                                                                                                       |                                                                                                        |
+-------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+

 

 

From: Ding, Simon

Sent: 2015年1月15日 9:34

To: Lv, Zhiwei; Yang, Jack

Cc: Li, Jiangxiong; Ye, Dony

Subject: RE: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

Hi ,

 

Update

 

1,RH5.4下速度比RH6.5要好一些,截图如下,

2,RH5.4 下多路径固定四个是failed的,现在还在检查配置.

 

 

Orcal RH 6.5

 

![[Technology_ALL_未分类知识库_036_答复_ MD3200I_Performan ce issue(PROS)_SR_F_003.png]]

 

Redhat 5.4

![[Technology_ALL_未分类知识库_036_答复_ MD3200I_Performan ce issue(PROS)_SR_F_002.png]]

 

Redhat 5.4下,固定下面四个failed,orcal RH6.5的时候是不固定那条会failed

![[Technology_ALL_未分类知识库_036_答复_ MD3200I_Performan ce issue(PROS)_SR_F_004.png]]

 

+-------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+
| 丁思明                                                                                                                    | Simon Ding                                                                 |
|                                                                                                                                                       |                                                                                                        |
| 企业级产品工程师                                                                                                                                      | Enterprise Product Engineer                                                                            |
|                                                                                                                                                       |                                                                                                        |
| [戴尔]\| 企业级技术支持  |                                                                                                        |
|                                                                                                                                                       |                                                                                                        |
|                                                                                                                                                       |                                                                                                        |
+-------------------------------------------------------------------------------------------------------------------------------------------------------+--------------------------------------------------------------------------------------------------------+

 

 

From: Lv, Zhiwei

Sent: 2015年1月14日 15:50

To: Yang, Jack

Cc: Ding, Simon; Li, Jiangxiong; Ye, Dony

Subject: 答复: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

Dell - Internal Use - Confidential 

Hello jack

 

客户在windows下测试其实写可以到100MB, 这个性能他可以接受了.

 

只是在RH6上性能不理想.所以我让Simon在部署这部分先跳过switch.

 

Simon, 把客户用于测试写性能的Linux下命令提供一下.

 

目前等看看客户用RH5的版本测试写, 是否正常, 如果是ok的, 客户会安排时间更新firmware到80这个版本(兼容RH6)

 

 

 

Regards

 

Zhiwei Lv

GC Resolution Manager

Dell \| Global Support & Deployment (GSD)

office +86 592-818-8832

 

发件人: Yang, Jack 

发送时间: 2015年1月14日 15:40

收件人: Lv, Zhiwei

抄送: Ding, Simon; Li, Jiangxiong; Ye, Dony

主题: RE: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

 

Zhiwei

同时 建议检查 下交换机的配置，看下交换机上面是否按照要求做了优化配置，谢谢

 

From: Lv, Zhiwei

Sent: 2015年1月14日 11:29

To: Yang, Jack

Cc: Ding, Simon; Li, Jiangxiong; Lv, Zhiwei; Ye, Dony

Subject: 答复: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

Dell - Internal Use - Confidential 

Hi Jack

 

早上我跟Simon联系客户, 当前控制器075这个版本只support到RH5.

 

比较不好的消息是,客户反映这台的部署当时是随机器下的,这套环境是EDT的人帮他搭建的(两两ISCSI 口同网段)

 

客户会安装一个RH5的版本来测试写性能,确认是否是固件影响.

 

对于客户使用的测试命令, Simon你帮忙补充起来

 

同时邮件我也CC给Dony, 看看Dony这边对于在Linux下, 用来测试读性能的命令用什么命令来测试比较合理.

 

Thanks!

 

Regards

 

Zhiwei Lv

GC Resolution Manager

Dell \| Global Support & Deployment (GSD) office +86 592-818-8832

 

 

\-\-\-\--邮件原件\-\-\-\--

发件人: Yang, Jack

发送时间: 2015年1月13日 16:49

收件人: Li, Jiangxiong

抄送: Ding, Simon; Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME; CN XMN TS Storage Escalation

主题: RE: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

 

I will follow the case .

 

\-\-\-\--Original Message\-\-\-\--

From: Li, Jiangxiong

Sent: 2015年1月13日 16:25

To: Yang, Jack

Cc: Ding, Simon; Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME; CN XMN TS Storage Escalation

Subject: 答复: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

Jack

Please help on this case

 

Li Jiangxiong

Enterprise Product Engineer

Dell \| Enterprise Support Services

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn)

DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

 

 

\-\-\-\--邮件原件\-\-\-\--

发件人: [No_reply@dell.com](mailto:No_reply@dell.com) \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)[\] ]

发送时间: 2015年1月13日 16:19

收件人: CN XMN TS Server Escalation

抄送: Ding, Simon

主题: MD3200I\|Performan ce issue(PROS)\|SR:F7W953X\|sR:905248633

 

Detail Symptom Descriptions:

1,服务器安装的orcal rh6.5的版本,不管是安装在hyper-v的虚拟机里面还是安装在物理服务器上,速度都很慢,客户的windows服务器是正常的,可以达到100m/s以上,orcal linux慢了三倍多(客户没有官方标准一些的测试方法,也是用copy,对着秒表来测试的,对比windows速度慢了三倍多,估计预计30m/s左右),客户没有用命令或者工具来测试过准确的速度.

Troubleshooting Setups:

2,在orcal rh6.5系统里面只有有I/O读写,发现multipach 里面就有随机的路径是failed的

3,存储日志里面并没有发现问题.

4,客户尝试安装redhat 6.3配置多路径来测试,现象一样有I/O就会有路径显示failed,并且速度比较慢(有截图信息)包括日志.

Current status:

5,配置上没有发现问题暂时,客户着急使用起来这个linux机器,需要我们尽快确认问题

6,升级L2协助处理

Must collect logs:

md log/linux配置截图

 

已使用 OneNote 创建。
