lspci固件查询

2019年1月3日

10:38

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   RE: 小米网卡固件查询问题 #RT# SR：982209379 \<\<#671067-33986132-44103971#\>\>
  From      Ye, Dony
  To        Wu1, Peter; Liu, Lyndon
  Sent      2018年11月12日 9:03
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Hi, Peter

 

经过测试，确认最终NIC FW版本准确的方法是使用 lspci -vvv 命令查看，如下：

 

1. 测试环境，使用的CentOS7.2 系统 + X520 网卡，ethtool 看到的FW版本是 0x8000095c。

![[Technology_ALL_Linux 问题收集_046_lspci固件查询_001.jpg]]

 

 

2. lspci里看到的版本是 "18.3.6"与在官网的下载的版本是相同的。

![[Technology_ALL_Linux 问题收集_046_lspci固件查询_002.png]]

 

 

3. 升级FW到18.5.18

![[Technology_ALL_Linux 问题收集_046_lspci固件查询_003.png]]

 

 

4. 升级完成后，在lspci命令里能到FW版本是18.5.18,说明已经升级成功。

![[Technology_ALL_Linux 问题收集_046_lspci固件查询_004.png]]

 

5. 所以从ethtool -i 这个命令不能很好判断NIC的FW版本，可以通过lspci -vvv 命令来确认是否升级成功。

 

B R

Dony

 

From: Wu1, Peter

Sent: 2018年11月9日 10:41

To: Ye, Dony; Liu, Lyndon

Subject: 答复: 小米网卡固件查询问题 #RT# SR：982209379 \<\<#671067-33986132-44103971#\>\>

 

应该用的Racadm批量做的。18.5.17 因为issue问题已经下架了。 

 

18.5.18

<https://www.dell.com/support/home/cn/zh/cnbsd1/drivers/DriversDetails?productCode=poweredge-r730xd&driverId=3XJH0>

 

18.3.6

<https://www.dell.com/support/home/cn/zh/cnbsd1/drivers/DriversDetails?productCode=poweredge-r730xd&driverId=GD64X>

 

 

 

 

吴培刚 Peter Wu

China Technical Service Manager

[Peter.wu1@dell.com](mailto:Peter.wu1@dell.com)

Tel: +8610-58261966

Fax:+8610-58261000

MP:+86 13501399210

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

