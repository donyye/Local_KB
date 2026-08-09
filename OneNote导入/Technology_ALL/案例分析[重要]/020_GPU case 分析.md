GPU case 分析

Tuesday, September 09, 2014

10:23 AM

1. GPU的电源连接、温度、电压等问题。

2. 系统是否支持，如P100 GPU支持CentOS7.2当支持RHEL7.3。这个需要注意。

3. vBIOS，其实是GPU的FW，但是比较少出现问题。

案例如：

这个KB提到P100的VBIOS有 know issue：

<https://kb.dell.com/infocenter/index?page=content&id=SLN307251&actp=LIST>

4. 驱动问题，安装驱动后系统X-windows不能用，进不了图形。

这个与安装驱动有关系。

 

5. 日志收集：收集此命令输出：nvidia-smi -q

\
\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--\
\
\
drive Dow：http://www.nvidia.cn/download/driverResults.aspx/77207/cn

 

GPU 测试软件(CUDA-Z): [http://cuda-z.sourceforge.net/](http://cuda-z.sourceforge.net/)

 

Drive upgrade 后解决的bug: [http://docs.nvidia.com/deploy/pdf/Dynamic_Page_Retirement.pdf](http://docs.nvidia.com/deploy/pdf/Dynamic_Page_Retirement.pdf)

 

GPU 在Dest 看不到Firmware，而且需要知道版本需要使用：

nvdia-debug-report 命令

IPS 处有Firmware。

 

已使用 OneNote 创建。
