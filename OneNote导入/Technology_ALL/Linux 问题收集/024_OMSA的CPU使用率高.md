[ OMSA]的CPU使用率高

Wednesday, November 02, 2016

9:30 AM

- ::: 
    -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: ST:GP6L342[  SR: 937803038   \| OMSA]的CPU使用率高
    发件人     Chen, Roy
    收件人     Hine 刘金海; Jiang, Jimmy
    抄送       Zhao, Thomson; qbanana@163.com; Gao, Ken; wenx 邓志尧; wchen@coremail.cn; Ren, Amy; Ruan, Garuda; Ye, Dony
    发送时间   Tuesday, November 01, 2016 4:35 PM
    -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

   

  刘先生

           您好， 感谢您的理解和配合。 如我们刚才电话所说，以下是相关的建议，供您参考， 如有什么疑问，及时让我们知道，感谢。

   

   

  1.  当前的OS是 CentOS release 6.6 Kernel 2.6.32-504.1.3.el6.x86_64 (x86_64) ；OMSA是8.1版本；
  2.  当前从最新的日志看， 服务器硬件工作正常。
  3.  根据你的反馈，目前服务器上的相关业务应用工作正常。
  4.  根据现有的信息，如下是相关的建议，供您参考：
      a.  尝试修改日期格式。（在操作之前先参考如下具体说明，同时最好确认是否有特定相关的应用需要特定的日期格式，在决定是否改）
      b.  更新到最新的版本的OMSA  8.4（先卸载旧版本在安装新版本，安装OMSA时最好安排在维护时间， 下载链接参考下面说明）。
  5.  如果问题依旧，帮忙在收集最新的sosreport和TSR日志。
  6.  如果有其他服务器在同样的环境下 没有这个问题的，  也帮忙收集一下对应的sosreport和TSR日志,以便我们做参考。
  7.  另外涉及到非DELL提供的操作系统Centos相关的问题，也建议你们也同时寻求其他OS方面的资源一起协同介入处理。

  感谢。  

   

   

   

  相关的资料，供你参考：

   

  一：dsm_om_connsvcd high cpu utilization

  [http://lists.us.dell.com/pipermail/linux-poweredge/2012-July/046595.html](http://lists.us.dell.com/pipermail/linux-poweredge/2012-July/046595.html) 

  solution:

  date -s \"\`date -u\`\"  

   

  二： dsm_om_connsvcd taking up a lot of CPU after date switch to 2012-07-01  

  [http://lists.us.dell.com/pipermail/linux-poweredge/2012-June/046591.html](http://lists.us.dell.com/pipermail/linux-poweredge/2012-June/046591.html) 

  solution:

  date; date \`date +\"%m%d%H%M%C%y.%S\"\`; date

   

  三：dsm_om_connsvcd uses 100% CPU

  [http://en.community.dell.com/support-forums/servers/f/177/t/19455661](http://en.community.dell.com/support-forums/servers/f/177/t/19455661)  

  solution:

  \# date -s \"\$(LC_ALL=C date)\"

  Then if you restart the OMSA services it should be ok. Does that seem to improve things?   

   

   

  OMSA 8.4的下载链接：

  [http://downloads.dell.com/FOLDER03909029M/1/OM-SrvAdmin-Dell-Web-LX-8.4.0-2193.RHEL6.x86_64_A00.tar.gz](http://downloads.dell.com/FOLDER03909029M/1/OM-SrvAdmin-Dell-Web-LX-8.4.0-2193.RHEL6.x86_64_A00.tar.gz)     

    

   

   

  Roy_Chen

  Resolution Manager

  Dell \| Global Support & Deployment (GSD)

  office +86 592 818 6226

  Roy_Chen@Dell.com

   

  From: Hine 刘金海 \[[mailto:jhliu@coremail.cn](mailto:jhliu@coremail.cn)[\] ]

  Sent: 2016年10月31日 12:51

  To: Jiang, Jimmy \<Jimmy_Jiang@Dell.com\>

  Cc: Chen, Roy \<Roy_Chen@Dell.com\>; Zhao, Thomson \<Thomson_Zhao@dell.com\>; qbanana@163.com; Gao, Ken \<Ken_Gao@dell.com\>; wenx 邓志尧 \<wdeng@coremail.cn\>; wchen@coremail.cn; Ren, Amy \<Amy_Ren@Dell.com\>

  Subject: Re: ST:GP6L342

   

  您好, 

    dsm_om_connsvcd占用CPU使用达1250.2 比系统的应用全部加起来还要高,如下截图, 2号电源丢失是机房电路断电导致,但是很快就恢复用电了.请进一步分析,谢谢! 

  ![[Technology_ALL_Linux 问题收集_024_OMSA的CPU使用率高_001.png]]

   

   

   

  \-\-\-\--原始邮件\-\-\-\--

  发件人:Jimmy.Jiang@dell.com

  发送时间:2016-10-31 11:47:32 (星期一)

  收件人: [jhliu@coremail.cn](mailto:jhliu@coremail.cn)

  抄送: [Roy.Chen@dell.com](mailto:Roy.Chen@dell.com)

  主题: ST:GP6L342

  刘先生，您好

  从日志看，近期没有硬件的报警，

  只看到10月20日的2号电源冗余丢失（有可能是2号电源线没有接好） 

  您说的openmanage 占用CPU使用率过高， 建议查看操作系统里哪些应用占用CPU资源比较多。 

  CENTOS系统非戴尔认证的系统，我们只能从硬件层面帮您分析一下。 

   

  ![[Technology_ALL_Linux 问题收集_024_OMSA的CPU使用率高_002.jpg]]

   

  发件人: CN, EEC 

  发送时间: Monday, October 31, 2016 11:36 AM

  收件人: Jiang, Jimmy \<[Jimmy_Jiang@Dell.com](mailto:Jimmy_Jiang@Dell.com)\>

  主题: RE: GP6L342-CPU使用率过高 \<\<#1977118-18468993-23492651#\>\> 

   

  jimmy,

      你的case请follow下. 

  谢谢！

   

  \-\-- Original Message \-\--

  From: \"Hine 刘金海\" \<[jhliu@coremail.cn](mailto:jhliu@coremail.cn)\>

  Received: 31/10/16 上午 09:57:56

  To: \"CN, EEC\" \<<EEC_CN@DELL.com>\>, \"Zhao, Thomson\" \<<Thomson_Zhao@dell.com>\>

  CC:\"[qbanana@163.com](mailto:qbanana@163.com)\" \<[qbanana@163.com](mailto:qbanana@163.com)\>, \"wenx 邓志尧\" \<[wdeng@coremail.cn](mailto:wdeng@coremail.cn)\>, \"[wchen@coremail.cn](mailto:wchen@coremail.cn)\" \<[wchen@coremail.cn](mailto:wchen@coremail.cn)\>

  Subject: GP6L342-CPU使用率过高 

  Thomson,

      您好, openmanage 占用CPU使用率过高,请帮忙分析处理,谢谢! 

   

   

   

  \-\-\-\-\-- Please do not remove your unique tracking number! \-\-\-\-\--

  \<\<#1977118-18468993-23492651#\>\>

 

已使用 OneNote 创建。
