VMware升级驱动错误

2019年4月10日

14:51

  ------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   FW: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support[  ( Service Tag: 7TCDMM2 )]
  From      Chen, Eddy (CD)
  To        Ye, Dony
  Sent      2019年4月10日 14:41
  ------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

From: Chen, Jesse

Sent: 2019年4月9日 18:49

To: Yin, Guoxun; Chen, Eddy (CD); Wang, Dingguo

Subject: RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

Internal Use - Confidential

 

Guoxun，

 

非常感谢！

 

Dingguo,

 

烦请让用户解压出来直接安装里面的VIB包即可。谢谢。

 

 

 

 

 

 

 

 

Jesse Chen

 

Enterprise Product Engineer

Dell EMC \| Enterprise Support Services

 

From: Yin, Guoxun

Sent: 2019年4月9日 18:29

To: Chen, Eddy (CD); Wang, Dingguo; Chen, Jesse

Subject: RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

Dears,

 

问题应该出在包的互相依赖性检查设置而且感觉好些少了驱动包模块(没有qedi和qedf)

这是offline 套包，原则上单独拆包装是不合理的，我这边没办法擅自做主帮用户拆包装，所以当前已经停止远程，

 

这是用户安装的时候遇到的报错

[\[root@r01u26esxi20:/tmp\] esxcli software vib install -d /tmp/QLG-qed-ESXi6.7-offline_bundle-10019024.zip] 

 [\[DependencyError\]]

 VIB QLC_bootbank_scsi-qedil\_1.0.22.0-1OEM.600.0.0.2494585 requires qedentv_ver = X.0.7.5, but the requirement cannot be satisfied within the ImageProfile.

 VIB QLC_bootbank\_qedf\_1.2.24.6-1OEM.600.0.0.2768847 requires qedentv_ver = X.0.7.5, but the requirement cannot be satisfied within the ImageProfile.

 Please refer to the log file for more details.

 

以下在测试VM上拆包分片装的结果，可以看到是可以单独装模块的，

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_001.png]]

 

 

From: Yin, Guoxun

Sent: 2019年4月9日 16:41

To: Chen, Eddy (CD); Wang, Dingguo; Chen, Jesse

Subject: FW: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

是附件中这个

 

From: Yin, Guoxun

Sent: 2019年3月26日 15:11

To: XIAO Fei; Wang, Dingguo

Cc: <731484113@qq.com>; Chen, Jesse; Chen, Eddy (CD)

Subject: RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

肖先生，

ESXi6.7中请使用附件中的这个驱动包更新Qlogic 41164卡的驱动，如下图我验证过了可以正常安装，不过这个驱动要求匹配的固件是8.37.系列，固件的事情我们还在查找和确认中，需要些时间，请知悉。

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_002.png]]

 

 

FW和Driver的兼容关系

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_003.png]]

 

 

From: XIAO Fei \<<xiaofei.yn@qq.com>\>

Sent: 2019年3月26日 12:01

To: Yin, Guoxun; Wang, Dingguo

Cc: <731484113@qq.com>

Subject: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

\[EXTERNAL EMAIL\]

查收日志，打包有12个文件，

6台，硬件日志+ nicinfo.sh

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- 原始邮件 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

发件人: \"Guoxun.Yin\"\<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>;

发送时间: 2019年3月26日(星期二) 上午10:13

收件人: \"XIAO Fei\"\<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>;\"Dingguo.Wang\"\<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>;

抄送: \"731484113\"\<[731484113@qq.com](mailto:731484113@qq.com)\>; 

主题: RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

肖先生，

请稍等，我查下驱动包的事情

 

From: XIAO Fei \<<xiaofei.yn@qq.com>\>

Sent: 2019年3月26日 10:12

To: Yin, Guoxun; Wang, Dingguo

Cc: <731484113@qq.com>

Subject: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

\[EXTERNAL EMAIL\]

你给我的包安装不了，还有  dependency

 

\[root@r01u26esxi20:/tmp\] esxcli software vib install -d /tmp/QLG-qed-6.5-offline_bundle-8467197.zip

\[DependencyError\]

VIB QLC_bootbank_scsi-qedil_1.0.22.0-1OEM.600.0.0.2494585 requires qedentv_ver = X.0.7.5, but the requirement cannot be satisfied within the ImageProfile.

