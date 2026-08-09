Linux下对BIOS卡与SSD检查

2018年11月29日

16:56

 

KB：

 

HOW12982

SLN310056

SLN308893

 

 

1. BIOS卡学习，具体看KB。

<https://kb.dell.com/infocenter/index?page=content&id=SLN308893&viewlocale=zh_CN>

 

2. BIOSS卡在下面版本有bug，需要升级到最新的FW版本，具体看KB。

<https://kb.dell.com/infocenter/index?page=content&id=SLN310056>

 

3. 在Linux下通过MVCLI 来测试SSD的状态，具体在Linux的操作如下面的KB。

<https://kb.dell.com/infocenter/index?page=content&id=HOW12982>

 

Linux ：

1) Download latest MVCLI from [http://dell.com/support](http://dell.com/support)

2) Transfer the .ZIP using your preferred SCP application (eg, WinSCP) to the /tmp directory on the linuxhost

3) SSH into the linux host 

4) Navigate to  /tmp

 

[\[root@localhost \~\]# cd /tmp]

 

4) Unzip the archive (file name may be different based on version)

 

[\[root@localhost tmp\]# unzip mvcli\\ 4.1.13.31_A01.zip]

 

5) Navigate to installation directory (directory name may be different vased on version) :

 

[\[root@localhost tmp\]# cd /tmp/mvcli\\ 4.1.13.31_A01/i386/cli]

 

6) Apply executable permission (must be done using root or sudo) :

 

[\[root@localhost cli\]# chmod +x install.sh mvcli]

 

信息输出：

[\[root@localhost \~\]# mvcli smart -p 0] \> mvcli.txt[    \# ]此输出的mvcli.txt需要提供给我们

 

[\[root@localhost \~\]# mvcli event] \> mvcli-2.txt[  \# ]此输出的mvcli-2.txt需要提供给我们

 

已使用 OneNote 创建。
