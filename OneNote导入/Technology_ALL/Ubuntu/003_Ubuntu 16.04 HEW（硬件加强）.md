Ubuntu 16.04 HEW（硬件加强）

2018年3月20日

14:56

- <https://certification.ubuntu.com/hardware/201709-25726/>

  Option Support Statement

  The following optional adapters for this server are not currently supported by 16.04 LTS via either the 4.4 or 4.10 kernels:

  Dell EMC PERC10 RAID Controllers -- H740p, H840

  Qlogic NIC's -- QL41264, QL41164, QL41262

  Support for these optional adapters will be available in the 16.04.4 HWE kernel.

   

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

   

  <https://wiki.ubuntu.com/Kernel/LTSEnablementStack>

  Ubuntu 16.04 LTS - Xenial Xerus

  The 16.04.2 and newer point releases will ship with an updated kernel and X stack by default for the desktop. Server installations will default to the GA kernel and provide the enablement kernel as optional.

  The 16.04 HWE Stacks will follow a new Rolling Update Model as documented at the following location:

  [https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)

  It is highly recommended to read the above documentation before executing the following commands, as the HWE model has changed in 16.04.

  Installing the HWE stack is simple:

  DESKTOP

   sudo apt-get install \--install-recommends linux-generic-hwe-16.04 xserver-xorg-hwe-16.04 

  SERVER

   sudo apt-get install \--install-recommends linux-generic-hwe-16.04 

   

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

  <https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack#hwe-16.04>

   

   

  <http://ywnz.com/linuxxz/446.html>

   

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

   

   

  <http://cdimage.ubuntu.com/netboot/16.04/?_ga=2.54696747.1695914675.1521526224-2082666710.1516843062>

   

   

  Ubuntu 16.04 LTS (Xenial Xerus) Netboot

  For advice on using netboot images, see the [installation guide](https://help.ubuntu.com/16.04/installation-guide/). These are generally aimed at experienced users with special requirements.

  Select an architecture to install 16.04 with Xenial\'s 4.4 GA kernel

  - [amd64](http://archive.ubuntu.com/ubuntu/dists/xenial-updates/main/installer-amd64/current/images/netboot/) - For 64-bit Intel/AMD (x86_64)
  - [i386](http://archive.ubuntu.com/ubuntu/dists/xenial-updates/main/installer-i386/current/images/netboot/) - For 32-bit Intel/AMD (x86)
  - [arm64](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-arm64/current/images/netboot/) - For 64-bit ARM (ARMv8)

  <!-- -->

  - armhf ([generic](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-armhf/current/images/generic/netboot/), [generic-lpae](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-armhf/current/images/generic-lpae/netboot/)) - For 32-bit ARM (ARMv7)

  <!-- -->

  - [ppc64el](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-ppc64el/current/images/netboot/) - For Little-Endian PowerPC (POWER8)

  <!-- -->

  - powerpc ([32-bit](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-powerpc/current/images/powerpc/netboot/), [64-bit](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-powerpc/current/images/powerpc64/netboot/), [e500mc](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-powerpc/current/images/e500mc/netboot/)) - For Big-Endian PowerPC

  <!-- -->

  - [s390x](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-s390x/current/images/generic/) - For IBM System z

  Ubuntu 16.04 LTS (Xenial Xerus) Netboot with HWE kernel

  Ubuntu provides backport kernels for hardware enablement on our LTS point releases, as well as matching netboot images. Below are links to the netboot images for 16.04 with the rolling HWE kernel. For more information about backport kernels, including the support and upgrade policies, please see the [documentation](https://wiki.ubuntu.com/Kernel/LTSEnablementStack).

  Select an architecture to install 16.04 with the rolling HWE kernel

  - [amd64](http://archive.ubuntu.com/ubuntu/dists/xenial-updates/main/installer-amd64/current/images/hwe-netboot/) - For 64-bit Intel/AMD (x86_64)[   \<== ]点击出现下面链接
  - [i386](http://archive.ubuntu.com/ubuntu/dists/xenial-updates/main/installer-i386/current/images/hwe-netboot/) - For 32-bit Intel/AMD (x86)
  - [arm64](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-arm64/current/images/hwe-netboot/) - For 64-bit ARM (ARMv8)

  <!-- -->

  - armhf ([generic](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-armhf/current/images/hwe-generic/netboot/), [generic-lpae](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-armhf/current/images/hwe-generic-lpae/netboot/)) - For 32-bit ARM (ARMv7)

  <!-- -->

  - [ppc64el](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-ppc64el/current/images/hwe-netboot/) - For Little-Endian PowerPC (POWER8)
  - [s390x](http://ports.ubuntu.com/ubuntu-ports/dists/xenial-updates/main/installer-s390x/current/images/hwe-generic/) - For IBM System z

   

   

   

   

  <http://archive.ubuntu.com/ubuntu/dists/xenial-updates/main/installer-amd64/current/images/hwe-netboot/>

  ![[Technology_ALL_Ubuntu_003_Ubuntu 16.04 HEW（硬件加强）_001.png]]

   

 

已使用 OneNote 创建。