VIB QLC_bootbank_qedf_1.2.24.6-1OEM.600.0.0.2768847 requires qedentv_ver = X.0.7.5, but the requirement cannot be satisfied within the ImageProfile.

Please refer to the log file for more details.

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- 原始邮件 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

发件人: \"Guoxun.Yin\"\<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>;

发送时间: 2019年3月25日(星期一) 中午1:00

收件人: \"XIAO Fei\"\<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>;\"Dingguo.Wang\"\<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>;

抄送: \"731484113\"\<[731484113@qq.com](mailto:731484113@qq.com)\>; 

主题: RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

@肖先生，

日志的对应时间(下面是UTC时间，+8H是我们东八区的时间)可以看到VSAN 网络上有通讯异常如下面附图，然后VSAN heartbeat超时，跟之前的猜测一样，跟41164的驱动固件相关。，

升级驱动和固件可以解决问题，之前那个网址下载下来的包的版本还是不对， 请用附件中的这个驱动包更新驱动，驱动版本是3.9.17，不是3.7.9.1，

 

另外请升级完驱动并重启后，请执行命令/usr/lib/vmware/vm-support/bin/nicinfo.sh，把输出的所有信息给我一下再做下核对。

 

 

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_004.png]]

 

From: XIAO Fei \<<xiaofei.yn@qq.com>\>

Sent: 2019年3月25日 12:30

To: Yin, Guoxun; Wang, Dingguo

Cc: <731484113@qq.com>

Subject: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

\[EXTERNAL EMAIL\]

好的，最后一台日志也传成功了。

等你们的分析情况

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- 原始邮件 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

发件人: Guoxun.Yin \<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>

发送时间: 2019年3月25日 12:21

收件人: xiaofei.yn \<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>, Dingguo.Wang \<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>

抄送: 731484113 \<[731484113@qq.com](mailto:731484113@qq.com)\>

主题: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

对，

驱动是对的了，请等下我先检查完log看昨天发生的事情经过再谈

 

From: XIAO Fei \<<xiaofei.yn@qq.com>\>

Sent: 2019年3月25日 12:18

To: Yin, Guoxun; Wang, Dingguo

Cc: <731484113@qq.com>

Subject: 回复：RE: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

\[EXTERNAL EMAIL\]

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_005.png]]

 

这个驱动，系统里，已经是最新的了。

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- 原始邮件 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

发件人: \"XIAO Fei\"\<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>;

发送时间: 2019年3月25日(星期一)中午12:13

收件人: \"Guoxun.Yin\"\<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>;\"Dingguo.Wang\"\<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>;

抄送: \"[731484113@qq.com](mailto:731484113@qq.com)\"\<[731484113@qq.com](mailto:731484113@qq.com)\>;

主题: 回复：RE:回复：RE:回复：RE:回复：RE:回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

 

\[root@r01u26esxi20:/tmp\] esxcli software vib list

Name                           Version                               Vendor   Acceptance Level  Install Date

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\--  \-\-\-\-\-\-\-\-\-\-\--

dell-shared-perc8              06.806.90.00-1OEM.650.0.0.4598673     Avago    VMwareCertified   2019-01-03

lsi-mr3                        7.705.10.00-1OEM.670.0.0.8169922      Avago    VMwareCertified   2019-01-03

lsi-msgpt3                     17.00.01.00-1OEM.670.0.0.8169922      Avago    VMwareCertified   2019-01-03

bnxtnet                        212.0.119.0-1OEM.670.0.0.8169922      BCM      VMwareCertified   2019-01-03

bnxtroce                       212.0.114.0-1OEM.670.0.0.8169922      BCM      VMwareCertified   2019-01-03

dell-configuration-vib         6.7-1A03                              DellEMC  PartnerSupported  2019-01-03

dellemc-osname-idrac           6.7-0A01                              DellEMC  PartnerSupported  2019-01-03

lpfc                           12.0.257.5-1OEM.670.0.0.8169922       EMU      VMwareCertified   2019-01-03

i40en-ens                      1.1.3-1OEM.670.0.0.8169922            INT      VMwareCertified   2019-01-03

i40en                          1.7.11-1OEM.670.0.0.8169922           INT      VMwareCertified   2019-01-03

