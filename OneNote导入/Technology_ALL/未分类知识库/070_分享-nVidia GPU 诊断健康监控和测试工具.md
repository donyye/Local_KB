分享-nVidia GPU 诊断健康监控和测试工具

星期四, 2025年11月13日

下午 1:52

分享-nVidia GPU 诊断健康监控和测试工具-DCGM常用命令使用(Fieldiag 629 Diagnostics备用)

 

[ ]呈上nVidia GPU 的Datacenter GPU Manager 常用命令DCGMi使用,谢幕分享请查阅.

 

•        因NDA相关原因,Fieldiag 629 Diagnostics可能不再对一线和客户可用(DE可用)(KB 000211613)

•        该工具适用datacenter/Tesla/GeForce等等多种系列GPU 和多种linux环境.https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/getting-started.html#supported-platforms

•        DCGMi Tool 该工具 PCIe-based GPUs建议使用.

•        建议配合系统命令(demsg/lspci等)及nvdia-smi 命令使用.

 

该工具的diagnostic部分可用于识别类/ECC 类/健康监控/修复验证/stree压测.....

 

BTW: 新版SLI (r9.6)已发布,可以内部适用,适用17G(Intel /AMD). 对应(nVidia/amd/Intel)GPU的如r9.6 -nvgpuiso 已发布

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_001.png]]

 

安装与运行参考: [https://www.dell.com/support/kbdoc/000219485](https://www.dell.com/support/kbdoc/000219485) PowerEdge: NVIDIA DataCenter GPU Manager (DCGM) install and how to run diagnostics

 

A 功能概要,如其中的Monitoring/health check /diagnostics/stress

<https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/index.html#focus-areas>

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_002.png]]

 

 

B DCGM Diag 四个run level,测试项目及大概运行时间如下

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_003.png]]

 

 

C 测试及输出(r9.6 /lmc R760xa with A30)

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_004.png]]

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_005.png]]

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_006.png]]

 

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_007.png]]

 

 

#dcgm health -c[  (]该GPU0/1 为PCIe ,非nvlink 可忽略该nvlink 提示)

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_008.png]]

 

 

 

迭代3次测试 runlevel 2

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_009.png]]

 

 

GPU[  Memory bandwidth ]测试

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_010.png]]

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_011.png]]

 

 

GPU Memory test0\~10 测试,适用GPU ECC等memory 相关报错 [https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/dcgm-diagnostics.html#test-descriptions](https://docs.nvidia.com/datacenter/dcgm/latest/user-guide/dcgm-diagnostics.html#test-descriptions)

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_012.png]]

 

 

Power测试5min

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_013.png]]

 

 

测试指定GPU

 

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_014.png]]

 

 

运行dcgm 测试时nvidia-smi的状态信息输出,(可以看到当前执行进程是NVIDIA Validation Suite (NVVS) ,dcgm以前的叫法)

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_015.png]]

 

 

测试log相关,如nvvs log, 测试时无显示,一般建议使用如下方式打包成bundle log

By default, DCGM emits debugging information into logs that are stored under /var/log/nvidia-dcgm/nvvs.log.

 

To capture console output from dcgmi diag, you must redirect output to a file which I would suggest doing with the tee command:

#dcgmi diag -r \# \| tee -a /path/to/file.log

If you would like to get all the log files generated as a bundle you could use the following examples:

#dcgmi diag -r 4 \| tee -a /var/log/nvidia-dcgm/dcgmi-diag-results.log

#tar czvf dcgmi-diag-logs-\$(date +%Y%m%d%H%M%S).tgz /var/log/nvidia-dcgm/

The example above would save console results of dcgmi diag in /var/log/nvidia-dcgm/ and then create a compressed tar/gz bundle of those files, with a timestamped filename.

 

In the tar command above, this assumes dcgmi diag -r 4 in which all those files would be generated. You may get errors about files not found if you run as is above with a lower run level resulvt.

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_016.png]]

 

 

Nvvs log示例

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_017.png]]

 

 

 

Dcgm测试报错log示例

![[Technology_ALL_未分类知识库_070_分享-nVidia GPU 诊断健康监控和测试工具_018.png]]

 

 

 

附:

<https://developer.nvidia.com/dcgm#Downloads>

<https://github.com/NVIDIA/DCGM>

<https://www.dell.com/support/kbdoc/000202730> Dell PowerEdge - Process to engage NVIDIA for support on OEM cards - Enterprise customer

<https://www.dell.com/support/kbdoc/000211613>  PowerEdge: NVIDIA Field Diagnostics/DCGMi Tool for PCIe GPU type - Non HGX systems

 

GPU 诊断工具用法和备选指导

重要提示：由于与 NVIDIA 签订了保密协议 （NDA），Fieldiag （629 Diagnostics） 工具无法再直接分发给一线团队或客户。

工具访问策略

•        只有 域工程师 （DE） 可以根据具体案例要求确定是否需要 Fieldiag 工具，以及是否可以与客户共享该工具。

•        持有 NVIDIA 直接许可协议 的客户可以直接联系 NVIDIA 支持 ，请求访问 Fieldiag 工具。

 

NVIDIA Datacenter GPU Management (DCGM) Diagnostics

<https://dellservices.lightning.force.com/lightning/app/c__Lightning_Knowledge/articles/Lightning_Kn…>

 

XE GPU platforms \| Running Nvidia baseboard field diagnostics on XE (HGX) platforms

<https://dellservices.lightning.force.com/lightning/app/c__Lightning_Knowledge/articles/Lightning_Kn…>

 

nVidia GPU Field diagnostics:

<https://dellservices.lightning.force.com/lightning/app/c__Lightning_Knowledge/articles/Lightning_Kn…>

 

XE GPU platforms Nvidia GPU missed issue troubleshooting step

<https://dellservices.lightning.force.com/lightning/app/c__Lightning_Knowledge/articles/Lightning_Kn…>

 

Dell EMC - XE Series x4/x8 GPU accelerator Landing Page

<https://dellservices.lightning.force.com/lightning/app/c__Lightning_Knowledge/articles/Lightning_Kn…>

 

已使用 OneNote 创建。
