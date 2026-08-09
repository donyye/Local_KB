\[Case Share\] R750 with PCIE 80G A100 GPU, Ubuntu OS(20.04 and 18.04) install GPU driver and then black screen.\-\-\--LKB#000194569

2022年5月24日

17:24

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

 

Ubuntu server 20.04/18.04 +R750+PCIe 80G A100 GPU

 

1, we have checked the TSR log and found the GPU can be detected and no any hardware error record.

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

 

2, We have tried to install different version drivers(470.xx/460.xx/450.xx), still cannot boot into os after installed the driver.

[https://www.nvidia.com/Download/driverResults.aspx/182612/en-us](https://www.nvidia.com/Download/driverResults.aspx/182612/en-us) 

 

We installed driver 460.xx, we run the nvidia-smi command will show no device found.

We installed driver 450.xx, we run the nvidia-smi command will show no command.

 

Release notes show A100 80G PCIe is new support on 470.82

Added support for the following NVIDIA GPU products:

- NVIDIA A16
- NVIDIA A100 80 GB PCIe

3, We have installed the ubuntu server 20.04 and ubuntu server 18.04, still cannot fix the issue.

 

4, We have installed the driver 470.82 on ubuntu desktop 20.04 and then run nvidia-smi, it will cause kernel panic and cannot boot into os,we need to boot into multi-user mode and removed the GPU driver.

![[Technology_ALL_Linux 问题收集_076_[Case Share] R750 with PCIE 80G A100 GPU,_001.jpg]]

 

![[Technology_ALL_Linux 问题收集_076_[Case Share] R750 with PCIE 80G A100 GPU,_002.jpg]]

 

5, Confirm BIOS secure boot is disabled.

![[Technology_ALL_Linux 问题收集_076_[Case Share] R750 with PCIE 80G A100 GPU,_003.jpg]]

 

6, We follow below guide to install 470 driver.

a, Install Ubuntu 20.04.03 with HWE kernel

b, apt update/apt upgrade

c, apt install nvidia-headless-no-dkms-470-server

d, apt install nvidia-utils-470-server

e, apt install nvidia-dkms-470-server

 

Run nvidia-smi will show"NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver, Make sure that the latest NVIDIA driver is installed and running "

![[Technology_ALL_Linux 问题收集_076_[Case Share] R750 with PCIE 80G A100 GPU,_004.jpg]]

 

7, we have replaced the GPU, MB and GPU power cable, still cannot fix the issue

8,  We use DELL SLI3.0(centos) boot the server and install GPU driver, no issue found.

![[Technology_ALL_Linux 问题收集_076_[Case Share] R750 with PCIE 80G A100 GPU,_005.jpg]]

 

9, We installed RHEL7.9 and then install GPU driver,  no issue found. Run GPU fieldiag tool, hardware pass. No hardware issue.

![[Technology_ALL_Linux 问题收集_076_[Case Share] R750 with PCIE 80G A100 GPU,_006.jpg]]

 

10, We installed Ubuntu Server 18.04 and installed the 470.82.01 deb driver,  Still show"NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver, Make sure that the latest NVIDIA driver is installed and running "

 

11,  we try to add "pci=realloc=off" to grub and issue gone.

![[Technology_ALL_Linux 问题收集_076_[Case Share] R750 with PCIE 80G A100 GPU,_007.jpg]]

 

 

Resolution：

To ensure option NVIDIA GPUs and PCIe network adapters communicate with the appropriate driver, add the kernel parameter at boot time: \"pci=realloc=off\" as shown below:

1.  Open /etc/default/grub using a text editor (for example, vi /etc/default/grub)
2.  After the \"GRUB_CMDLINE_LINUX_DEFAULT=\", add \"pci=realloc=off\" in quotation marks as shown below:

![[Technology_ALL_Linux 问题收集_076_[Case Share] R750 with PCIE 80G A100 GPU,_008.jpg]]

1.  Save and close the file, and run the command \"update-grub\" as follows:

![[Technology_ALL_Linux 问题收集_076_[Case Share] R750 with PCIE 80G A100 GPU,_009.jpg]]

1.  Reboot the system. The driver should be able to communicate with all GPUs on the next boot.

Note: Red Hat Enterprise Linux and CentOS have this parameter set to \"off\" by default, whereas Ubuntu has this parameter set to \"on\" by default.

 

 

 

Jack Wong

Senior Engineer \| China - ISG Global Network and Compute

Dell Technologies \| APJC ISG Support Services

Office +86 0592 818 6526 EXT. 8886526

[Jack_wong1@dell.com](mailto:Jack_wong1@dell.com)

![[Technology_ALL_Linux 问题收集_076_[Case Share] R750 with PCIE 80G A100 GPU,_010.jpg]]

How am I doing? Please contact my manager <Xing_Fang_Wang@DELL.com>to provide feedback. Thanks!

 

Internal Use - Confidential

 

已使用 OneNote 创建。