igbn                           1.4.7-1OEM.670.0.0.8169922            INT      VMwareCertified   2019-01-03

ixgben-ens                     1.1.3-1OEM.670.0.0.8169922            INT      VMwareCertified   2019-01-03

ixgben                         1.7.10-1OEM.670.0.0.8169922           INT      VMwareCertified   2019-01-03

nmlx5-core                     4.17.13.8-1OEM.670.0.0.8169922        MEL      VMwareCertified   2019-01-03

nmlx5-rdma                     4.17.13.8-1OEM.670.0.0.8169922        MEL      VMwareCertified   2019-01-03

qcnic                          1.0.16.0-1OEM.670.0.0.7535516         QLC      VMwareCertified   2019-01-03

qedentv                        3.7.9.1-1OEM.670.0.0.7535516          QLC      VMwareCertified   2019-01-03

qedf                           1.2.24.6-1OEM.600.0.0.2768847         QLC      VMwareCertified   2019-01-03

qedrntv                        3.7.9.2-1OEM.670.0.0.7535516          QLC      VMwareCertified   2019-01-03

qfle3                          1.0.69.0-1OEM.670.0.0.7535516         QLC      VMwareCertified   2019-01-03

qfle3f                         1.0.51.0-1OEM.670.0.0.7535516         QLC      VMwareCertified   2019-01-03

qfle3i                         1.0.16.0-1OEM.670.0.0.7535516         QLC      VMwareCertified   2019-01-03

scsi-qedil                     1.0.22.0-1OEM.600.0.0.2494585         QLC      VMwareCertified   2019-01-03

qlnativefc                     3.1.9.0-1OEM.670.0.0.8169922          QLogic   VMwareCertified   2019-01-03

ata-libata-92                  3.00.9.2-16vmw.670.0.0.8169922        VMW      VMwareCertified   2019-01-03

ata-pata-amd                   0.3.10-3vmw.670.0.0.8169922           VMW      VMwareCertified   2019-01-03

ata-pata-atiixp                0.4.6-4vmw.670.0.0.8169922            VMW      VMwareCertified   2019-01-03

ata-pata-cmd64x                0.2.5-3vmw.670.0.0.8169922            VMW      VMwareCertified   2019-01-03

ata-pata-hpt3x2n               0.3.4-3vmw.670.0.0.8169922            VMW      VMwareCertified   2019-01-03

ata-pata-pdc2027x              1.0-3vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

ata-pata-serverworks           0.4.3-3vmw.670.0.0.8169922            VMW      VMwareCertified   2019-01-03

ata-pata-sil680                0.4.8-3vmw.670.0.0.8169922            VMW      VMwareCertified   2019-01-03

ata-pata-via                   0.3.3-2vmw.670.0.0.8169922            VMW      VMwareCertified   2019-01-03

block-cciss                    3.6.14-10vmw.670.0.0.8169922          VMW      VMwareCertified   2019-01-03

brcmfcoe                       11.4.1078.5-11vmw.670.1.28.10302608   VMW      VMwareCertified   2019-01-03

char-random                    1.0-3vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

ehci-ehci-hcd                  1.0-4vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

elxiscsi                       11.4.1174.0-2vmw.670.0.0.8169922      VMW      VMwareCertified   2019-01-03

elxnet                         11.4.1095.0-5vmw.670.1.28.10302608    VMW      VMwareCertified   2019-01-03

hid-hid                        1.0-3vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

iavmd                          1.2.0.1011-2vmw.670.0.0.8169922       VMW      VMwareCertified   2019-01-03

ima-qla4xxx                    2.02.18-1vmw.670.0.0.8169922          VMW      VMwareCertified   2019-01-03

ipmi-ipmi-devintf              39.1-5vmw.670.1.28.10302608           VMW      VMwareCertified   2019-01-03

ipmi-ipmi-msghandler           39.1-5vmw.670.1.28.10302608           VMW      VMwareCertified   2019-01-03

ipmi-ipmi-si-drv               39.1-5vmw.670.1.28.10302608           VMW      VMwareCertified   2019-01-03

iser                           1.0.0.0-1vmw.670.1.28.10302608        VMW      VMwareCertified   2019-01-03

