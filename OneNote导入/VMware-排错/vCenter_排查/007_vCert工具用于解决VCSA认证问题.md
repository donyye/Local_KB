vCert工具用于解决VCSA认证问题

2025年3月31日

16:21

 

vCert Tool to troubleshoot VCSA Certification issue

vCert工具用于解决VCSA认证问题

 

仅供参考-博通提供的一种工具，可用于修复VCSA证书过期问题。

我刚刚用它来修复我们的evovcenter.sst.lab证书过期问题。

 

![[VMware-排错_vCenter_排查_007_vCert工具用于解决VCSA认证问题_001.jpg]]

 

\<\<vCert-6.0.0-20250218.zip\>\>

 

附件是一个vcert.py工具，用于排除vCenter证书问题。

下载附件，解压缩，然后上传到VCSA中的/temp文件夹。

然后按照下面的屏幕截图所示运行它。

下面是许多好的功能。

 

注意：在进行任何更改之前，请务必进行备份。😊

 

vCert工具用于解决VCSA认证问题

![[VMware-排错_vCenter_排查_007_vCert工具用于解决VCSA认证问题_002.gif]]

 

选项1：检查当前认证状态

 

![[VMware-排错_vCenter_排查_007_vCert工具用于解决VCSA认证问题_003.gif]]

\
选项2：查看认证信息

![[VMware-排错_vCenter_排查_007_vCert工具用于解决VCSA认证问题_004.gif]]

选项2-1：查看机器SSL认证

![[VMware-排错_vCenter_排查_007_vCert工具用于解决VCSA认证问题_005.gif]]

选项2-7:STS签名认证

![[VMware-排错_vCenter_排查_007_vCert工具用于解决VCSA认证问题_006.gif]]

 

\
选项5：检查配置

![[VMware-排错_vCenter_排查_007_vCert工具用于解决VCSA认证问题_007.gif]]

\
 

选项9：生成认证报告

报告位置：var/log/vmware/vCert/vcenter-certification-Report.txt

![[VMware-排错_vCenter_排查_007_vCert工具用于解决VCSA认证问题_008.gif]]

 

![[VMware-排错_vCenter_排查_007_vCert工具用于解决VCSA认证问题_009.gif]]

 

 

 

已使用 OneNote 创建。
