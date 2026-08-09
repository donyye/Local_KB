案例4：VC无法启动WCP问题

2023年6月2日

11:42

- 客户在VC新建了一个 cluster，但是发现状态不对，是Degraded 状态，而且vCLS 也没有自动部署。

  IC: 168889310 \
   

  ![[VMware-排错_vCenter_排查_014_案例4：VC无法启动WCP问题_001.png]]

   

  在ESXI使用 esxcli vm process list 查看确实没有vCls vm在。

   

  点击cluster查看你cluster 信息，找到\"domain-cxxxx\"

  ![[VMware-排错_vCenter_排查_014_案例4：VC无法启动WCP问题_002.png]]

  点击VC，在"配置"里的"高级"选项里可以添加值False和在True，也没有反应。(默认是 True，写错无法修改)

  config.vcls.clusters.domain-c2007.enabled      False

   

  当时在VC里查看VC的服务也没问题

  ![[VMware-排错_vCenter_排查_014_案例4：VC无法启动WCP问题_003.png]]

   

  后来收集了VC log，在日志里有看到如下错误，和KB吻合，根据KB方式进行修复：

   

  /var/log/vmware/wcp/wcpsvc.log

  2023-05-29T20:20:13.202Z error wcp \[eamlib/lister.go:84\] \[opID=EAMAgent\] Failed to get EAM agencies. Err ServerFaultCode: EAM is still loading from database. Please try again later.

  2023-05-29T20:20:13.202Z error wcp \[informer/informer.go:129\] \[opID=EAMAgent\] Failed to list EAMAgent. Err ServerFaultCode: EAM is still loading from database. Please try again later.

  2023-05-29T20:20:23.209Z error wcp \[eamlib/lister.go:84\] \[opID=EAMAgent\] Failed to get EAM agencies. Err ServerFaultCode: EAM is still loading from database. Please try again later.

  2023-05-29T20:20:23.209Z error wcp \[informer/informer.go:129\] \[opID=EAMAgent\] Failed to list EAMAgent. Err ServerFaultCode: EAM is still loading from database. Please try again later.

  2023-05-29T20:20:33.217Z error wcp \[eamlib/lister.go:84\] \[opID=EAMAgent\] Failed to get EAM agencies. Err ServerFaultCode: EAM is still loading from database. Please try again later.

  2023-05-29T20:20:33.217Z error wcp \[informer/informer.go:129\] \[opID=EAMAgent\] Failed to list EAMAgent. Err ServerFaultCode: EAM is still loading from database. Please try again later.

   

  通过这个KB进行解决

  <https://kb.vmware.com/s/article/80588>

   

  1.  Log in to the vCenter Server Appliance using SSH.
  2.  Run this command to enable access the Bash shell:\
      \
      shell.set \--enabled true\
       
  3.  Type shell and press Enter.
  4.  Run this command to retrieve the vpxd-extension solution user certificate and key:\
      \
      mkdir /certificate\
      \
      /usr/lib/vmware-vmafd/bin/vecs-cli entry getcert \--store vpxd-extension \--alias vpxd-extension \--output /certificate/vpxd-extension.crt\
      \
      /usr/lib/vmware-vmafd/bin/vecs-cli entry getkey \--store vpxd-extension \--alias vpxd-extension \--output /certificate/vpxd-extension.key\
       
  5.  Run this command to update the extension\'s certificate with vCenter Server.\
      \
      python /usr/lib/vmware-vpx/scripts/updateExtensionCertInVC.py -e com.vmware.vim.eam -c /certificate/vpxd-extension.crt -k /certificate/vpxd-extension.key -s localhost -u Administrator@domain.local

  Note: If this produces the error \"Hostname mismatch, certificate is not valid for \'localhost\'\", change \'localhost\' to the FQDN or IP of the vCenter. The process is checking this value against the SAN entries of the certificate.

  Note: The default user and domain is Administrator@vsphere.local. If this was changed during configuration, change the domain to match your environment. When prompted, type in the Administrator@domain.local password.

  1.  Restart EAM and start the rest of the services with these commands:

  service-control \--stop vmware-eam

  service-control \--start \--all

   

  需要注意的是运行 python 命令的时候可能会报错，但是注意看前面如果是有 "Successfully updated certificate"其实说明是成功的。

  对这个问题KB也有描述。

   

  ![[VMware-排错_vCenter_排查_014_案例4：VC无法启动WCP问题_004.png]]

   

  当时的截图

  ![[VMware-排错_vCenter_排查_014_案例4：VC无法启动WCP问题_005.png]]

   

   

   

 

已使用 OneNote 创建。