lpnic                          11.4.59.0-1vmw.670.0.0.8169922        VMW      VMwareCertified   2019-01-03

lsi-msgpt2                     20.00.04.00-5vmw.670.1.28.10302608    VMW      VMwareCertified   2019-01-03

lsi-msgpt35                    03.00.01.00-12vmw.670.1.28.10302608   VMW      VMwareCertified   2019-01-03

misc-cnic-register             1.78.75.v60.7-1vmw.670.0.0.8169922    VMW      VMwareCertified   2019-01-03

misc-drivers                   6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

mtip32xx-native                3.9.8-1vmw.670.1.28.10302608          VMW      VMwareCertified   2019-01-03

ne1000                         0.8.4-1vmw.670.1.28.10302608          VMW      VMwareCertified   2019-01-03

nenic                          1.0.21.0-1vmw.670.1.28.10302608       VMW      VMwareCertified   2019-01-03

net-bnx2                       2.2.4f.v60.10-2vmw.670.0.0.8169922    VMW      VMwareCertified   2019-01-03

net-bnx2x                      1.78.80.v60.12-2vmw.670.0.0.8169922   VMW      VMwareCertified   2019-01-03

net-cdc-ether                  1.0-3vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

net-cnic                       1.78.76.v60.13-2vmw.670.0.0.8169922   VMW      VMwareCertified   2019-01-03

net-e1000                      8.0.3.1-5vmw.670.0.0.8169922          VMW      VMwareCertified   2019-01-03

net-e1000e                     3.2.2.1-2vmw.670.0.0.8169922          VMW      VMwareCertified   2019-01-03

net-enic                       2.1.2.38-2vmw.670.0.0.8169922         VMW      VMwareCertified   2019-01-03

net-fcoe                       1.0.29.9.3-7vmw.670.0.0.8169922       VMW      VMwareCertified   2019-01-03

net-forcedeth                  0.61-2vmw.670.0.0.8169922             VMW      VMwareCertified   2019-01-03

net-igb                        5.0.5.1.1-5vmw.670.0.0.8169922        VMW      VMwareCertified   2019-01-03

net-ixgbe                      3.7.13.7.14iov-20vmw.670.0.0.8169922  VMW      VMwareCertified   2019-01-03

net-libfcoe-92                 1.0.24.9.4-8vmw.670.0.0.8169922       VMW      VMwareCertified   2019-01-03

net-mlx4-core                  1.9.7.0-1vmw.670.0.0.8169922          VMW      VMwareCertified   2019-01-03

net-mlx4-en                    1.9.7.0-1vmw.670.0.0.8169922          VMW      VMwareCertified   2019-01-03

net-nx-nic                     5.0.621-5vmw.670.0.0.8169922          VMW      VMwareCertified   2019-01-03

net-tg3                        3.131d.v60.4-2vmw.670.0.0.8169922     VMW      VMwareCertified   2019-01-03

net-usbnet                     1.0-3vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

net-vmxnet3                    1.1.3.0-3vmw.670.0.0.8169922          VMW      VMwareCertified   2019-01-03

nfnic                          4.0.0.14-0vmw.670.1.28.10302608       VMW      VMwareCertified   2019-01-03

nhpsa                          2.0.22-3vmw.670.1.28.10302608         VMW      VMwareCertified   2019-01-03

nmlx4-core                     3.17.9.12-1vmw.670.0.0.8169922        VMW      VMwareCertified   2019-01-03

nmlx4-en                       3.17.9.12-1vmw.670.0.0.8169922        VMW      VMwareCertified   2019-01-03

nmlx4-rdma                     3.17.9.12-1vmw.670.0.0.8169922        VMW      VMwareCertified   2019-01-03

ntg3                           4.1.3.2-1vmw.670.1.28.10302608        VMW      VMwareCertified   2019-01-03

nvme                           1.2.2.17-1vmw.670.1.28.10302608       VMW      VMwareCertified   2019-01-03

nvmxnet3-ens                   2.0.0.21-1vmw.670.0.0.8169922         VMW      VMwareCertified   2019-01-03

nvmxnet3                       2.0.0.29-1vmw.670.1.28.10302608       VMW      VMwareCertified   2019-01-03

ohci-usb-ohci                  1.0-3vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

