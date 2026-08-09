[ ]在Windows 2012/R2 下使用了4KN Disk 后出现 BSOD System thread exception not handled (wppRecorder.sys) 问题的解决办法

Monday, March 06, 2017

5:01 PM

  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       Share:[  ]在Windows 2012/R2 下使用了4KN Disk 后出现 BSOD System thread exception not handled (wppRecorder.sys) 问题的解决办法
  发件人     Yin, Guoxun
  收件人     CN XMN TS ENT L2 SME
  发送时间   Monday, March 06, 2017 4:43 PM
  -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Hi Team,

目前已经在4KN 的硬盘上安装Windows 2012 ，会随机出现NTFS  compression被启用从而导致的蓝屏， stop exception 字段的提示为 "BSOD System thread exception not handled (wppRecorder.sys) "

请先检查使用的硬盘是否为4KN磁盘，该报错在非4KN 磁盘上可能是其他原因导致，具体情况具体分析。

出现该问题后，系统会无休止的反复蓝屏重启，安全模式无法进入，最后一次正确配置无法生效。 解决办法为先使用workaround恢复系统启动，然后进入系统后再安装相应的hotfix以根除该问题.

 

Woraround如下：

 

1\) Boot to the recovery console (you should automatically get there after a few Blue Screen crashes)

2\) Launch command prompt

3\) Run the following command:

     c:\\windows\\system32\\compact.exe /U c:\\windows\\system32\\drivers\\\*.sys

4\) Reboot

5\) As soon as you successfully boot, disable NTFS compression system-wide so that the CBS Scavenger does not reintroduce the issue again:

     fsutil behavior set DisableCompression 1

6\) Reboot again (so the DisableCompression setting takes effect)

 

 

进入系统后，安装以下补丁：

\"0x00000024\" Stop error in FsRtlNotifyFilterReportChange and copy file may fail in Windows

[https://support.microsoft.com/en-us/help/3121255](https://support.microsoft.com/en-us/help/3121255)

Ntfs.sys    6.3.9600.18183

 

 

\"0x0000003B\" or \"0x0000007E\" Stop error on a Windows-based computer that has 4K sector disks

[https://support.microsoft.com/en-us/help/3027108](https://support.microsoft.com/en-us/help/3027108)

 

 

已使用 OneNote 创建。
