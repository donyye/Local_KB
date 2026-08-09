Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU 

2024年2月19日

11:08

Cao Ting[  \--\> share\
\
share ubuntu22.04]系统安装cuda使用gpu_burn压测GPU

kb: 000219769

除了filediag外也可以使用这个工具来压测gpu性能和硬件是否正常

 

 

系统kernkel版本如下：

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_001.jpg]]

  

准备工作：

1：禁用nouveau驱动

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_002.jpg]]

 

添加如下内容：

blacklist nouveau

options nouveau modest=0

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_003.jpg]]

 

修改为多用户模式：

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_004.jpg]]

 

 Reboot之后确保nouveau驱动模块不会加载

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_005.jpg]]

 

2：配置源后安装开发工具包：

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_006.jpg]]

 

 

3：

下载cuda：

<https://developer.nvidia.com/cuda-toolkit-archive>

下载gpu_burn:

<https://github.com/wilicc/gpu-burn>

 

4：添加执行权限后./cudaxxxx.run

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_007.jpg]]

 

选择accept下一步：

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_008.jpg]]

 

5: cuda有包含GPU驱动，如果系统已经安装过GPU驱动，可以不用勾选驱动；也可以选择安装当前更新版本的GPU驱动

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_009.jpg]]

 

6：安装完成后如下：

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_010.jpg]]

 

7：配置环境变量后确保NVCC --version正常输出：

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_011.jpg]]

 

添加内容如下：

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_012.jpg]]

 

8:检查GPU情况：

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_013.jpg]]

 

9：上传gpu_burn make生成gpu_burn程序：

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_014.jpg]]

 

 10：运行gpu_burn 测试：

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_015.jpg]]

 

压测同时查看 GPU使用状态：

 

![[Technology_ALL_Linux 问题收集_098_Ubuntu22.04 LTS 安装CUDA并使用gpu_burn 压测GPU_016.jpg]]

 

 

 

 

 

已使用 OneNote 创建。