pvscsi                         0.1-2vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

qflge                          1.1.0.11-1vmw.670.0.0.8169922         VMW      VMwareCertified   2019-01-03

sata-ahci                      3.0-26vmw.670.0.0.8169922             VMW      VMwareCertified   2019-01-03

sata-ata-piix                  2.12-10vmw.670.0.0.8169922            VMW      VMwareCertified   2019-01-03

sata-sata-nv                   3.5-4vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

sata-sata-promise              2.12-3vmw.670.0.0.8169922             VMW      VMwareCertified   2019-01-03

sata-sata-sil24                1.1-1vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

sata-sata-sil                  2.3-4vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

sata-sata-svw                  2.3-3vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

scsi-aacraid                   1.1.5.1-9vmw.670.0.0.8169922          VMW      VMwareCertified   2019-01-03

scsi-adp94xx                   1.0.8.12-6vmw.670.0.0.8169922         VMW      VMwareCertified   2019-01-03

scsi-aic79xx                   3.1-6vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

scsi-bnx2fc                    1.78.78.v60.8-1vmw.670.0.0.8169922    VMW      VMwareCertified   2019-01-03

scsi-bnx2i                     2.78.76.v60.8-1vmw.670.0.0.8169922    VMW      VMwareCertified   2019-01-03

scsi-fnic                      1.5.0.45-3vmw.670.0.0.8169922         VMW      VMwareCertified   2019-01-03

scsi-hpsa                      6.0.0.84-3vmw.670.0.0.8169922         VMW      VMwareCertified   2019-01-03

scsi-ips                       7.12.05-4vmw.670.0.0.8169922          VMW      VMwareCertified   2019-01-03

scsi-iscsi-linux-92            1.0.0.2-3vmw.670.0.0.8169922          VMW      VMwareCertified   2019-01-03

scsi-libfc-92                  1.0.40.9.3-5vmw.670.0.0.8169922       VMW      VMwareCertified   2019-01-03

scsi-megaraid-mbox             2.20.5.1-6vmw.670.0.0.8169922         VMW      VMwareCertified   2019-01-03

scsi-megaraid-sas              6.603.55.00-2vmw.670.0.0.8169922      VMW      VMwareCertified   2019-01-03

scsi-megaraid2                 2.00.4-9vmw.670.0.0.8169922           VMW      VMwareCertified   2019-01-03

scsi-mpt2sas                   19.00.00.00-2vmw.670.0.0.8169922      VMW      VMwareCertified   2019-01-03

scsi-mptsas                    4.23.01.00-10vmw.670.0.0.8169922      VMW      VMwareCertified   2019-01-03

scsi-mptspi                    4.23.01.00-10vmw.670.0.0.8169922      VMW      VMwareCertified   2019-01-03

scsi-qla4xxx                   5.01.03.2-7vmw.670.0.0.8169922        VMW      VMwareCertified   2019-01-03

shim-iscsi-linux-9-2-1-0       6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

shim-iscsi-linux-9-2-2-0       6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

shim-libata-9-2-1-0            6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

shim-libata-9-2-2-0            6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

shim-libfc-9-2-1-0             6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

shim-libfc-9-2-2-0             6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

shim-libfcoe-9-2-1-0           6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

shim-libfcoe-9-2-2-0           6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

shim-vmklinux-9-2-1-0          6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

shim-vmklinux-9-2-2-0          6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

shim-vmklinux-9-2-3-0          6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

smartpqi                       1.0.1.553-12vmw.670.1.28.10302608     VMW      VMwareCertified   2019-01-03

uhci-usb-uhci                  1.0-3vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

usb-storage-usb-storage        1.0-3vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

usbcore-usb                    1.0-3vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

vmkata                         0.1-1vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

vmkfcoe                        1.0.0.1-1vmw.670.1.28.10302608        VMW      VMwareCertified   2019-01-03

vmkplexer-vmkplexer            6.7.0-0.0.8169922                     VMW      VMwareCertified   2019-01-03

vmkusb                         0.1-1vmw.670.1.28.10302608            VMW      VMwareCertified   2019-01-03

vmw-ahci                       1.2.3-1vmw.670.1.28.10302608          VMW      VMwareCertified   2019-01-03