中文官方技术支持论坛：[http://bbs.dell.com.cn/](http://bbs.dell.com.cn/)

Dell硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

How am I doing? 欢迎您对我的服务做出评价 [feng_li@dell.com](mailto:feng_li@dell.com)

 

发件人: Ye, Dony 

发送时间: 2018年11月9日 10:12

收件人: Wu1, Peter; Liu, Lyndon

主题: RE: 小米网卡固件查询问题 #RT# SR：982209379 \<\<#671067-33986132-44103971#\>\>

 

Hi, Peter

 

刚打客户电话没有人接。从客户的截图俩看，一个CentOS6.x 的截图，一个CentOS7.x的截图，这两个不同的系统所刷出来的FW版本表示很有可能不一样，所以客户说的是否6与7版本看到的不一样了？

 

另外客户是使用idrac的方式去更新NIC FW还是通过Linux下安装BIN包安装的了？ 是否我们提供的下载链接，能否给我一下。

 

B R

Dony

 

From: Wu1, Peter

Sent: 2018年11月9日 9:56

To: Ye, Dony; Liu, Lyndon

Subject: 答复: 小米网卡固件查询问题 #RT# SR：982209379 \<\<#671067-33986132-44103971#\>\>

 

 

机器型号通过序列号反查即可， 网卡型号是i350/x520，FW就是官网的下载地址。  Centos 6.3 and 7 小版本不详得问客户了。 

 

强建新手机：17778159942

邮箱：[qiangjianxin@xiaomi.com](mailto:qiangjianxin@xiaomi.com)

 

R730 G26JKQ2  截图  18.5.17 

R730 G29GKQ2 截图 18.3.6 

 

 

 

吴培刚 Peter Wu

China Technical Service Manager

[Peter.wu1@dell.com](mailto:Peter.wu1@dell.com)

Tel: +8610-58261966

Fax:+8610-58261000

MP:+86 13501399210

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

中文官方技术支持论坛：[http://bbs.dell.com.cn/](http://bbs.dell.com.cn/)

Dell硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

How am I doing? 欢迎您对我的服务做出评价 [feng_li@dell.com](mailto:feng_li@dell.com)

 

发件人: Ye, Dony 

发送时间: 2018年11月8日 17:59

收件人: Wu1, Peter; Liu, Lyndon

主题: RE: 小米网卡固件查询问题 #RT# SR：982209379 \<\<#671067-33986132-44103971#\>\>

 

Hi, Peter

 

刚做了一些测试，NIC的FW版本号在dmesg找不到，其它log里也没有。所以需要通过其它的方法去确认。

 

另外明天我可能需要与客户确认一下，比如机器型号，网卡型号，使用的那个FW的下载地址，具体OS版本，那些系统的那些FW与那些不一样等。

 

了解完后再确认如何再做测试。

 

racadm电源输出输出问题建议问一下mouse会比较清楚。

 

B R

Dony

 

From: Wu1, Peter

Sent: 2018年11月8日 17:22

To: Liu, Lyndon; Ye, Dony

Subject: 答复: 小米网卡固件查询问题 #RT# SR：982209379 \<\<#671067-33986132-44103971#\>\>

 

 

Dony:

                可否帮忙顺便看看racadm电源输出, 客户R630显示输出4个电源，不知是否正常现象

 

 

 

 

吴培刚 Peter Wu

China Technical Service Manager

[Peter.wu1@dell.com](mailto:Peter.wu1@dell.com)

Tel: +8610-58261966

Fax:+8610-58261000

MP:+86 13501399210

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

中文官方技术支持论坛：[http://bbs.dell.com.cn/](http://bbs.dell.com.cn/)

Dell硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

How am I doing? 欢迎您对我的服务做出评价 [feng_li@dell.com](mailto:feng_li@dell.com)

 

发件人: CN, EEC 

发送时间: 2018年11月8日 14:45

收件人: Wu1, Peter

主题: RE: 小米网卡固件查询问题 #RT# SR：982209379 \<\<#671067-33986132-44103971#\>\>

 

 

Hi Peter

已提交Linux 专员处理，等待跟进确认。

 

 

 

 

Lyndon liu

Tech Support Engineer, Great China Infrastructure & Client Solutions Support

Dell EMC \| Support and Deployment Services

[Lyndon_liu@Dell.com](mailto:Lyndon_liu@Dell.com)

 

如果您对我的服务有任何意见或建议,也可以联系我的经理 [Ray_wong@Dell.com](mailto:Ray_wong@Dell.com)

 

![[Technology_ALL_Linux 问题收集_046_lspci固件查询_005.png]]

Incident ID：33986132

 

\-\-- Original Message \-\--

From: \"Wu1, Peter\" \<<Peter_Wu1@Dell.com>\>

Received: 18-11-8 上午10时53分52秒 CST

To: \"CN, EEC\" \<<EEC_CN@DELL.com>\>

Subject: 小米网卡固件查询问题

TS:

                客户一批机器有18.5.17 网卡固件问题， 现在运维批量升级后，需要每个业务部门自己找时间重启机器，并进系统确认固件升级是否成功（业务方没有idrac权限）。 

 

                但是OS下显示各不相同，无法做准确确认。

 

G26JKQ2  截图  18.5.17 

G29GKQ2 截图 18.3.6 

最后一个是 18.5.18 

 

                客户想知道 firmware-version: 0x8000092E 等16位进制的含义， 或者OS下是否有其他办法查询到网卡固件版本。 

 

 

吴培刚 Peter Wu

China Technical Service Manager

[Peter.wu1@dell.com](mailto:Peter.wu1@dell.com)

Tel: +8610-58261966

Fax:+8610-58261000

MP:+86 13501399210

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn/)

中文官方技术支持论坛：[http://bbs.dell.com.cn/](http://bbs.dell.com.cn/)

Dell硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

How am I doing? 欢迎您对我的服务做出评价 [feng_li@dell.com](mailto:feng_li@dell.com)

 

发件人: 强建新 \[[mailto:qiangjianxin@xiaomi.com](mailto:qiangjianxin@xiaomi.com)[\] ]

发送时间: 2018年11月8日 10:39

收件人: Wu1, Peter

主题: 网卡固件

 

\[EXTERNAL EMAIL\]

Please report any suspicious attachments, links, or requests for sensitive information.

 

\-\-\-\-\-- Please do not remove your unique tracking number! \-\-\-\-\--

\<\<#671067-33986132-44103971#\>\>

 

已使用 OneNote 创建。
