关于NVME SSD

Saturday, October 11, 2014

3:03 PM

  -------------------------------------- ----------------------------------------------------------------------
  主题       关于NVME SSD 
  发件人     Yin, Guoxun
  收件人     CN XMN GSD TS Enterprise
  发送时间   Saturday, October 11, 2014 2:58 PM
  附件       \<\<dell-poweredge-exp-fsh-nvme-pcie-ssd_User\'s Guide_zh-cn.pdf\>\>
  -------------------------------------- ----------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Hi Team

请注意NVME SSD目前还没有发布VMware vSphere环境下的驱动，尽管附件手册说支持ESXi5.5，但是事实上根本不存在ESXi下的相应驱动。该设备在lspci下的特征字符串如下，供判断设备参考使用。如果有客户咨询，请务必注意此问题！

 

 

   Vendor Name: Samsung Electronics Co Ltd

   Device Name: Express Flash NVMe XS1715 SSD GB

   Vendor ID: 0x144d

   Device ID: 0xa820

   SubVendor ID: 0x1028

   SubDevice ID: 0x1f96

   Device Class: 0x0108

   Device Class Name: Non-Volatile memory controller

 

 

 

Best Regards

 

Yin Guo Xun

Dell \| Enterprise Support Services

Mail Address:[guoxun_yin@dell.com](http://guoxun_yin@dell.com)

Certifications: VCP3/4/5 , CCA , HPUX_CSA

 

How am I doing? Email my manager ([Wang, XingFang](mailto:Xing_Fang_Wang@Dell.com)) with any feedback.

 

 

已使用 OneNote 创建。
