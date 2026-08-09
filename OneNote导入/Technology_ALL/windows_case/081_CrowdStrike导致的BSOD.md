CrowdStrike导致的BSOD

2024年7月22日

9:01

- 近期如有遇到客户Windows Server 系统使用CrowdStrike 杀毒软件报蓝屏，蓝屏代码为0x50 or 0x7E ，请参考如下KB

   

   

  Stop Code：Page_fault_in_nonpaged_area

  What failed: csagent.sys

   

  ![[Technology_ALL_windows_case_081_CrowdStrike导致的BSOD_001.jpg]]

   

  \
  具体解决方法

  <https://support.microsoft.com/en-us/topic/0d7741f7-aca1-4487-8a54-bd431cb49455#ID0EBBH=Physical_servers>

   

  1.  Boot Windows into Safe Mode or the Windows Recovery Environment
  2.  Navigate to the C:\\Windows\\System32\\drivers\\CrowdStrike directory
  3.  Locate the file matching "C-00000291\*.sys", and delete it.
  4.  Boot the host normally.

   

   

  CrowdStrike 官方说明：

  <https://www.crowdstrike.com/blog/statement-on-falcon-content-update-for-windows-hosts/>

   

  微软说明:

  [Windows message center \| Microsoft Learn](https://learn.microsoft.com/en-us/windows/release-health/windows-message-center#3353)

   

   

  Dell 对外KB:

  [Blue Screen Error Occurs After Updating CrowdStrike \| Dell US](https://www.dell.com/support/kbdoc/en-us/000227088/blue-screen-error-occurs-after-updating-crowdstrike)

   

   

   

   

   

  Resolution：

  [KB5042426: CrowdStrike issue impacting Windows servers causing an 0x50 or 0x7E error message on a blue screen - Microsoft Support](https://support.microsoft.com/en-us/topic/0d7741f7-aca1-4487-8a54-bd431cb49455#ID0EBBH=Hyper-V_hosts)

   

   

  To resolve this issue on Physical servers, follow the steps in the following methods.

  In the following methods, we use the Dell iDRAC remote management console. For example, access the Remote Management Interface for the affected server. This might be different for each vendor depending on the OEM (such as iLO for HP, iDRAC for Dell, CIMC for Cisco). 

  Navigate to the section of the interface that allows you to start the remote console or virtual console.

  Mounting the ISO from Remote Console

  1.  Navigate to Virtual Media in the Remote Console section of the management console.
  2.  Locate the option for mounting an ISO or inserting virtual media. This option might be labeled as Virtual Media, Virtual DVD, or so on.\
      

  ![[Technology_ALL_windows_case_081_CrowdStrike导致的BSOD_002.gif]]

  1.  Select the option to mount or attach an ISO image. You will be prompted to browse for the ISO file on your local system.\
      

  ![[Technology_ALL_windows_case_081_CrowdStrike导致的BSOD_003.gif]]

  1.  Browse and select the ISO file which is of the same version as the affected server version.\
      

  ![[Technology_ALL_windows_case_081_CrowdStrike导致的BSOD_004.gif]]

  1.  Confirm the selection and wait for the management console to upload and mount the ISO to the server.
  2.  Once the ISO is mounted, open the server's operating system or management interface.
  3.  On the Choose an option screen, select Troubleshoot and then select Command Prompt.\
      

  ![[Technology_ALL_windows_case_081_CrowdStrike导致的BSOD_005.gif]]

   

  ![[Technology_ALL_windows_case_081_CrowdStrike导致的BSOD_006.gif]]

  ​​​​​​​

  1.  If your system drive is different than C:\\, type C: and then press Enter. This will switch you to the C:\\ drive.
  2.  Type in the following command and then press Enter:

  CD C:\\Windows\\System32\\drivers\\CrowdStrike

  Note In this example, C is your system drive. This will change the directory to the CrowdStrike directory.

  1.  Once in the CrowdStrike directory, locate the file matching "C-00000291\*.sys". To do this, type the following command and then press Enter:

  dir C-00000291\*.sys

  1.  Permanently delete the file(s). To do this, type the following command and then press Enter.

  del C-00000291\*.sys

  1.  Restart your device.

  Contact CrowdStrike

  If after following the above steps, if you still experience issues logging into your device, please reach out to [CrowdStrike](https://www.crowdstrike.com/) for additional assistance.

   

 

已使用 OneNote 创建。
