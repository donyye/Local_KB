FW: VMware Support Request SR 15704356007    \[ ref:\_00D409hQR.\_50034jnXcC:ref \]

Thursday, July 09, 2015

2:43 PM

+--------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
| 主题     | FW: VMware Support Request SR 15704356007[    \[ ref:\_00D409hQR.\_50034jnXcC:ref \]] |
+--------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
| 发件人   | Zhao, Michael                                                                                                                               |
+--------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
| 收件人   | Yin, Guoxun; Ye, Dony; Xiao, Qihua                                                                                                          |
+--------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
| 发送时间 | Thursday, July 09, 2015 10:26 AM                                                                                                            |
+--------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+
| 附件     | \<\<VSAN_HBA.jpg\>\>                                                                                                                        |
|                                      |                                                                                                                                             |
|                                      |                                                                                                                                             |
+--------------------------------------+---------------------------------------------------------------------------------------------------------------------------------------------+

 

Dell - Internal Use - Confidential 

Guoxun & Donny:

 

Based on VMWare KB in version 6 Dell should provide PERC H730 driver for ESXi, how to understand this discribution? Do we have the driver in our support website?

 

在 ESXi 5.5 主机中使用 Dell Perc 控制器导致出现 IO 故障或中止，并报告 VSAN 磁盘异常 (2124247) 

Symptoms

免责声明：本文为 [Using a Dell Perc controller in an ESXi 5.5 host produces IO failures or aborts, and reports unhealthy VSAN disks (2109665)](http://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2109665) 的翻译版本。尽管我们会不断努力为本文提供最佳翻译版本，但本地化的内容可能会过时。有关最新内容，请参见英文版本。 

 

 

使用 Dell Perc 控制器时，遇到以下错误状况：

- VMware vCenter 事件日志显示以下消息：\
  \
  IO was aborted by VMFS via a virt-reset on the device
- 当 VSAN 负载不足时，可以看到此显示内容或类似的 VSAN 状态显示，说明 VSAN 磁盘异常：

![[个人记录_后续需要学习_009_FW_ VMware Support Request SR 15704356007_001.png]]

Cause

在 Dell ESXi 5.5 和 6.0 主机上，本机驱动程序为 lsi_mr3。但在 ESXi 5.5 中，使用 Perc 控制器需要有 megaraid_sas 或 megaraid_perc9 驱动程序。

 

由于 lsi_mr3 驱动程序已预装，即使安装了 megaraid_sas 或 megaraid_perc9 驱动程序，也不会使用它们。

Resolution

该已知问题会影响 VMware ESXi 5.x 和 6.0。要在 VMware ESXi 5.x 中解决该问题，必须确保主机上安装了 megaraid_sas 或 megaraid_perc9 驱动程序，并在 Dell Perc 控制器中使用。VSAN HCL 中指定了 ESXi 5.x 的替换驱动程序。ESXi 6.0 的替换驱动程序取决于 HBA 硬件，并且应该由硬件供应商提供。

 

要检查正在使用的驱动程序，可将其禁用，然后安装正确的驱动程序：

1.  从主机 SSH 会话运行以下命令，以显示虚拟磁盘适配器 (vmhba) 和关联的驱动程序：\
    \
    esxcfg-scsidevs -a
2.  如果看到其中一个 vmhba 正在使用 lsi_mr3 驱动程序，则运行以下命令禁用该驱动程序：\
    \
    esxcli system module set \--enabled=false \--module=lsi_mr3
3.  从适合 VSAN 的 VMware 硬件兼容性列表确定所用的 Perc 控制器的正确驱动程序，并将其下载到主机。将该文件解压以获取下一步所需的 offline-bundle.zip 文件。
4.  运行以下命令使用脱机捆绑包安装最新的 megaraid_sas 或 megaraid_perc9 驱动程序（需要绝对路径）：\
    \
    esxcli software vib install --d /absolute_path/offline-bundle.zip
5.  重新引导主机，启动后，检查同一个 vmhba（按照步骤 1），并确保其已由 megaraid_sas 或 megaraid_perc9 驱动。

要在 ESXi 6.0 主机中解决此问题，必须请 Perc 硬件供应商找出要使用的正确驱动程序，然后使用针对 ESXi 5.x 给出的步骤替换为正确的驱动程序。

 

注意：对主机应用补丁可能会移除在第 2 步中完成的模块设置，从而导致再次启用并使用不正确的驱动程序。补丁和升级应在重新检查模块设置后执行，然后禁用本机驱动程序并根据需要重新安装正确的驱动程序。

 

 

Michael Zhao

Technical Account Manager

Dell \| Global Support Service

office +86 755 2532 1020, mobile +86 180 3344 2100

 

\-\-\-\--Original Message\-\-\-\--

From: 技术部-谢仲元 \[[mailto:steven@syntek.net](mailto:steven@syntek.net)[\] ]

Sent: Thursday, July 09, 2015 9:46 AM

To: Zhao, Michael

Cc: Lu, Tony; jiaenchen@vmware.com

Subject: 转发[: VMware Support Request SR 15704356007 \[ ref:\_00D409hQR.\_50034jnXcC:ref \]]

 

昨天操作了一下，已经换了驱动了，但是还是在警告列表里面(如附件图片）

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

发件人: 技术部-谢仲元

发送时间: 2015年7月8日 20:25

收件人: \"VMware Technical Support\" ; 销售部-闫鹏

抄送: 185555444@qq.com

主题: 答复[: VMware Support Request SR 15704356007 \[ ref:\_00D409hQR.\_50034jnXcC:ref \]]

 

根据您提供的CASE，操作完成后如下：

 

执行后：（有重启系统）

\[root@VDI-BG060-OA-H5:\~\] esxcfg-scsidevs -a

vmhba0 megaraid_perc9 link-n/a unknown.vmhba0 (0000:02:00.0) LSI / Symbios Logic PERC H730 Mini

 

执行前：

\[root@VDI-BG060-OA-H5:\~\] esxcfg-scsidevs -a

vmhba0 lsi_mr3 link-n/a sas.544a842027308900 (0000:02:00.0) LSI / Symbios Logic PERC H730 Mini

 

 

 

\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_

发件人[: \"VMware Technical Support\" \[webform@vmware.com\]]

发送时间: 2015年7月8日 16:48

收件人: 销售部-闫鹏

抄送: 技术部-谢仲元; 185555444@qq.com

主题[: VMware Support Request SR 15704356007 \[ ref:\_00D409hQR.\_50034jnXcC:ref \]]

 

如您需要您的邮件被回复，请不要修改该邮件的标题！

 

尊敬的谢先生：

 

您好！

 

感谢您联系VMware技术支持中心

\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~

在 ESXi 5.5 主机中使用 Dell Perc 控制器导致出现 IO 故障或中止，并报告 VSAN 磁盘异常 (2124247)

<http://kb.vmware.com/kb/2124247>

 

如您有任何疑问，欢迎拨打我们的服务支持热线进行咨询。

 

再次感谢您对VMware的选择和支持！

 

Daniel Lv

技术支持工程师

VMware全球技术支持中心

VMware Inc.

 

 

ref:\_00D409hQR.\_50034jnXcC:ref

 

已使用 OneNote 创建。