xhci-xhci                      1.0-3vmw.670.0.0.8169922              VMW      VMwareCertified   2019-01-03

cpu-microcode                  6.7.0-1.28.10302608                   VMware   VMwareCertified   2019-01-03

elx-esx-libelxima.so           11.4.1184.0-0.0.8169922               VMware   VMwareCertified   2019-01-03

esx-base                       6.7.0-1.39.11675023                   VMware   VMwareCertified   2019-01-28

esx-dvfilter-generic-fastpath  6.7.0-0.0.8169922                     VMware   VMwareCertified   2019-01-03

esx-ui                         1.30.0-9946814                        VMware   VMwareCertified   2019-01-03

esx-update                     6.7.0-1.39.11675023                   VMware   VMwareCertified   2019-01-28

esx-xserver                    6.7.0-0.0.8169922                     VMware   VMwareCertified   2019-01-03

lsu-hp-hpsa-plugin             2.0.0-16vmw.670.1.28.10302608         VMware   VMwareCertified   2019-01-03

lsu-intel-vmd-plugin           1.0.0-2vmw.670.1.28.10302608          VMware   VMwareCertified   2019-01-03

lsu-lsi-drivers-plugin         1.0.0-1vmw.670.1.39.11675023          VMware   VMwareCertified   2019-01-28

lsu-lsi-lsi-mr3-plugin         1.0.0-13vmw.670.1.28.10302608         VMware   VMwareCertified   2019-01-03

lsu-lsi-lsi-msgpt3-plugin      1.0.0-9vmw.670.1.39.11675023          VMware   VMwareCertified   2019-01-28

lsu-lsi-megaraid-sas-plugin    1.0.0-9vmw.670.0.0.8169922            VMware   VMwareCertified   2019-01-03

lsu-lsi-mpt2sas-plugin         2.0.0-7vmw.670.0.0.8169922            VMware   VMwareCertified   2019-01-03

lsu-smartpqi-plugin            1.0.0-3vmw.670.1.28.10302608          VMware   VMwareCertified   2019-01-03

native-misc-drivers            6.7.0-0.0.8169922                     VMware   VMwareCertified   2019-01-03

rste                           2.0.2.0088-7vmw.670.0.0.8169922       VMware   VMwareCertified   2019-01-03

vmware-esx-esxcli-nvme-plugin  1.2.0.34-1.28.10302608                VMware   VMwareCertified   2019-01-03

vmware-fdm                     6.7.0-11726888                        VMware   VMwareCertified   2019-01-28

vsan                           6.7.0-1.39.11399593                   VMware   VMwareCertified   2019-01-28

vsanhealth                     6.7.0-1.39.11399595                   VMware   VMwareCertified   2019-01-28

tools-light                    10.3.2.9925305-10176879               VMware   VMwareCertified   2019-01-03

\[root@r01u26esxi20:/tmp\]

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--原始邮件 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

发件人: \"Guoxun.Yin\"\<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>;

发送时间: 2019年3月25日(星期一)中午12:08

收件人: \"XIAO Fei\"\<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>;\"Dingguo.Wang\"\<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>;

抄送: \"731484113\"\<[731484113@qq.com](mailto:731484113@qq.com)\>;

主题: RE:回复：RE:回复：RE:回复：RE:回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

 

包名字不对，您先解压缩刚下的那个包，拿到如下面的这个带offline_bundle字样的压缩包，就不用再解压缩了，按照我说的方式安装就应该正常了

 

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_006.png]]

 

From: XIAO Fei \<<xiaofei.yn@qq.com>\>

Sent: 2019年3月25日 12:07

To: Yin, Guoxun; Wang, Dingguo

Cc: <731484113@qq.com>

Subject: 回复：RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

\[EXTERNAL EMAIL\]

安装不了，软件包报错。

\[root@r01u26esxi20:\~\] esxcli software vib install -d /tmp/QLG-qed-ESXi6.7-8434518.zip

\[MetadataDownloadError\]

