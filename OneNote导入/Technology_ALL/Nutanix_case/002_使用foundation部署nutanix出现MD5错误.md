使用foundation部署nutanix出现MD5错误

Wednesday, October 26, 2016

10:07 AM

  -------------------------------------- ------------------------------------------------------------
  主题       RE: ISP SR Activity Escalation
  发件人     Ong, Tan Choong
  收件人     Ye, Dony
  抄送       Lim, Haan Yu
  发送时间   Tuesday, October 25, 2016 3:49 PM
  -------------------------------------- ------------------------------------------------------------

 

解释一下，foundation 其实是一个VM，通过它可以快速的部署nutanix。

 

 

Dell - Internal Use - Confidential 

Hi Dony,

Please filter the below info as some is Internal Educate dell stuff.

 

All XC6230 New Purchase is supposed to come with Deployment Engineer service on site.

I am not sure why this is not. We will highlight this to XC series Program Manager on this case.

Is this a customer unit or a Dell Evaluation Unit?

 

 

From your MD5 error:   

File \"/home/hudsonb/workspace/workspace/foundation_installer-3.3/builds/build-installer-3.3-release/foundation-python-tree/bdist.linux-x86_64/egg/foundation/parameter_validation.py\", line 263, in validate_parameters

StandardError: Hypervisor installer with md5sum 6daa8c9838936304d9ad59b2ab83c92c is not supported

20161024 18:27:09 DEBUG Setting state of \<ImagingStepValidation(\<NodeConfig(10.10.21.120) \@d250\>) \@de10\> from RUNNING to FAILED

 

 

For the Foundation issue on the MD5, you would need to upload the latest whitelist which you can download from Nutanix Portal.

 

![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_001.jpg]]

 

![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_002.jpg]]

 

![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_003.jpg]]

 

I have check the new whitelist contain Dell Image MD5

 

![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_004.jpg]]

 

![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_005.png]]

 

 

We do not have the Deployment Guide for Dell XC using Foundation but you can read more on Educate.dell and create your own document to give to customer.

 

Below is the content in the XC6230 on our Educate.dell portal.

The EKT webex recording is about the manual installation step.

 

![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_006.png]]

 

![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_007.png]]

 

 

OR

 

provide the Nutanix Foundation Deployment Guide from below link.

