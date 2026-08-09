VCSA 证书过期

2023年5月31日

12:32

- VCSA 证书过期

  FYI： 

  <https://kb.vmware.com/s/article/79248?lang=zh_cn>  \--\> 用checksts.sh来验证是否SLS证书过期了。

  <https://kb.vmware.com/s/article/76719>  \--\> 使用fixsts.sh来修复证书过期问题。

  如果 vCenter Server 部署为版本 6.5 Update 2 或更高版本，则安全令牌服务 (STS) 签名证书的有效期可能为两年。根据 vCenter 的部署时间，证书可能即将过期。

   

  注意：vCenter Server 不会在升级时刷新 STS 证书。

   

  TS 证书过期时，不会出现警告。在某些系统中，证书在初始部署两年后即过期。

   

  注意：

  - 在以下情况下，STS签名证书的使用寿命预计为 2 年左右。
  - 并非涉及所有的 6.5 U2 及以上版本，仅限 6.5 产品线中的 6.5 U2 或更高版本。
  - 全新安装的 PSC / vCenter Server 6.5 U2 或更高版本（仅 6.5 产品线）。
  - 新安装的 PSC / vCenter Server 6.5 U2 或任何更高版本的 6.5 产品，并已升级到更高版本，包括 6.7 和 7.0。
  - 在 PSC 或 vCenter Server 安装后，使用 certool 替换了 STS 签名证书。
  - STS 签名证书已替换为自定义证书（内部/外部 CA 签名）。

   

  1.  下载本知识库文章所附的 checksts.py 脚本\
      [https://kb.vmware.com/s/article/79248?lang=zh_cn](https://kb.vmware.com/s/article/79248?lang=zh_cn)   在附件里
  2.  上载到 vCenter Server 或外部 PSC。例如，VCSA 上的 /tmp 或 Windows 上的 %TEMP% （可以使用 WinSCP 将脚本上载到 VCSA，如果使用 WinSCP 连接失败，请参见 [Error when uploading files to vCenter Server Appliance using WinSCP](https://kb.vmware.com/s/article/2107727) ）。
  3.  使用 cd /tmp 更改为 /tmp 目录：
  4.  运行 python checksts.py

  - 对于 Windows，请运行 %VMWARE_PYTHON_BIN% checksts.py

  ![[VMware-排错_vCenter_排查_006_VCSA 证书过期_001.png]]

  说明证书过期了，没有过期的如下图：

  ![[VMware-排错_vCenter_排查_006_VCSA 证书过期_002.png]]

  替换证书测试过程。使用 fixsts.sh 脚本

  <https://kb.vmware.com/s/article/76719>

  [root@vcsa \[ \~ \]#] ./fixsts.sh 

  NOTE: This works on external and embedded PSCs

  This script will do the following

  1: Regenerate STS certificate

  What is needed?

  1: Offline snapshots of VCs/PSCs

  2: SSO Admin Password

  IMPORTANT: This script should only be run on a single PSC per SSO domain

  ==================================

  Resetting STS certificate for vcsa.ddoonnyy.com started on Wed Jan 13 13:23:03 CST 2021

   

   

  Detected DN: cn=vcsa.ddoonnyy.com,ou=Domain Controllers,dc=vsphere,dc=local

  Detected PNID: vcsa.ddoonnyy.com

  Detected PSC: vcsa.ddoonnyy.com

  Detected SSO domain name: vsphere.local

  Detected Machine ID: 2ccb8d93-14c2-4505-9f48-df80de8baccc

  Detected IP Address: 10.10.40.250

  Domain CN: dc=vsphere,dc=local

  ==================================

  ==================================

   

  Detected Root\'s certificate expiration date: 2030 Aug 27

  Detected today\'s date: 2021 Jan 13

  ==================================

   

  Exporting and generating STS certificate

   

  Status : Success

  Using config file : /tmp/vmware-fixsts/certool.cfg

  Status : Success

   

   

  Enter password for administrator@vsphere.local: \<\-- 输入VC密码

  Amount of tenant credentials: 1

  Exporting tenant 1 to /tmp/vmware-fixsts

   

  Deleting tenant 1

   

  Amount of trustedcertchains: 1

  Exporting trustedcertchain 1 to /tmp/vmware-fixsts

   

  Deleting trustedcertchain 1

   

   

  Applying newly generated STS certificate to SSO domain

  adding new entry \"cn=TenantCredential-1,cn=vsphere.local,cn=Tenants,cn=IdentityManager,cn=Services,dc=vsphere,dc=local\"

   

  adding new entry \"cn=TrustedCertChain-1,cn=TrustedCertificateChains,cn=vsphere.local,cn=Tenants,cn=IdentityManager,cn=Services,dc=vsphere,dc=local\"

   

   

  Replacement finished - Please restart services on all vCenters and PSCs in your SSO domain  \<\--完成

  ==================================

  IMPORTANT: In case you\'re using HLM (Hybrid Linked Mode) without a gateway, you would need to re-sync the certs from Cloud to On-Prem after following this procedure

  ==================================

  ==================================

  root@vcsa \[ \~ \]# 

   

  证书更新完成后重启服务

  [root@vcsa \[ \~ \]#  ]service-control \--stop \--all 

  [root@vcsa \[ \~ \]#  ]service-control \--start \--all

  并使用命令监控服务启动情况。大概需要5分钟。

  [root@vcsa \[ \~ \]#] watch -n5 service-control \--status \--all

   

  第二部分：如果有服务不能驱动，请做下面FQDN与hosts的检查：

  \-\-\-\-\-\-\-- 确认\-\-\-\-\-\-\--

  [root@vcsa \[ \~ \]#] /usr/lib/vmware-vmafd/bin/vmafd-cli get-pnid \--server-name localhost

  [root@vcsa \[ \~ \]#] hostnamectl status

  [root@vcsa \[ \~ \]#] nslookup vcsa.ddoonnyy.com

  [root@vcsa \[ \~ \]#] nslookup 10.10.40.250

  [root@vcsa \[ \~ \]#] cat /etc/hosts

   

   

   

  第三部分：尝试更新所有的证书

  <https://kb.vmware.com/s/article/2112283>

  /usr/lib/vmware-vmca/bin/certificate-manager

  ![[VMware-排错_vCenter_排查_006_VCSA 证书过期_003.png]]

   

  ![[VMware-排错_vCenter_排查_006_VCSA 证书过期_004.png]]

  需要注意的是，VCSA name 这里是证书的名字，不要和之前的证书名字重复，随便填个新的命令字，如newxxx 就行了。

  ![[VMware-排错_vCenter_排查_006_VCSA 证书过期_005.png]]

  完成后，它会自动重启VCSA的服务。

   

  ======

  补充：更新了VCSA的证书后对horzion的证书不会有影响，因为它们是两个独立的证书。

   

  如果是做了全部证书替换，就要去VIEW里面接受一下变更

   

  另外， 如果VC的web ssl证书变更了， 到VIEW管理界面中的VC属性窗口里验证下证书就行了。

   

  VC换了证书，在VIEW里面的VC的连接位置，点开VC的连接，它会自动校验证书，

   

  点一下接受就可以了

  VCSA更新了证书也不会有什么单点登录的问题。

   

  出现下面这种情况是因为有的证书不需要更新后跳过了。

   

  ==============================================

  如果更新完证书后VCSA还是有关于证书状态的错误，如下图：

  ![[VMware-排错_vCenter_排查_006_VCSA 证书过期_006.png]]

  通过下面KB解决：

  <https://kb.vmware.com/s/article/82560>

  1.  Check the certificates with the following command: 

  for i in \$(/usr/lib/vmware-vmafd/bin/vecs-cli store list); do echo STORE \$i; /usr/lib/vmware-vmafd/bin/vecs-cli entry list \--store \$i \--text \| egrep \"Alias\|Not After\"; done

  \# 运行 for 命令后可以看到有 backup_store证书存在，而且有过期的。

  ![[VMware-排错_vCenter_排查_006_VCSA 证书过期_007.png]]

   使用下面KB方式解决：

  1.  If there are expired or Expiring Certificates under STORE BACKUP_STORE please proceed to run the attached Script clean_backup_stores.sh

  <!-- -->

  1.  Upload the attached script to the VCSA or external PSC to the /tmp/ directory.
  2.  Make the script executable by running: chmod +x clean_backup_stores.sh
  3.  Run the script with the command: ./clean_backup_stores.sh
  4.  Note: You may receive an error: -bash: ./clean_backup_stores.sh: /bin/bash\^M: bad interpreter: No such file or directory. If so, run: sed -i -e \'s/\\r\$//\' clean_backup_stores.sh
  5.  Restart services with command  service-control \--stop \--all && service-control \--start \--all

  在运行 ./clean_backup_stores.sh 时会有个提示，提醒是否已经做了VCSA的快照，如果有多个VCSA连在一起的，就必须做快照或关机。

  ![[VMware-排错_vCenter_排查_006_VCSA 证书过期_008.png]]

  输入 y 后开始删除那些backup的证书。

  重启服务

  ![[VMware-排错_vCenter_排查_006_VCSA 证书过期_009.png]]

   

  再次运行 for 命令可以看到  backup_store证书已经被删除。

  ![[VMware-排错_vCenter_排查_006_VCSA 证书过期_010.png]]

  ================ done =================

  补充：

  如果删除根证书（root证书）

  <https://kb.vmware.com/s/article/2146011>

  ================ done =================

  关于更新证书后又回退到之前的证书问题。

  更新更新完后，通过下面的工具检测一下。 在网页附件里

  <https://kb.vmware.com/s/article/80469?lang=en_us>

  Lookup Service 医生（lsdoctor）是一种工具，用于解决 PSC 数据库中存储的数据以及 vCenter 的本地数据（无论 PSC 是外部还是嵌入式）的问题。  该工具可用于检测和更正可能导致拓扑更改失败的问题（融合、重新指向等）、升级或因维护而产生的失败（例如，错误地应用新的 SSL 证书）。  本文将概述其功能和使用情况。 

  \# chmod +755 lsdoctor.py 

  \# python ./lsdoctor.py \--lscheck

  or

  \# python lsdoctor.py -l

  ![[VMware-排错_vCenter_排查_006_VCSA 证书过期_011.png]]

  其它更多使用请看KB

  ================ done =================

 

已使用 OneNote 创建。