Could not download from depot at zip:/tmp/QLG-qed-ESXi6.7-8434518.zip?index.xml, skipping ((\'zip:/tmp/QLG-qed-ESXi6.7-8434518.zip?index.xml\', \'\', \'Error extracting index.xml from /tmp/QLG-qed-ESXi6.7-8434518.zip: \"There is no item named \\\'index.xml\\\' in the archive\"\'))

        url = zip:/tmp/QLG-qed-ESXi6.7-8434518.zip?index.xml

 Please refer to the log file for more details.

 

验证过 MD5是正确的。

md5 QLG-qed-ESXi6.7-8434518.zip

MD5 (QLG-qed-ESXi6.7-8434518.zip) = 12febaab32b859ae2479419bdec4ecad

 

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--原始邮件 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

发件人: \"Guoxun.Yin\"\<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>;

发送时间: 2019年3月25日(星期一)中午11:59

收件人: \"XIAO Fei\"\<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>;\"Dingguo.Wang\"\<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>;

抄送: \"731484113\"\<[731484113@qq.com](mailto:731484113@qq.com)\>;

主题: RE:回复：RE:回复：RE:回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

 

这个包不能解压缩，直接传送下面这个offline包到esxi  /tmp目录下，然后把主机至于维护模式，运行命令esxcli system software vib install -d /tmp/QLGxxxxxx.zip 安装

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_006.png]]

安装后要重启节点才能生效

 

From: Yin, Guoxun

Sent: 2019年3月25日 11:49

To: \'XIAO Fei\'; Wang, Dingguo

Cc: <731484113@qq.com>

Subject: RE: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

这个包不能解压缩，直接传送下面这个offline包到esxi  /tmp目录下，然后把主机至于维护模式，运行命令esxcli system software vib install -d /tmp/QLGxxxxxx.zip 安装

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_006.png]]

 

From: XIAO Fei \<<xiaofei.yn@qq.com>\>

Sent: 2019年3月25日 11:45

To: Yin, Guoxun; Wang, Dingguo

Cc: <731484113@qq.com>

Subject: 回复：RE: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

\[EXTERNAL EMAIL\]

 

 

 

解压完后，有两个版本。我升级哪个版本？

 

QLC_bootbank_qedentv_3.7.9.1-1OEM.670.0.0.7535516.vib

QLC_bootbank_qedrntv_3.7.9.2-1OEM.670.0.0.7535516.vib

 

是不是在esxi上用下面命令升级？

 

esxcli software vib update -v

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--原始邮件 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

发件人: \"Guoxun.Yin\"\<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>;

发送时间: 2019年3月25日(星期一)中午11:43

收件人: \"XIAO Fei\"\<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>;\"Dingguo.Wang\"\<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>;

抄送: \"731484113\"\<[731484113@qq.com](mailto:731484113@qq.com)\>;

主题: RE:回复：RE:回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

好的，下载连接如下，

您方便的话就先收集下，不方便的话我先看那三个也OK，

这次情况和时间点都比较清晰，所以22的可能不是必须的

 

<https://my.vmware.com/web/vmware/details?downloadGroup=DT-ESX65-QLOGIC-QED-39170&productId=614>

 

 

From: XIAO Fei \<<xiaofei.yn@qq.com>\>

Sent: 2019年3月25日 11:30

To: Yin, Guoxun; Wang, Dingguo

Cc: <731484113@qq.com>

Subject: 回复：RE: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

\[EXTERNAL EMAIL\]

22的日志，有问题，传了两次都报错，我再重新收集一下。

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--原始邮件 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

发件人: \"XIAO Fei\"\<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>;

发送时间: 2019年3月25日(星期一)中午11:25

收件人: \"Guoxun.Yin\"\<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>;\"Dingguo.Wang\"\<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>;

抄送: \"[731484113@qq.com](mailto:731484113@qq.com)\"\<[731484113@qq.com](mailto:731484113@qq.com)\>;

主题: 回复：RE:回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

驱动还没有升级。

给一个驱动的下载连接。

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--原始邮件 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

发件人: \"Guoxun.Yin\"\<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>;

发送时间: 2019年3月25日(星期一)中午11:09

收件人: \"XIAO Fei\"\<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>;\"Dingguo.Wang\"\<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>;

抄送: \"731484113\"\<[731484113@qq.com](mailto:731484113@qq.com)\>;

主题: RE:回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

好的，我会去下载日志，

驱动有升级吗？

 

From: XIAO Fei \<<xiaofei.yn@qq.com>\>

Sent: 2019年3月25日 10:56

