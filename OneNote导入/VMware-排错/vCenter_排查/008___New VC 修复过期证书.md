\|\_New VC 修复过期证书

2024年5月22日

14:22

1.  先使用下面KB的命令查看那些证书过期了

<https://knowledge.broadcom.com/external/article?legacyId=82332>

 

for store in \$(/usr/lib/vmware-vmafd/bin/vecs-cli store list \| grep -v TRUSTED_ROOT_CRLS); do echo \"\[\*\] Store :\" \$store; /usr/lib/vmware-vmafd/bin/vecs-cli entry list \--store \$store \--text \| grep -ie \"Alias\" -ie \"Not After\";done;

 

 

1.  通过脚本来更新证书

<https://knowledge.broadcom.com/external/article?articleNumber=322249>

 

python fixcerts.py replace \--certType expired_only[   ]只更新过期的

python fixcerts.py replace \--certType all[      ]全都更新

 

1.  如果有 bak 证书残留解决，通过脚本

<https://knowledge.broadcom.com/external/article?legacyId=82560>

 

 

 

 

 

 

已使用 OneNote 创建。
