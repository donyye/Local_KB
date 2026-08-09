ESXI 更新失败 

Wednesday, July 27, 2016

3:31 PM

  -------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------
  主题       Case Closed\| Normal escalation\| M630\|OS install\|PSP\|[  SR:933552912]
  发件人     Yin, Guoxun
  收件人     Jin, Leon
  抄送       CN XMN TS Server Coach; CN XMN GSD TS ESG MGMT
  发送时间   Wednesday, July 27, 2016 2:52 PM
  -------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------

 

Hi Leon,

RA已经关闭。

Esxi updating 失败是因为出厂预装的系统分区顺序设置不合理导致更新程序无法执行，删除其中无效分区后更新成功，问题解决，用户同意关闭CASE。

 

BR.

Guoxun.

From: Lin, Yongliang

Sent: 2016年7月25日 11:38

To: Yin, Guoxun \<guoxun_Yin@Dell.com\>; Jin, Leon \<Leon_Jin@DELL.com\>

Cc: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

Subject: 答复: M630\|OS install\|PSP\| SR:933552912

 

Dell - Internal Use - Confidential 

hi Guoxun:

 

help

Thanks & Best Regards.

Yongliang Lin

Enterprise Product Engineer

Dell \| Enterprise Support Services

 

 

\-\-\-\--邮件原件\-\-\-\--

发件人: [No_reply@dell.com](mailto:No_reply@dell.com) \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)[\] ]

发送时间: 2016年7月25日 11:22

抄送: CN XMN TS Server Escalation ; Jin, Leon 

主题: M630\|OS install\|PSP\| SR:933552912

 

由于业务需要，系统升级

原来出厂安装的ESXI 5.5 系统升级到ESXi6.0 在安装完毕重新启动过程中出现了如下错误，就启动不来； 

ESXI6.0 是最新Dell定制的

VMware-VMvisor-Installer-6.0.0.update02-3620759.x86_64-Dell_Customized-A02.iso

 

出现问题后 按回车系统重启还是报同样的错误， 

客户表示20几台服务器同样型号同样的问题

 

客户有发送错误信息截图邮件可以L2

 

公司名称：veritas 软件公司维理软体

 

已使用 OneNote 创建。