[https://portal.nutanix.com/#/page/docs/details?targetId=Field_Installation_Guide-v3_2:v3\_\_cluster_image_foundation_t.html#task_lmh_msc_zm](https://portal.nutanix.com/#/page/docs/details?targetId=Field_Installation_Guide-v3_2:v3__cluster_image_foundation_t.html#task_lmh_msc_zm)

 

Hope this clarified.

 

Best Regards,

Tan Choong, ONG 

Software & Solutions Senior Engineer

APJ Solutions Support Team (SST)

Dell EMC \| APJ Remote Services

 

[![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_008.png]]](http://www.dell.com/en-us/work/learn/large-enterprise-solutions)

 

From: Ye, Dony

Sent: Tuesday, October 25, 2016 2:37 PM

To: Ong, Tan Choong \<Tan_Choong_Ong@Dell.com\>

Cc: Lim, Haan Yu \<Haan_Yu_Lim@Dell.com\>

Subject: 答复: ISP SR Activity Escalation

 

Hi, Tan Choong

 

At present, customers have to solve the problem by using foundation.

But have a problem. He use Dell\'s website download the ESXI OS cannot pass foundation deployment, because will be prompted to verify the MD5 problem, but the download from VMware ESXI OS there is no this problem.

 

From infojdf log:

20161024 18:27:03 DEBUG Setting state of \<ImagingStepValidation(\<NodeConfig(10.10.21.120) \@d250\>) \@de10\> from PENDING to RUNNING

20161024 18:27:03 INFO Running \<ImagingStepValidation(\<NodeConfig(10.10.21.120) \@d250\>) \@de10\>

20161024 18:27:03 INFO Validating parameters. This may take few minutes

20161024 18:27:09 INFO Validating parameters. This may take few minutes

20161024 18:27:09 ERROR Exception in running \<ImagingStepValidation(\<NodeConfig(10.10.21.120) \@d250\>) \@de10\>

Traceback (most recent call last):

  File \"/home/hudsonb/workspace/workspace/foundation_installer-3.3/builds/build-installer-3.3-release/foundation-python-tree/bdist.linux-x86_64/egg/foundation/imaging_step.py\", line 124, in \_run

  File \"/home/hudsonb/workspace/workspace/foundation_installer-3.3/builds/build-installer-3.3-release/foundation-python-tree/bdist.linux-x86_64/egg/foundation/imaging_step_validation.py\", line 39, in run

  File \"/home/hudsonb/workspace/workspace/foundation_installer-3.3/builds/build-installer-3.3-release/foundation-python-tree/bdist.linux-x86_64/egg/foundation/config_manager.py\", line 192, in get

  File \"/home/hudsonb/workspace/workspace/foundation_installer-3.3/builds/build-installer-3.3-release/foundation-python-tree/bdist.linux-x86_64/egg/foundation/config_validator.py\", line 526, in common_validations

  File \"/home/hudsonb/workspace/workspace/foundation_installer-3.3/builds/build-installer-3.3-release/foundation-python-tree/bdist.linux-x86_64/egg/foundation/parameter_validation.py\", line 263, in validate_parameters

StandardError: Hypervisor installer with md5sum 6daa8c9838936304d9ad59b2ab83c92c is not supported

20161024 18:27:09 DEBUG Setting state of \<ImagingStepValidation(\<NodeConfig(10.10.21.120) \@d250\>) \@de10\> from RUNNING to FAILED

20161024 18:27:09 DEBUG Setting state of \<GetNosVersion(\<NodeConfig(10.10.21.120) \@d250\>) \@dc50\> from PENDING to NR

20161024 18:27:09 WARNING Skipping \<GetNosVersion(\<NodeConfig(10.10.21.120) \@d250\>) \@dc50\> because dependencies not met

20161024 18:27:09 DEBUG Setting state of \<PrepareHypervisorIso(\<NodeConfig(10.10.21.120) \@d250\>) \@3210\> from PENDING to NR

20161024 18:27:09 WARNING Skipping \<PrepareHypervisorIso(\<NodeConfig(10.10.21.120) \@d250\>) \@3210\> because dependencies not met

20161024 18:27:09 DEBUG Setting state of \<ImagingStepTypeDetection(\<NodeConfig(10.10.21.120) \@d250\>) \@3e10\> from PENDING to NR

20161024 18:27:09 WARNING Skipping \<ImagingStepTypeDetection(\<NodeConfig(10.10.21.120) \@d250\>) \@3e10\> because dependencies not met

20161024 18:27:09 DEBUG Setting state of \<ImagingStepPrepareVendor(\<NodeConfig(10.10.21.120) \@d250\>) \@3710\> from PENDING to NR

20161024 18:27:09 WARNING Skipping \<ImagingStepPrepareVendor(\<NodeConfig(10.10.21.120) \@d250\>) \@3710\> because dependencies not met

20161024 18:27:09 DEBUG Setting state of \<ImagingStepInitIPMI(\<NodeConfig(10.10.21.120) \@d250\>) \@3fd0\> from PENDING to NR

20161024 18:27:09 WARNING Skipping \<ImagingStepInitIPMI(\<NodeConfig(10.10.21.120) \@d250\>) \@3fd0\> because dependencies not met

20161024 18:27:09 DEBUG Setting state of \<ImagingStepPreInstall(\<NodeConfig(10.10.21.120) \@d250\>) \@3590\> from PENDING to NR

20161024 18:27:09 WARNING Skipping \<ImagingStepPreInstall(\<NodeConfig(10.10.21.120) \@d250\>) \@3590\> because dependencies not met

20161024 18:27:09 DEBUG Setting state of \<ImagingStepPhoenix(\<NodeConfig(10.10.21.120) \@d250\>) \@3310\> from PENDING to NR

20161024 18:27:09 WARNING Skipping \<ImagingStepPhoenix(\<NodeConfig(10.10.21.120) \@d250\>) \@3310\> because dependencies not met

20161024 18:27:09 DEBUG Setting state of \<InstallHypervisorESX(\<NodeConfig(10.10.21.120) \@d250\>) \@3d50\> from PENDING to NR

20161024 18:27:09 WARNING Skipping \<InstallHypervisorESX(\<NodeConfig(10.10.21.120) \@d250\>) \@3d50\> because dependencies not met

20161024 18:27:09 DEBUG Setting state of \<ImagingStepLajollaCopyHDD(\<NodeConfig(10.10.21.120) \@d250\>) \@3f10\> from PENDING to NR

20161024 18:27:09 WARNING Skipping \<ImagingStepLajollaCopyHDD(\<NodeConfig(10.10.21.120) \@d250\>) \@3f10\> because dependencies not met

20161024 18:27:12 INFO Validating parameters. This may take few minutes

20161024 18:27:15 INFO Validating parameters. This may take few minutes

 

B R

Dony

 

发件人: Ong, Tan Choong 

发送时间: Tuesday, October 25, 2016 1:46 PM

收件人: Ye, Dony \<[dony_ye@Dell.com](mailto:dony_ye@Dell.com)\>

抄送: Lim, Haan Yu \<[Haan_Yu_Lim@Dell.com](mailto:Haan_Yu_Lim@Dell.com)\>

主题: RE: ISP SR Activity Escalation

 

Dell - Internal Use - Confidential 

Hi Dony,

Below is the webex link:

[https://dell.webex.com/dell/sc30/t.php?MTID=sb3ecd52930414d970c53dbad54f4efc7](https://dell.webex.com/dell/sc30/t.php?MTID=sb3ecd52930414d970c53dbad54f4efc7)

 

 

Best Regards,

Tan Choong, ONG 

Software & Solutions Senior Engineer

APJ Solutions Support Team (SST)

Dell EMC \| APJ Remote Services

 

[![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_009.png]]](http://www.dell.com/en-us/work/learn/large-enterprise-solutions)

 

From: Ye, Dony

Sent: Tuesday, October 25, 2016 1:40 PM

To: Ong, Tan Choong \<<Tan_Choong_Ong@Dell.com>\>; Lim, Haan Yu \<<Haan_Yu_Lim@Dell.com>\>

Subject: 答复: ISP SR Activity Escalation

 

Dell - Internal Use - Confidential 

Hi, Tan Choong

 

Please send the remote connection in advance, I forwarded to the customer.

Thanks！

 

B R

Dony

 

发件人: Ong, Tan Choong 

发送时间: Tuesday, October 25, 2016 1:37 PM

收件人: Ye, Dony \<[dony_ye@Dell.com](mailto:dony_ye@Dell.com)\>; Lim, Haan Yu \<[Haan_Yu_Lim@Dell.com](mailto:Haan_Yu_Lim@Dell.com)\>

主题: RE: ISP SR Activity Escalation

 

Dell - Internal Use - Confidential 

OK sure.

 

Best Regards,

Tan Choong, ONG 

Software & Solutions Senior Engineer

APJ Solutions Support Team (SST)

Dell EMC \| APJ Remote Services

 

[![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_010.png]]](http://www.dell.com/en-us/work/learn/large-enterprise-solutions)

 

From: Ye, Dony

Sent: Tuesday, October 25, 2016 1:02 PM

To: Lim, Haan Yu \<<Haan_Yu_Lim@Dell.com>\>; Ong, Tan Choong \<<Tan_Choong_Ong@Dell.com>\>

Subject: 答复: ISP SR Activity Escalation

 

Dell - Internal Use - Confidential 

Hi, All

 

I have and customer confirmed, on 2:00 PM can be remote.

 

B R

Dony

 

发件人: Lim, Haan Yu 

发送时间: Tuesday, October 25, 2016 11:29 AM

收件人: Ye, Dony \<[dony_ye@Dell.com](mailto:dony_ye@Dell.com)\>; Ong, Tan Choong \<[Tan_Choong_Ong@Dell.com](mailto:Tan_Choong_Ong@Dell.com)\>

主题: RE: ISP SR Activity Escalation

 

Dell - Internal Use - Confidential 

Hi Dony,

 

No worries. I believe the Dell Customized ESXi 5.5 ISO is already in the MICROSDCARD

So we will use Foundation to reimage the nodes to ESXI 5.5 and create the cluster

By default, booting up from the MICROSDCARD out of box installs AHV first

And then using Foundation, we will reimage the nodes back to ESXi 5.5

 

Ensure that the customer has the Nutanix installer downloaded for us

 

This is the current workflow for all XC6320

 

 

 

Haan-Yu, LIM 

Software & Solutions Master Engineer

APJ Solutions Support (SST)

Dell EMC \| Remote Services - APJ

[Haan.Yu.Lim@Dell.com](mailto:Haan.Yu.Lim@Dell.com)

 

[![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_011.png]]](http://www.dell.com/en-us/work/learn/large-enterprise-solutions)

 

Please consider the environment before printing this email.

 

Confidentiality Notice: This email message, including any attachments, is for the sole use of the intended recipient(s) and may contain confidential or proprietary information. Any unauthorized review, use, disclosure or distribution is prohibited. If you are not the intended recipient, immediately contact the sender by reply e-mail and destroy all copies of the original message.

 

 

From: Ye, Dony

Sent: Tuesday, October 25, 2016 11:26 AM

To: Ong, Tan Choong \<<Tan_Choong_Ong@Dell.com>\>

Cc: Lim, Haan Yu \<<Haan_Yu_Lim@Dell.com>\>

Subject: 答复: ISP SR Activity Escalation

 

Dell - Internal Use - Confidential 

Hi，Tan Choong

 

Can we have a webex now with customer?

I can ask customer whether to do.

 

Is this new deployment? Any data in the cluster?

Total 4 node,  not anything data. New machines.

 

If not new deployment, why do they install ESXi?

I see that "ST" read "Nutanix OS for ESXi 5.5U, factory installed" ,  but I don\'t know why no system.

 

 

B R

Dony

 

发件人: Ong, Tan Choong 

发送时间: Tuesday, October 25, 2016 11:12 AM

收件人: Ye, Dony \<[dony_ye@Dell.com](mailto:dony_ye@Dell.com)\>

抄送: Lim, Haan Yu \<[Haan_Yu_Lim@Dell.com](mailto:Haan_Yu_Lim@Dell.com)\>

主题: RE: ISP SR Activity Escalation

 

Dell - Internal Use - Confidential 

Hi Dony,

Can we have a webex now with customer?

Is this new deployment? Any data in the cluster?

If not new deployment, why do they install ESXi?

Appreciate you clarify this.

 

Best Regards,

Tan Choong, ONG 

Software & Solutions Senior Engineer

APJ Solutions Support Team (SST)

Dell EMC \| APJ Remote Services

 

[![[Technology_ALL_Nutanix_case_002_使用foundation部署nutanix出现MD5错误_012.png]]](http://www.dell.com/en-us/work/learn/large-enterprise-solutions)

\-\-\-\--Original Message\-\-\-\--

From: <No_reply@dell.com> \[[mailto:No_reply@dell.com](mailto:No_reply@dell.com)\]

Sent: Tuesday, October 25, 2016 10:45 AM

To: APJ_COE_L3

Subject: ISP SR Activity Escalation

 

This is an automatic email notification. Please do not reply to this email.

 

SR Details:

 

• SR #: 938200133

 

Activity Details:

 

• Activity \# : A-15X6C349

 

• Activity Escalated Group: ENT.ESC.TECHEXPERT.TS.APJ.EN

 

• Activity Priority: 1-ASAP

 

• Activity Owner (if any): NO OWNER

 

• Activity Agent Description: 

 

• Activity Last Updated: 10/25/2016 02:44:34 AM

 

• Activity Created By: DONY_YE 

 

已使用 OneNote 创建。
