Vmware 里的VPC是否能用Dell OEM windows?

Wednesday, June 17, 2015

9:46 AM

- ::: 
    -------------------------------------- -------------------------------------------------------------------------------
    主题       RE: R630\|OS issue\|PROS\|sr#912582011 st#6TFD762
    发件人     Yin, Guoxun
    收件人     Li, Jiangxiong; hu, Changping
    抄送       Zeng, Mars; Gao, Yi - Dell Team; CN XMN TS ENT L2 SME
    发送时间   Wednesday, June 17, 2015 9:40 AM
    -------------------------------------- -------------------------------------------------------------------------------
  :::

   

  Changping

   

  Windows 2012没有VM key的概念，之前版本的VM KEY也只是for Hyper-v的，跟vSphere一点儿关系也没有。

  我们的OEM系统是绑定硬件的，原则上运行在VM中是不推荐的，VMware亦有说明如下列KB。

  <http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2077956&sliceId=1&docTypeID=DT_KB_1_1&dialogID=621170356&stateId=1%200%20621180174>

   

  如果客户坚持这么做的话，可以试下如下的变通办法：

   

  1.  关闭虚拟机，在虚拟机上点右键，
  2.  选择"Edit settings"  编辑设置，打开如下面的窗口，按顺序一直到第四步，添加如第四步中的那行代码，注意前后区别
  3.  然后保存退出，开机尝试激活。

  ![[Technology_ALL_VMware_分析案例_009_Vmware 里的VPC是否能用Dell OEM windows__001.png]]

   

   

  From: Li, Jiangxiong

  Sent: 2015年6月17日 9:21

  To: hu, Changping; Yin, Guoxun

  Cc: Zeng, Mars; Gao, Yi - Dell Team; CN XMN TS ENT L2 SME

  Subject: RE: R630\|OS issue\|PROS\|sr#912582011 st#6TFD762

   

  Dell - Internal Use - Confidential 

  Guoxun

  Please help on this case

   

  From: hu, Changping

  Sent: 2015年6月16日 21:31

  To: CN XMN TS Server Escalation

  Cc: Zeng, Mars; Gao, Yi - Dell Team

  Subject: R630\|OS issue\|PROS\|sr#912582011 st#6TFD762

   

  Dell - Internal Use - Confidential 

   

   

   

  1、order

  Windows Server 2012, Standard Edition, Factory Installed, No Media, 2 Socket, 2 VMs, S-Chinese

  2、现在改为底层esxi6.0，上面虚拟机win2012 standard

  3、安装好之后是未激活状态

  4、在线激活报错：0XC004E003  软件授权服务报告许可证评估失败

  5、客户的license key没有vm key，，客户疑问此随机key是否可以安装到虚拟机上

  6、已建议客户明日先联系微软激活热线：8008301832  1-4

   

   

   

 

已使用 OneNote 创建。
