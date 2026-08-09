收集VM-dumplog

2018年4月2日

8:24

- ::: 
    -------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: Re: Support of MS #RT#962791394
    发件人     Yin, Guoxun
    收件人     Aaron CHEN
    抄送       Kent ZHANG; zhang, eason; Huang, Jiaoming; Ye, Dony
    发送时间   2018年4月1日 15:04
    -------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Hi Aaron,

  之前我们谈的测试结果如下,  请先按照下面步骤1和2提前做好系统设置.        另外下周一和周二我不在公司也无法接听电话, 如果问题复现了,  请按照以下介绍操作后提取dump文件给我们分析.  届时请全部答复邮件. 谢谢.

   

  我再次测试了下在传统的键盘热键方式里面促使windows crash效果不行, 所以这种方式我们忽略掉.  

  就在这几天VMware提供了send nmi的方式让VM crash的KB 文章如下,模拟并测试了下该方式有效,  所以建议采用该方式在复现问题的时候抓取OS dump来分析.

  [https://kb.vmware.com/s/article/2149185](https://kb.vmware.com/s/article/2149185)

   

  请注意需要提前在系统中的system properties-à Startup and Recovery 界面中,  设置后以下红框的位置并确保满足一下条件:

  1.  标记1的位置请选择complete memory dump,
  2.  标记2的位置可以直接输入路径指定Memory.dmp文件的保存位置,  默认是C盘,  如果C盘当前空闲空间小于该机内存容量的60%, 那么请转移到其他空间满足条件的盘符,  举例如设定为D:\\Memory.dmp,  之后系统若提示重启才能生效,  那么请在合适的时间重启该主机.

   

   

  在问题复现后,按照上述VMware KB文章操作之后, VM内部屏幕应该会呈现BSOD的蓝色背景, 同时屏幕上提示正在完成故障信息转储,  等任务完成后会自动重启,  之后即可进入设定目录找到Memory.dmp文件给我们.

   

  ![[Technology_ALL_VMware_分析案例_085_收集VM-dumplog_001.png]]

   

   

   

   

  From: Aaron CHEN \[[mailto:aaron.chen@decathlon.com](mailto:aaron.chen@decathlon.com)\]

  Sent: Friday, March 30, 2018 11:46 AM

  To: Yin, Guoxun \<guoxun_Yin@Dell.com\>

  Cc: Kent ZHANG \<kent.zhang@decathlon.com\>

  Subject: Re: Re: Support of MS #RT#962791394

   

   

 

已使用 OneNote 创建。
