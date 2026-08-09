022-PSOD-TPM

2023年9月12日

17:01

022-LKB-000223702_2024-0402

 

ISSUE：

客户在更换cpu与主板后，重启系统出现 PSOD issue，如下图显示。

Unable to restore the system configureation. A security violation was detected.

![[记录_信息_LKB_记录_023_022-PSOD-TPM_001.png]]

 

Solution：

此问题与 TPM 有关系，请做下面检查。

1. 确认旧主板是否有开启TPM后安装的系统？

2. 如果之前主板有开启过，请在新的主板上开启 TPM 后导入 RecoveryKey 并重启。(具体方法请看KB)

3. 更换到旧的主板后重启尝试，或是考虑在新换的主板后重新安装系统。

4. 如果在更换的主板后有开启TPM并安装系统，之后又关闭的，也会出现此问题。

 

详细可以查看KB：

<https://kb.vmware.com/s/article/81446>

 

 

已使用 OneNote 创建。
