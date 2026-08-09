Share: ESXi 6.0 U2 升级U3后 VGPU可能无法工作

Tuesday, July 4, 2017

1:13 PM

  -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       Share: ESXi 6.0 U2 升级U3后 VGPU可能无法工作
  发件人     Yin, Guoxun
  收件人     Ye, Dony; Han, Ruyang; CN XMN TS ENT L2 SME
  发送时间   Tuesday, July 4, 2017 12:30 PM
  -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------

 

Hi Team,

请注意以下KB，ESXi 6.0 U2 升级到U3后 VGPU可能无法工作，表现为xorg service无法启动，提示如其中KB所述.

<https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2150498>

 

 \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

After upgrading ESXi hosts to ESXi600-201706001 Hardware 3D graphics functioning fails (2150498) 

Symptoms

After patching an ESXi 6.0.x host to the patch bulletin ESXi600-201706103-SG from ESXi600-201706001,you may notice following symptoms.

- After patching an ESXi 6.0.x host, you may noticed VMs are no longer utilizing vSGA or vGPU resources properly.
- In Horizon View, an automated VMs which use GRID or Hardware 3D settings fail to boot.
- From an SSH session the command /etc/init.d/xorg start shows:\
  \
  Error: Unknown command or namespace graphics host refresh

 

 

Resolution

This issue is seen in ESXi600-201706001.

 

Currently, there is no resolution.

 

To Workaround this issue.

1.  Download the attached file xorg_temp.zip from the below attachment.
2.  Extract the file within to a local temp directory.
3.  Login to ESXi through SSH.\
    \
    SCP the extracted file to the ESXi, using WinSCP.\
    \
    Place the file in /tmp
4.  Change the file mode of the existing xorg file\
    \
    chmod +wt /etc/init.d/xorg
5.  Rename the existing xorg file\
    \
    mv /etc/init.d/xorg /etc/init.d/xorg_old
6.  Place the new xorg file in the original location\
    \
    cp /tmp/xorg_temp /etc/init.d/xorg
7.  Change the file mode of the new file\
    \
    chmod 555 /etc/init.d/xorg
8.  Run the command to start xorg\
    \
    /etc/init.d/xorg start
9.  Confirm the status is running:\
    \
    /etc/init.d/xorg status

 

Note:Do not reboot the host at this point. If the host needs to be rebooted, perform the above steps once again.

Attachments 

- [xorg_temp.zip](javascript:openConsole('2150498',%20'xorg_temp.zip','_blank'))

Request a Product Feature

To request a new product feature or to provide feedback on a VMware product, please visit the [Request a Product Feature](http://www.vmware.com/contact/contactus.html?department=prod_request) page.

 

来自 \<[https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2150498](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2150498)\> 

 

 

 

已使用 OneNote 创建。
