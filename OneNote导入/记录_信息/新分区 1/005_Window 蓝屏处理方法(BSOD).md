Window 蓝屏处理方法(BSOD)

Wednesday, June 10, 2015

1:17 PM

- ::: 
    -------------------------------------- ---------------------------------------------------------------------------------
    主题       RE: R720\|System unstable issue\|PROS\|SR:912292170
    发件人     Yin, Guoxun
    收件人     Li, Jiangxiong; Fang, Yubin
    抄送       Lin, Wenjie; CN XMN TS ENT L2 SME
    发送时间   Wednesday, June 10, 2015 12:46 PM
    -------------------------------------- ---------------------------------------------------------------------------------
  :::

   

  Yubin

  据目前收集到的信息看，建议做以下安排：

   

  1.  搜索C盘根目录看是否存在任何dmp文件，有的话返回给我们
  2.  停止目前正在运行的所有应用，或者重启进入安全模式，执行命令sfc /scannow 扫描系统，按照提示检查修复受损文件
  3.  设置为kernel memory dump

   

  请提醒用户下次BSOD的时候务必做到

  1.  拍下字体清晰可见的蓝屏照片给我们
  2.  确认屏幕提示Dump动作是否完成，或者拍照片后是否还有任何变化

   

   

  From: Li, Jiangxiong

  Sent: 2015年6月10日 11:24

  To: Yin, Guoxun

  Cc: Fang, Yubin; Lin, Wenjie; CN XMN TS ENT L2 SME

  Subject: RE: R720\|System unstable issue\|PROS\|SR:912292170

   

  Dell - Internal Use - Confidential 

  Guoxun

  Please help on this case

  \-\-\-\--Original Message\-\-\-\--

  From: <No_reply@dell.com> \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)\]

  Sent: 2015年6月10日 11:22

  Cc: CN XMN TS Server Escalation; Fang, Yubin

  Subject: R720\|System unstable issue\|PROS\|SR:912292170

   

  Detail Symptom Descriptions

  机器昨天蓝屏2次 蓝屏后需要手动重启

  Troubleshooting Steps

   

  OEM OS

  guide cust check dmp setting is small memory dump,cant found any dmp files cust hasnot capture BSOD screen DSET no hw error

   

  Bios/Driver/FW及存储控制器相关FW版本:

  Current status

  客户公司名称:/业务影响:/升级的原因:/RM/TAM:

  Must Collect Logs

  DSET,MPS log已经提供

 

已使用 OneNote 创建。
