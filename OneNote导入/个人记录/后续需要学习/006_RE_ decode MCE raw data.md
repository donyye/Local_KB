RE: decode MCE raw data

2015年2月3日

17:16

  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       RE: decode MCE raw data
  发件人     W, Robin
  收件人     CN XMN TS ENT L2 SME
  抄送       Wang, Xing Fang
  发送时间   2014年12月30日 11:52
  -------------------------------------- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

仅供内部参考，请勿对外转发。谢谢！！

 

如何根据raw data定位内存槽位：

下面是使用ipmitool收的raw data，跟DSET里面看到的是一样的：a1是data3，C1是data2，20是data1

Data3中的a表示是有效raw data，1表示错误类型（一般跟description是一样的）

Data2中的C表示每个CPU有12个内存插槽。其他数值参考下面的槽位对应:

data 1中的20表示6 （1=2，2=2，4=3，8=4，10=5，20=6.....二进制）

data2中的1表示需要使用data1中的6加8，如果是0就是data2所指向的槽位，如果是3表示data2的数值加上16，以此类推。。。（因为data1最大数值是8） 

 

下面这个报错解析出来是：

B2内存Uncorrectable ECC

 

 

SEL Record ID          : 000d

Record Type           : 02

Timestamp             : 11/12/2014 19:16:28

Generator ID          : 00b1

EvM Revision          : 04

Sensor Type           : Memory

Sensor Number         : 02

Event Type            : Sensor-specific Discrete

Event Direction       : Assertion Event

Event Data            : a1c120

Description           : Uncorrectable ECC

 

槽位对应:

\- 08h = 4 DIMMs per Package

\- 09h = 6 DIMMs per Package

\- 0Ah = 8 DIMMs per Package

\- 0Bh = 9 DIMMs per Package

\- 0Ch = 12 DIMMs per Package

\- 0Dh = 24 DIMMs per Package

 

 

Thanks and Best Regards!

 

Robin Wang  (王央友）

Enterprise Tech Supp Advisor

office +86-21-22030976

Email <Robin_W@Dell.com>

Dell \| Global Customer Support Services

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn)

中文官方技术支持论坛：[http://bbs.dell.com.cn/](http://bbs.dell.com.cn/)

Dell硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

地址：上海市长宁区长宁路1027号，多媒体产业园5层501室

Room 501,No.999 Chang Ning Road, Shanghai,CN 200050

 

From: W, Robin

Sent: 2014年12月30日 10:23

To: CN XMN TS ENT L2 SME

Cc: Wang, Xing Fang

Subject: decode MCE raw data

 

Hi all，

以下信息仅供team内部分享讨论，请不要外传。谢谢！！

 

Raw data是根据Intel software developer guide来解析的，所以只能分析OEM data，machine check和CPU IERR的data都是已经解析好的，无需再次解析。

 

一般情况下只要出现machine check error都会产生4条OEM event，通过OEM event可以大概了解machine check error的原因。

 

下面是最近一个case的log：

Machine check error中的A6表示这是一个有效的数据，如果是其他的字符这些信息可能就不准确，包括OEM data可能也解析不出来。

后面的01表示第一个CPU，有些情况下DSET和idrac看到的CPU槽位不一致，可以根据raw data判断，一般来说idrac看到的跟raw data解析出来是一致的。

第二条0F0E是machine check architecture code，0E是data 1，0F是data 2，00和190400是model-specific-register地址，这个地址对我们来说是没用的，可以忽略。

 

通过MCA code到Intel software developer guide上面去解析，15.9.1和15.9.2章节。

[http://www.intel.com/content/www/us/en/architecture-and-technology/64-ia-32-architectures-software-developer-manual-325462.html](http://www.intel.com/content/www/us/en/architecture-and-technology/64-ia-32-architectures-software-developer-manual-325462.html)

![[个人记录_后续需要学习_006_RE_ decode MCE raw data_001.png]]

 

 

 

Thanks and Best Regards!

 

Robin Wang  (王央友）

Enterprise Tech Supp Advisor

office +86-21-22030976

Email <Robin_W@Dell.com>

Dell \| Global Customer Support Services

中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn)

中文官方技术支持论坛：[http://bbs.dell.com.cn/](http://bbs.dell.com.cn/)

Dell硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

地址：上海市长宁区长宁路1027号，多媒体产业园5层501室

Room 501,No.999 Chang Ning Road, Shanghai,CN 200050

 

 

已使用 OneNote 创建。
