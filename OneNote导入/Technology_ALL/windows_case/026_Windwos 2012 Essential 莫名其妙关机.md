Windwos 2012 Essential 莫名其妙关机

Tuesday, June 14, 2016

9:25 AM

- ::: 
    -------------------------------------- ---------------------------------------------------------------------------------------------------------------
    主题       Share: Windwos 2012 Essential 莫名其妙关机
    发件人     Yin, Guoxun
    收件人     CN XMN TS ENT L2 SME
    发送时间   Tuesday, June 14, 2016 9:23 AM
    -------------------------------------- ---------------------------------------------------------------------------------------------------------------
  :::

   

  遇到个客户反映服务器莫名其妙关机的CASE，检查日志发现关机事件的记录情况如下：

  ![[Technology_ALL_windows_case_026_Windwos 2012 Essential 莫名其妙关机_001.jpg]]

   

  很明显的是从系统层面发起的关机，所以让客户做了关机权限检查，限制关机权限的授权和更改管理员帐户密码，但是之后不久又发生了意料外的关机事件，Power event还是跟以前一模一样，翻阅整天的日志才发现如下这么一条记录，

  这才是关机事件的真凶

  ![[Technology_ALL_windows_case_026_Windwos 2012 Essential 莫名其妙关机_002.jpg]]

   

  客户使用的OS版本是Windows server 2012 essential，这个版本跟之前的SBS很接近，便宜但是限制多，一般在以下两种情况下会出现上面截图中的关机记录：

  1.  Windows 2012 Essential 版本只支持小于25个客户同时并发访问连接，如果超过25个就会导致如下的报错关机，如果必须支持同时并发大于25个客户连接，那么必须做License transition 到standard 版本。
  2.  有其他Windows 2012 Essential 版本的server在同一个Domain中，这个是不支持的配置，也会导致如上报错，解决办法只能是将其中一个服务器移出Domain.

   

  Share给大家供参考

   

  Essential 做license transition 到standard 版本的解释和流程参考以下网址：

  [https://blogs.technet.microsoft.com/sbs/2012/08/06/growing-beyond-25-users-with-windows-server-2012-essentials/](https://blogs.technet.microsoft.com/sbs/2012/08/06/growing-beyond-25-users-with-windows-server-2012-essentials/)

  [https://technet.microsoft.com/en-us/library/jj247582](https://technet.microsoft.com/en-us/library/jj247582)

   

   

   

   

  BR.

  Guoxun.

   

 

已使用 OneNote 创建。
