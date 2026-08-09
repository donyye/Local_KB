dracut \--force \--add-drivers/ RHEL 8

2019年9月2日

10:04

  ------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   FW: OOB driver problems w/ RHEL 8
  From      Lim YH, Patrick
  To        APJ Linux Support
  Sent      2019年8月30日 9:49
  ------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

解决方法：

解决方法：在安装后阶段，在安装Z-Stream内核并且系统仍处于chroot环境之后，将initrd重建为

     dracut \--force \--add-drivers"\<OOB_Kernel_module_name-1\> \<OOB_Kernel_module_name-2\> \..."/ boot / \<initramfs-kernel_version.img\>

  

示例： - 由于RHEL-8的z-stream内核为"4.18.0-80.7.1.el8_0.x86_64"，因此运行：

dracut -f \--add-drivers"megaraid_sas mpt3sas i40e"/boot/initramfs-4.18.0-80.7.1.el8_0.x86_64.img

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

具体内容如下：

 

Patrick Lim

Principal Engineer, Infrastructure & Client Solutions

Solutions Support Team (SST)

Dell EMC \| Support and Deployment Services

[Patrick.lim.yh@dell.com](mailto:Patrick.lim.yh@dell.com)

 

Working Hours: Monday ‒ Friday \| 09:00 ‒ 17:30 (MYSG Time)

Certifications: SAP HANA Associate \| CCAH Hadoop \| RHCE RHEL7 \| RHCSA OpenStack \| SCA Enterprise Linux 12,15

                       VCP6.5-DCV  \| CCNA \| NPP \| CCA for XenServer 6 \| Certified Professional SC & PS Series

 

 

 

From: Olsson, Kurt

Sent: Friday, August 30, 2019 2:32 AM

To: Coelho, Alcides; II, Richard Bayless; Leccardi, Diego; Lim YH, Patrick

Cc: Chuah, Sharene; Ermis, Josef; Smaili, Karim

Subject: OOB driver problems w/ RHEL 8

 

Hello team, let me know if you have questions / concerns about the below content. The discussion is focused on factory, but there is some concern on my part that the impact is not limited to the factory or to just the products that engineering has called out.

 

We have an issue ([JIT-144720](https://jira.gtie.dell.com/browse/JIT-144720)) where the Out of Box(OOB) Drivers compiled for RHEL-8 GA are not getting included in initrd on the Z-Stream Kernel(Updated Kernel package released after GA).

Running modinfo of the OOB Driver shows right version and path, but the actual loaded driver will be still inbox driver. This is happening because of an issue in the OS(dracut).

 

We have a workaround where we have to rebuild the initrd manually to include these OOB drivers.

 

This is now affecting the 15G Factory installation as the minimal Kernel supported on 15G for RHEL-8 is the z-stream kernel

 

So, my question here is Does Factory include  OOB Drivers for installation in POST stage? If yes,

 

- Can the workaround(See below) be included in the Factory considering the timelines for the 15G Factory?
  - What would be the impact of including the workaround in the factory? How much efforts would it take interms of script changes and FIV?

 

- If the workaround can't be implemented, then we would be shipping OOB drivers with an issue. Hence, we may have to remove them. Is that possible to remove OOB drivers at this stage?
  - If we have to remove the OOB drivers from Factory environment, What would be the impact ? How much efforts would it take in terms of scripting and also FIV ? 

 

Workaround: In the Post Installation stage, after the Z-Stream kernel is installed and the system is still in chroot environment, rebuild the initrd as

                             dracut \--force \--add-drivers "\<OOB_Kernel_module_name-1\> \<OOB_Kernel_module_name-2\>..." /boot/\<initramfs-kernel_version.img\>

                             Example:- As the z-stream kernel for RHEL-8 is "4.18.0-80.7.1.el8_0.x86_64" run:-

dracut -f \--add-drivers \"megaraid_sas mpt3sas i40e\" /boot/initramfs-4.18.0-80.7.1.el8_0.x86_64.img

 

Thanks,

Kurt Olsson

Project Manager

Dell EMC \| Enterprise Solutions 

office + 1 512 728 8123

<kurt.olsson@dell.com>

 

\"From the equality of rights springs identity of our own highest interests; you cannot subvert your neighbor\'s rights without striking a dangerous blow at your own.\"

 

 

已使用 OneNote 创建。
