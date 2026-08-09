\[suse\]boot.txt

Wednesday, October 28, 2015

2:02 PM

#==\[ Command \]======================================#

\# /bin/uname -a

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-default-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-default-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-default-base-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-default-base-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-default-devel-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-firmware-20110923-0.42.49

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-syms-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-trace-devel-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-xen-devel-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-default-3.0.101-0.35.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-default-base-3.0.101-0.35.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-default-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-default-base-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-syms-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-default-devel-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-trace-devel-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-xen-devel-3.0.76-0.11.1

\--

#==\[ Verification \]=================================#

\# rpm -V kernel-firmware-20110923-0.42.49

\--

#==\[ Verification \]=================================#

\# rpm -V grub-0.97-162.168.25

\--

#==\[ Configuration File \]===========================#

\# /etc/grub.conf - File not found

\--

#==\[ Configuration File \]===========================#

\# /boot/grub/menu.lst - File not found

\--

#==\[ Configuration File \]===========================#

\# /boot/grub/device.map - File not found

\--

#==\[ Command \]======================================#

\# /usr/sbin/hwinfo \--framebuffer

\--

#==\[ Verification \]=================================#

\# rpm -V elilo-3.14-0.30.3

\--

#==\[ Command \]======================================#

\# /bin/ls -l \--time-style=long-iso /usr/lib64/efi

\--

#==\[ Configuration File \]===========================#

\# /boot/efi/efi/SuSE/elilo.conf

\--

#==\[ Configuration File \]===========================#

\# /etc/elilo.conf.old

\--

#==\[ Configuration File \]===========================#

\# /etc/elilo.conf

\--

#==\[ Command \]======================================#

\# /usr/bin/cksum /usr/lib64/efi/elilo.efi /boot/efi/efi/SuSE/elilo.efi

\--

#==\[ Command \]======================================#

\# /usr/sbin/efibootmgr -v

\--

#==\[ Command \]======================================#

\# /usr/sbin/mcelog \--ignorenodev \--filter \--dmi

\--

#==\[ Command \]======================================#

\# /usr/bin/last -xF \| egrep \"reboot\|shutdown\|runlevel\|system\"

\--

#==\[ Configuration File \]===========================#

\# /proc/cmdline

\--

#==\[ Configuration File \]===========================#

\# /etc/sysconfig/kernel

\--

#==\[ Configuration File \]===========================#

\# /etc/inittab

\--

#==\[ Configuration File \]===========================#

\# /etc/init.d/boot.local

\--

#==\[ Configuration File \]===========================#

\# /etc/init.d/before.local - File not found

\--

#==\[ Configuration File \]===========================#

\# /etc/init.d/after.local - File not found

\--

#==\[ Configuration File \]===========================#

\# /etc/init.d/halt.local

\--

#==\[ Command \]======================================#

\# /bin/ls -lR \--time-style=long-iso /boot/

\--

#==\[ Command \]======================================#

\# /bin/zcat /boot/initrd-3.0.101-0.35-default \| cpio -itv 2\>/dev/null

\--

#==\[ Command \]======================================#

\# /bin/zcat /boot/initrd-3.0.76-0.11-default \| cpio -itv 2\>/dev/null

\--

#==\[ Configuration File \]===========================#

\# /var/log/boot.msg

\--

#==\[ Configuration File \]===========================#

\# /var/log/boot.omsg

\--

#==\[ Command \]======================================#

\# /bin/dmesg

 

已使用 OneNote 创建。
