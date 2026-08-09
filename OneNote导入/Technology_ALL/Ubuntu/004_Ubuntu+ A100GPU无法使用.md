Ubuntu+ A100GPU无法使用

2022年10月28日

11:03

  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       [\[Case Share\] R750 with PCIE 80G A100 GPU, Ubuntu OS(20.04 and 18.04) install GPU driver and then black screen.\-\-\--LKB#000194569]
  发件人     Wong1, Jack
  收件人     CN XMN TS ENT L2 SME
  抄送       Xu, Xiaofeng; Huang, Antti
  发送时间   2021年12月16日 17:59
  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Thanks IPS Xiaofeng and SST Antti for great support.

 

 

Summary:  R750 with PCIE 80G A100 GPU, Ubuntu OS install GPU driver and then black screen.

 

Symptoms:

R750 with PCIE 80G A100 GPU, we cannot find hardware error record for this GPU from TSR log

We have tried to install the GPU A100 Driver and OS cannot boot, show black screen.

R750 与 PCIE 80G A100 GPU，我们无法从 TSR 日志中找到此 GPU 的硬件错误记录

我们尝试安装 GPU A100 驱动程序，操作系统无法启动，显示黑屏。

 

 

Ubuntu server 20.04/18.04 +R750+PCIe 80G A100 GPU

 

1、我们检查了TSR日志，发现可以检测到GPU，没有任何硬件错误记录。

PCI Devices

Bus:Dev:Func Vendor Description Location

[000 : 17 : 05 Intel Corporation C620 Series Chipset Family SSATA Controller \[AHCI mode\] AHCI.Embedded.1-1]

[000 : 23 : 00 Intel Corporation C620 Series Chipset Family SATA Controller \[AHCI mode\] AHCI.Embedded.2-1]

000 : 31 : 04 Intel Corporation C620 Series Chipset Family SMBus SMBus.Embedded.3-1

003 : 00 : 00 Matrox Electronics Systems Ltd. Integrated Matrox G200eW3 Graphics Controller Video.Embedded.1-1

004 : 00 : 00 Broadcom Inc. and subsidiaries PowerEdge Rx5xx LOM Board NIC.Embedded.1-1-1

004 : 00 : 01 Broadcom Inc. and subsidiaries PowerEdge Rx5xx LOM Board NIC.Embedded.2-1-1

049 : 00 : 00 Broadcom / LSI PERC H745 Front RAID.SL.7-1

050 : 00 : 00 Broadcom Inc. and subsidiaries BCM57412 NetXtreme-E 10Gb RDMA Ethernet Controller NIC.Integrated.1-1-1

050 : 00 : 01 Broadcom Inc. and subsidiaries BCM57412 NetXtreme-E 10Gb RDMA Ethernet Controller NIC.Integrated.1-2-1

101 : 00 : 00 KIOXIA Corporation Dell Ent NVMe CM6 RI 3.84TB Disk.Bay.7:Enclosure.Internal.0-1

102 : 00 : 00 KIOXIA Corporation Dell Ent NVMe CM6 RI 3.84TB Disk.Bay.6:Enclosure.Internal.0-1

[202 : 00 : 00 NVIDIA Corporation GA100 \[A100 PCIe 80GB\] Video.Slot.7-1]

 

2、我们试过安装不同版本的驱动（470.xx/460.xx/450.xx），安装驱动后仍然无法开机进入操作系统。

[https://www.nvidia.com/Download/driverResults.aspx/182612/en-us](https://www.nvidia.com/Download/driverResults.aspx/182612/en-us) 

 

We installed driver 460.xx, we run the nvidia-smi command will show no device found.

We installed driver 450.xx, we run the nvidia-smi command will show no command.

 

Release notes show A100 80G PCIe is new support on 470.82

Added support for the following NVIDIA GPU products:

- NVIDIA A16
- NVIDIA A100 80 GB PCIe

 

3、我们已经安装了ubuntu server 20.04和ubuntu server 18.04，仍然无法解决问题。

 

4、我们在ubuntu desktop 20.04上安装了驱动470.82，然后运行nvidia-smi，会导致kernel panic，无法开机进入os，需要开机进入多用户模式，去掉GPU驱动。

![[Technology_ALL_Ubuntu_004_Ubuntu+ A100GPU无法使用_001.jpg]]

 

![[Technology_ALL_Ubuntu_004_Ubuntu+ A100GPU无法使用_002.jpg]]

 

5、确认 BIOS 安全启动已禁用。

![[Technology_ALL_Ubuntu_004_Ubuntu+ A100GPU无法使用_003.jpg]]

 

6，我们按照以下指南安装470驱动程序。

a, Install Ubuntu 20.04.03 with HWE kernel

b, apt update/apt upgrade

c, apt install nvidia-headless-no-dkms-470-server

d, apt install nvidia-utils-470-server

e, apt install nvidia-dkms-470-server

 

运行 nvidia-smi 会显示"NVIDIA-SMI has failed because it could not communicate with the NVIDIA driver, 请确保已安装并运行最新的 NVIDIA 驱动程序"

![[Technology_ALL_Ubuntu_004_Ubuntu+ A100GPU无法使用_004.jpg]]

 

7、我们更换了GPU、MB和GPU电源线，仍然无法解决问题

8、我们使用DELL SLI3.0(centos)启动服务器并安装GPU驱动，没有发现问题。

![[Technology_ALL_Ubuntu_004_Ubuntu+ A100GPU无法使用_005.jpg]]

 

9、我们安装了RHEL7.9，然后安装了GPU驱动，没有发现问题。 运行 GPU fieldiag 工具，硬件通过。 没有硬件问题。

![[Technology_ALL_Ubuntu_004_Ubuntu+ A100GPU无法使用_006.jpg]]

 

10、我们安装了Ubuntu Server 18.04并安装了470.82.01 deb驱动，仍然显示"NVIDIA-SMI has failed because it could not communicate with the NVIDIA driver, 请确保已安装最新的NVIDIA驱动并运行"

 

11，我们尝试在 grub 中添加"pci=realloc=off"，然后问题就消失了。

![[Technology_ALL_Ubuntu_004_Ubuntu+ A100GPU无法使用_007.jpg]]

 

 

Resolution：

为确保选项 NVIDIA GPU 和 PCIe 网络适配器与适当的驱动程序通信，请在启动时添加内核参数："pci=realloc=off"，如下所示：

1.  Open /etc/default/grub using a text editor (for example, vi /etc/default/grub)
2.  After the \"GRUB_CMDLINE_LINUX_DEFAULT=\", add \"pci=realloc=off\" in quotation marks as shown below:

![[Technology_ALL_Ubuntu_004_Ubuntu+ A100GPU无法使用_008.jpg]]

1.  Save and close the file, and run the command \"update-grub\" as follows:

![[Technology_ALL_Ubuntu_004_Ubuntu+ A100GPU无法使用_009.jpg]]

1.  Reboot the system. The driver should be able to communicate with all GPUs on the next boot.

Note: Red Hat Enterprise Linux 和 CentOS 默认将此参数设置为"off"，而 Ubuntu 默认将此参数设置为"on"。

Internal Use - Confidential

 

已使用 OneNote 创建。
