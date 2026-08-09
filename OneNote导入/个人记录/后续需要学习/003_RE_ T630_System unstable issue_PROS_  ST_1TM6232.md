RE: T630\|System unstable issue\|PROS\|  ST:1TM6232

2015年1月6日

11:26

- ::: 
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       RE: T630\|System unstable issue\|PROS\|[  ST:1TM6232]
    发件人     Yin, Guoxun
    收件人     Li, Jiangxiong; Xu, Hanson
    抄送       Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME
    发送时间   2015年1月6日 11:22
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Team

  我检查了4个dump，可以确定目前用户面对的BSOD问题和硬件基本没关系，从dump内容可以判定基本都是page fault，application loading/accessing violation occurred at mp kernel image。

  我们的建议总结如下：

   

  1.  卸载非必要软件，如360的所有软件
  2.  重启进入安全模式，运行sfc /scannow命令并根据扫描结果修复系统受损文件，需要提前准备好之前安装该操作系统所使用的光盘。
  3.  将dump模式修改为full memory dump, 将pagefile的大小设置为内存容量的60%（注意观察硬盘空间是否足够），继续运行观察，如果再次发生蓝屏，请用户务必等待蓝屏提示dump转存已经完成再重启主机，千万不能强行重启主机
  4.  请用户做好重新部署系统和应用的准备，目前系统存在的状况我们无法保证一定可以恢复。

   

   

   

  DEFAULT_BUCKET_ID:  WIN8_DRIVER_FAULT_SERVER

  BUGCHECK_STR:  AV

  PROCESS_NAME:  httpd.exe

   

   

  DEFAULT_BUCKET_ID:  LIST_ENTRY_CORRUPT

  BUGCHECK_STR:  0x139

  PROCESS_NAME:  bgtasksvr.exe

   

   

  DEFAULT_BUCKET_ID:  LIST_ENTRY_CORRUPT

  BUGCHECK_STR:  0x139

  PROCESS_NAME:  sqlservr.exe

   

  DEFAULT_BUCKET_ID:  WIN8_DRIVER_FAULT_SERVER

  BUGCHECK_STR:  AV

  PROCESS_NAME:  System

   

   

   

   

  \-\-\-\--Original Message\-\-\-\--

  From: Li, Jiangxiong

  Sent: 2015年1月6日 10:20

  To: Xu, Hanson; Yin, Guoxun

  Cc: Lin, Wenjie; Fu, John; CN XMN TS ENT L2 SME

  Subject: 答复: T630\|System unstable issue\|PROS\| ST:1TM6232

   

  Guoxun

  Please help to check System unstable issue

   

  Hanson

  Please open new RA to me, I will check hardware, and upload log to delta, thanks

   

   

  Li Jiangxiong

  Enterprise Product Engineer

  Dell \| Enterprise Support Services

  中文官方技术支持网站：[http://support.dell.com.cn](http://support.dell.com.cn)

  DELL硬件技术支持聊天室：[http://www.dell.com.cn/chat](http://www.dell.com.cn/chat)

  戴尔企业产品技术支持微博：[http://weibo.com/techsupportdell](http://weibo.com/techsupportdell)

   

   

  \-\-\-\--邮件原件\-\-\-\--

  发件人: [No_reply@dell.com](mailto:No_reply@dell.com) \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)[\] ]

  发送时间: 2015年1月6日 10:11

  收件人: CN XMN TS Server Escalation

  抄送: Xu, Hanson

  主题: T630\|System unstable issue\|PROS\| ST:1TM6232

   

  用户购买戴尔原厂win2012R2标准版本

  用户非专业IT,操作困难

  公司ERP系统服务器 ，无法长期关机测试

   

  1.cust report that auto reboot per 21min or 31min get DSET log /TTY log/ Minidump no find the issue DSET log show: PERC battery degrade(TTY log show: battery learn) DSET 最新3.7版本无法收集完整日志

   

  minidump：

  PSHED.dll fffff800\`8b56e000 fffff800\`8b583000 0x00015000 0x52346b3f 2013/9/14 21:57:19 Microsoft® Windows® Operating System 特定于平台的硬件错误驱动程序 6.1.7600.16385 (win7_rtm.090713-1255) C:\\Windows\\system32\\PSHED.dll BOOTVID.dll fffff800\`8b583000 fffff800\`8b58d000 0x0000a000 0x5215f8aa 2013/8/22 19:40:26 Microsoft® Windows® Operating System VGA Boot Driver 6.1.7600.16385 (win7_rtm.090713-1255) C:\\Windows\\system32\\BOOTVID.dll tcpip.sys ntoskrnl.exe

   

  2.建议BIOS 环境下测试30多分钟，正常

  3,用户暂时无法重装系统, 也不具备导出系统日志的能力

  4,更新网卡驱动, 问题依旧

   

  用户希望上门处理，

  鉴于问题复杂，上门也无法确认可以解决问题。

  故升级L2

 

已使用 OneNote 创建。
