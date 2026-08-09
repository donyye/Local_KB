RE: 客户遇到的一例紫屏问题,VM结论供参考

Tuesday, August 25, 2015

12:25 PM

  -------------------------------------- ------------------------------------------------------------------------------------------------------------------------
  主题       RE: 客户遇到的一例紫屏问题,VM结论供参考
  发件人     Li, Andrew
  收件人     Ye, Dony; Yang, Stone; Liu, Tianqiao; Yin, Guoxun
  发送时间   Tuesday, August 25, 2015 11:33 AM
  -------------------------------------- ------------------------------------------------------------------------------------------------------------------------

 

增加紫屏截图

![[Technology_ALL_VMware_分析案例_013_RE_ 客户遇到的一例紫屏问题,VM结论供参考_001.jpg]]

Best Regards

 

Andrew Li 李青春

Technical Account Manager

Dell \| Global Support and Deployment

 

From: Li, Andrew

Sent: 2015年8月25日 11:20

To: Ye, Dony; Yang, Stone; Liu, Tianqiao; Yin, Guoxun

Subject: 客户遇到的一例紫屏问题,VM结论供参考

 

供参考：

 

Best Regards

 

Andrew Li 李青春

Technical Account Manager

Dell \| Global Support and Deployment

 

\-\-\-\--Original Message\-\-\-\--

From: VMware Technical Support \[[mailto:webform@vmware.com](mailto:webform@vmware.com)\]

Sent: 2015年8月20日 16:36

To: <fufei@dpca.com.cn>; Li, Andrew; <yandi@dpca.com.cn>

Subject: VMware Support Request 15736319008 \[ ref:\_00D409hQR.\_50034kGiNt:ref \]

 

如您需要您的邮件被回复，请不要修改该邮件的标题！

 

尊敬的用户：

 

您好！

 

我是VMware售后工程师Evan，现在将由我与您一起共同解决SR#15736319008的问题。

 

从SR的记录中，我了解到您目前遇到的问题是：

\[客户环境[\]]：ESXi5.1

\[问题描述[\]]：一台ESXi主机出现紫屏。

\[问题参数[\]]：重启后恢复正常，测试环境中，业务受到了影响。

\[相关操作[\]]：

\[辅助资源[\]]：可提供紫屏的截图。

\[客户期望[\]]：请工程师联系：付先生 [fufei@dpca.com.cn](mailto:fufei@dpca.com.cn) 13886167562 

 