To: Yin, Guoxun; Wang, Dingguo

Cc: <731484113@qq.com>

Subject: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

\[EXTERNAL EMAIL\]

日志已经上传，网卡微码已经升级。

 

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_007.png]]

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--原始邮件 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

发件人: \"XIAO Fei\"\<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>;

发送时间: 2019年3月25日(星期一)上午9:38

收件人: \"Guoxun.Yin\"\<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>;\"Dingguo.Wang\"\<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>;

抄送: \"[731484113@qq.com](mailto:731484113@qq.com)\"\<[731484113@qq.com](mailto:731484113@qq.com)\>;

主题: 回复：RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

昨天，下午6：54发生故障，四个节点都发了，日志稍后上传。

网卡微码还没有来得及升级。

 

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_008.png]]

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--原始邮件 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

发件人: \"Guoxun.Yin\"\<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>;

发送时间: 2019年3月25日(星期一)上午9:07

收件人: \"XIAO Fei\"\<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>;\"Dingguo.Wang\"\<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>;

抄送: \"731484113\"\<[731484113@qq.com](mailto:731484113@qq.com)\>;

主题: RE: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

肖先生，

不用重新开CASE，你就还是用以前的方法传送下日志就行，我收到后就会去检查，

另外请告诉我出问题的大概时间点和问题出现在了哪个节点上，

 

 

From: XIAO Fei \<<xiaofei.yn@qq.com>\>

Sent: 2019年3月25日 9:00

To: Wang, Dingguo

Cc: <731484113@qq.com>; Yin, Guoxun

Subject: Re: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

\[EXTERNAL EMAIL\]

昨天又发生故障了，我们把日志收下开了，怎么上传呢？需要新开case吗？

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- Original \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

From: Dingguo.Wang \<[Dingguo.Wang@dell.com](mailto:Dingguo.Wang@dell.com)\>

Date: Thu,Mar 21,2019 5:07 PM

To: xiaofei.yn \<[xiaofei.yn@qq.com](mailto:xiaofei.yn@qq.com)\>

Cc: 731484113 \<[731484113@qq.com](mailto:731484113@qq.com)\>, Guoxun.Yin \<[Guoxun.Yin@dell.com](mailto:Guoxun.Yin@dell.com)\>

Subject: Re: Dell Technical Support ( Service Tag: 7TCDMM2 )

 

 

尊敬的戴尔客户：

 

      您好！以下是通过iDRAC的Web界面更新固件的方法，请参考：

 

步骤一：下载固件：[https://www.dell.com/support/home/cn/zh/cndhs1/drivers/driversdetails?driverid=25xd6&oscode=w12r2&productcode=poweredge-r940xa](https://www.dell.com/support/home/cn/zh/cndhs1/drivers/driversdetails?driverid=25xd6&oscode=w12r2&productcode=poweredge-r940xa)

 

 

步骤二：登陆iDRAC，详见附件

 

步骤三：开始更新(主要步骤如下,完整版见附件）

（1）iDRAC7的默认IP是192.168.0.120，默认的用户：root   密码：calvin

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_009.jpg]]

（2）更新过程的图示如下，请注意

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_010.png]]

（3）上载（上传）过程中可以看到，大约3－5分钟

 

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_011.png]]

（4）然后出现，点击"安装"即可

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_012.png]]

 

（5）然后出现"如下提示"――表示"更新已经开始"

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_013.png]]

（6）若点击了"作业队列"，就可以看到如下信息了

![[Technology_ALL_VMware_分析案例_099_VMware升级驱动错误_014.jpg]]

 

 为了确保您在报修过程中花费更少的精力，您可以邮件联系我，我悉知您此次报修的具体情况，能够准确定位您的问题

 

后续如需更多帮助，请直接回复本邮件，我一定尽力帮您解决！

 

真诚希望我们的服务能够令您满意,祝您身体健康, 工作愉快!

 

 

 

Best Regards!

 

 

Dingguo Wang

Tech Support Analyst, Great China Customer Support Services

Dell EMC \| Support and Deployment Services

[Dingguo_Wang@dell.com](mailto:Dingguo_Wang@dell.com)

How am I doing? Please contact my manager <Sukie_Wu@Dell.com>

 

 

已使用 OneNote 创建。
