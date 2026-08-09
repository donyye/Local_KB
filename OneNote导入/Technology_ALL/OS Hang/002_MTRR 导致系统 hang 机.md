MTRR 导致系统 hang 机

Thursday, December 12, 2013

4:47 PM

- 请在修改文件之前，先做好文件备份，以便可以恢复到当前状态。

  CHECK/检查

  1 -- 使用cat 命令检查message 文件。

  \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

  \# cat /var/log/message\* \| grep --I MTRR

  kernel: mtrr: no more MTRRs available ç 表示系统中有启用MTRR 功能

  kernel: mtrr: no more MTRRs available

  kernel: mtrr: no more MTRRs available

  kernel: mtrr: no more MTRRs available

  \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

  2 -- 检查Xorg.0.log 文件

  \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

  \# cat /var/log/Xorg.0.log \| grep --I combin

  (==) MGA(0): Write-combining range (0xf1000000,0x800000) ç 表示系统启用

  MTRR 功能成功

  (==) MGA(0): Write-combining range (0xf1000000,0x800000)

  \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

  3 -- 检查mtrr 文件

  \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*cat

  \# cat /proc/mtrr

  \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

   

  RESOLUTION/解决方案

  1 - 由于死机会出现两个位置，针对两个位置修改不同的配置文件。

  针对在引导kernel 过程出现的死机，修改/boot/grub/grub.conf 文件，再其引导过程中，规

  避调用MTRR。

  \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

  \# vim /boot/grub/grub.conf

  title Red Hat Enterprise Linux Server (2.6.18-194.el5)

  [            ]root (hd0,0)

  [             ]kernel /vmlinuz-2.6.18-194.el5 ro root=/dev/VolGroup00/LogVol00 rhgb quiet

  [             ]initrd /initrd-2.6.18-194.el5.img

  // remove rhgb and quiet parameter from this grub.conf, and add nofb.

  title Red Hat Enterprise Linux Server (2.6.18-194.el5)

  [             ]root (hd0,0)

  [             ]kernel /vmlinuz-2.6.18-194.el5 ro root=/dev/VolGroup00/LogVol00 nofb

  [             ]initrd /initrd-2.6.18-194.el5.img

  \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

  针对在从字符见面转换到图形界面kernel 过程出现的死机，修改/boot/grub/grub.conf 文件，

  再其引导过程中，规避调用MTRR。

  Second, I have edit /etc/X11/xorg.conf

  \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

  \# vim /etc/X11/xorg.conf

  Section \"Device\"

  Identifier \"Videocard0\"

  Driver \"mga\"

  EndSection

   

  Section \"Screen\"

  Identifier \"Screen0\"

  Device \"Videocard0\"

  DefaultDepth 24

  SubSection \"Display\"

  Viewport 0 0

  Depth 24

  EndSubSection

  EndSection

  // To change setting on video driver to vesa, and add driver option parameter "ShadowFB" "0".

  In recent change, I have add a new option parameter "NoMTRR" in the screen's section.

  Section \"Device\"

  Identifier \"Videocard0\"

  Driver \"vesa\"

  Option \"ShadowFB\" \"0\"

  EndSection

   

  Section \"Screen\"

  Identifier \"Screen0\"

  Device \"Videocard0\"

  DefaultDepth 24

  Option \"NoMTRR\"

  SubSection \"Display\"

  Viewport 0 0

  Depth 24

  Modes \"800x600\" \"640x480\"

  EndSubSection

  EndSection

  \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*

  验证配置：

  1 - 重启完毕，检查/var/log/Xorg.0.log，如果没有一下类似信息，即表明设定成功。

  (==) MGA(0): Write-combining range (0xf1000000,0x800000)

   

   

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

  \### Sosreport \###

  检查/var/log/messages，发现系统有MTRR功能

  kernel: mtrr: no more MTRRs available

  kernel: mtrr: no more MTRRs available

  kernel: mtrr: no more MTRRs available

   

  检查/var/log/Xorg.0.log，发现write-combing功能被MTRR启用

  (==) MGA(0): Write-combining range (0xf1000000,0x800000)

  (==) MGA(0): Write-combining range (0xf1000000,0x800000)

   

   

  \### ISSUE \###

  机器启动会出现黑屏死机，无任何响应。

   

   

  综合以上信息，建议如下：

  1 - 由于服务器出现的故障现象是黑屏死机，并且在日志中，有表明启用MTRR(请参见附录1)。

  且CPU是隶属于Westmere-EX系列的，符合红帽知识库提及的问题65862([https://access.redhat.com/knowledge/solutions/65862)，请参加附录2](https://access.redhat.com/knowledge/solutions/65862)，请参加附录2)。

   

  调整方法，请参考附件的MTRR.PDF，修改/boot/grub/grub.conf和/etc/X11/xorg.conf 两个文件，从而禁用系统使用MTRR功能。

   

   

  2 -- 规避以上建议不能解决当前问题，建议启动kdump功能。

  以便在配置以上参数后，还会出现死机故障时，可以自动收集信息，在提交升级case到红帽原厂处理。

  Note：

  需要用户购买正版系统，才有权升级到红帽原厂，来取得技术支持。

   

   

  \~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~

   

  附录1

  MTRR是Memory Type Range Register的缩写，内存类型寄存器。

  它规定了读写某段范围物理内存的策略，用于优化CPU数据传送性能。

  Intel CPU的MTRR可以控制访问显存地址范围，启用write-combining功能，

  CPU能够在底层的PCI/AGP总线上，将"多次少量的数据写入"集合成"一次大的数据写入"，

  来大大的提高图形的能力。提高速度。

   

   

  附录2

  Some systems with large numbers of logical CPUs hang during boot intermittantly.

  last modified by Christopher Tatman on 11/16/11 - 13:59

  Issue

   

  - Some systems with a large number of CPUs (either physical or logical via hyperthreading) hang during boot intermittantly. Dell has reported this problem to exist on some of their Westmere-EX based Poweredge systems. However, the issue may not be restricted to Dell systems alone and could possibly affect other vendor hardware. 

  Environment

  - Red Hat Enterprise Linux 5.7 and older 
  - The issue has been reported on Westmere-EX based processors
  -  

  E7-8867L -- WSM-EX 2.13GHz 10C 105W

  E7-2870 -- WSM-EX 2.40GHz 10C 130W

  E7-2860 -- WSM-EX 2.26GHz 10C 130W

  E7-2850 -- WSM-EX 2.00GHz 10C 130W

  E7- 4870 - WSM-EX 2.4GHz 10C 130W

  Resolution

  - The fix is to institute a write memory barrier across the cpus and disable interrupts on the cpu which is issuing the IPI before the IPI is issues. The IPI handler will disable interrupts on each of the other cpus. 
  - This fix will be available in a future RHEL5 kernel.  Please contact your support representative for additional details. 

  Root Cause

  - When setmtrr() is sending an IPI to each of the other 79 CPU an interrupt occurs on the CPU that\'s running setmtrr().

  This interrupt handler gets stuck in \_rcuprocesscallbacks()waiting for the rcp-\>lock spinlock that is held by one of the other CPUs, which got interrupted by the IPI right after it

  acquired that spin lock.

  This is a deadlock situation.

  CPU B acquires the rcp spinlock.

  CPU A is sending IPI to all the other CPUS but not disabling interrupts for itself.

  CPU B is interrupted by the IPI while it has held on to the spin lock. The IPI handler is waiting for the gate count(critical variable to change before it can exit the IPI handler)

  CPU A is interrupted by another interrupt that needs the spin lock held by CPU B before it was interrupted by the IPI.

  CPU B is waiting for the gate count to change by CPU A and CPU A is waiting for the spin lock to be released by CPU B\>

   

   

   

  \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

  网络上找到的：

  MTRR: 看解释说write combining就是把总线上的传输合并成一个大传输，可以提高2.5倍的图像写速度，write back就是普通的，不合并。

  [http://www.linuxforum.net/forum/ \... &sb=5&o=all](http://www.linuxforum.net/forum/showflat.php?Cat=&Board=linuxK&Number=65103&page=191&view=collapsed&sb=5&o=all)

  也有人说MTRR这个东东过时了

  [http://www.nvnews.net/vbulletin/showthread.php?t=110418](http://www.nvnews.net/vbulletin/showthread.php?t=110418)

 

已使用 OneNote 创建。