紫屏报错:

 

) - possible deadlock10Z cpu2:1075473)@BlueScreen: Spin count exceeded (!ä-4ñ 2015-08-17T14:19:25.710Z cpu2:1075473)Code start: 0x418014a00000 VMK uptime: 25:06:19:56.440 2015-08-17T14:19:25.711Z cpu2:1075473)0x4122e445b4d8:\[0x418014a7b71a\]PanicvPanicInt@vmkernel#nover+0x61 stack: 0x3000000008 2015-08-17T14:19:25.711Z cpu2:1075473)0x4122e445b5b8:\[0x418014a7bf1b\]Panic@vmkernel#nover+0xae stack: 0x4122e445b680 2015-08-17T14:19:25.712Z cpu2:1075473)0x4122e445b618:\[0x418014a8e368\]SP_WaitLock@vmkernel#nover+0x29f stack: 0x30e4b000 2015-08-17T14:19:25.713Z cpu2:1075473)0x4122e445b6a8:\[0x418014badf40\]NetSchedFIFOInput@vmkernel#nover+0x1e7 stack: 0x4122e445b6f8 2015-08-17T14:19:25.714Z cpu2:1075473)0x4122e445b758:\[0x418014bad102\]NetSchedInput@vmkernel#nover+0x191 stack: 0x4122e445b808 2015-08-17T14:19:25.714Z cpu2:1075473)0x4122e445b7f8:\[0x418014b3e010\]IOChain_Resume@vmkernel#nover+0x247 stack: 0x4122e445b858 2015-08-17T14:19:25.715Z cpu2:1075473)0x4122e445b848:\[0x418014b2d224\]PortOutput@vmkernel#nover+0xe3 stack: 0x4122e445b8c8 2015-08-17T14:19:25.716Z cpu2:1075473)0x4122e445b8c8:\[0x41801501f4b8\]TeamES_Output@#+0x16b stack: 0x1 2015-08-17T14:19:25.716Z cpu2:1075473)0x4122e445bac8:\[0x418015012047\]EtherswitchPortDispatch@#+0x142a stack: 0xffffffff000000 2015-08-17T14:19:25.717Z cpu2:1075473)0x4122e445bb38:\[0x418014b2c407\]Port_InputResume@vmkernel#nover+0x146 stack: 0x4122e445bb78 2015-08-17T14:19:25.718Z cpu2:1075473)0x4122e445bbd8:\[0x418014b2ef87\]PortsetProcessDeferredList@vmkernel#nover+0xce stack: 0x4122e445bc78 2015-08-17T14:19:25.719Z cpu2:1075473)0x4122e445bc28:\[0x418014b2f3a1\]Portset_ProcessAllDeferred@vmkernel#nover+0x8c stack: 0x41001c301280 2015-08-17T14:19:25.719Z cpu2:1075473)0x4122e445bc48:\[0x418014b31e45\]Portset_ReleasePort@vmkernel#nover+0x24 stack: 0x1e445bcc8 2015-08-17T14:19:25.720Z cpu2:1075473)0x4122e445bcc8:\[0x41801501f2d1\]TeamES_AdvertiseUnicastAddrTimer@#+0x280 stack: 0x410014 2015-08-17T14:19:25.721Z cpu2:1075473)0x4122e445bd68:\[0x418014aa65a4\]Timer_BHHandler@vmkernel#nover+0x20f stack: 0x418014d73f70 2015-08-17T14:19:25.721Z cpu2:1075473)0x4122e445bde8:\[0x418014a2088d\]BH_Check@vmkernel#nover+0x98 stack: 0x2e445be38 2015-08-17T14:19:25.722Z cpu2:1075473)0x4122e445bed8:\[0x418014bc8aab\]CpuSchedDispatch@vmkernel#nover+0x11ee stack: 0x0 2015-08-17T14:19:25.723Z cpu2:1075473)0x4122e445bf48:\[0x418014bc96cb\]CpuSchedWait@vmkernel#nover+0x242 stack: 0x410000000000 2015-08-17T14:19:25.723Z cpu2:1075473)0x4122e445bf98:\[0x418014bc98f0\]CpuSched_VcpuHalt@vmkernel#nover+0x14b stack: 0x418014bba525 2015-08-17T14:19:25.724Z cpu2:1075473)0x4122e445bfe8:\[0x418014af4478\]VMMVMKCall_Call@vmkernel#nover+0x1af stack: 0x0 2015-08-17T14:19:25.725Z cpu2:1075473)0x418014ac8408:\[0xfffffffffc223a16\]\_\_vmk_versionInfo_str@#+0xe6c3aeb5 stack: 0x0

 

根据这个报错来看，这是一个已知问题，您需要更新补丁来修复这个问题

 

请您更新下面KB提及的补丁

VMware ESXi 5.1, Patch ESXi510-201503401-BG: Updates esx-base (2099293)

[http://kb.vmware.com/kb/2099293](http://kb.vmware.com/kb/2099293)

 

 

此期间如果您有什么问题欢迎通过Email或拨打我们的免费技术支持热线进行咨询。

 

 

感谢您对VMware的选择和支持。

 

Evan Zhang

技术支持工程师

VMware全球技术支持中心

VMware Inc.

 

VMware 2015网络云博会将于7月23日揭幕, 欢迎注册：[http://info.vmware.com/content/APAC_CN_VCD2015_Reg?src=em_15Q2VM-GSS_APAC_CN_VCD2015&partnerref=em_15Q2VM-GSS_APAC_CN_VCD2015](http://info.vmware.com/content/APAC_CN_VCD2015_Reg?src=em_15Q2VM-GSS_APAC_CN_VCD2015&partnerref=em_15Q2VM-GSS_APAC_CN_VCD2015) 

 

相关链接：

技术支持电话:http://www.vmware.com/cn/support/phone_support.html

知识库：[http://www.vmware.com/r/knowledgebase.html](http://www.vmware.com/r/knowledgebase.html)

论坛：[http://www.vmware.com/support/pubs/](http://www.vmware.com/support/pubs/)

 

工作时间：星期-至星期五 8:30AM - 5:30PM CST

 

 

ref:\_00D409hQR.\_50034kGiNt:ref

 

已使用 OneNote 创建。
