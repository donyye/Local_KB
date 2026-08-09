RE: Red Hat Product Update - RHEL 5 and 6 - Lifecycle Dates You Need to Know

2020年8月25日

8:37

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   RE: Red Hat Product Update - RHEL 5 and 6 - Lifecycle Dates You Need to Know
  From      Huang, Antti
  To        Edward Jin
  Cc        Lim YH, Patrick; Wang, Xing Fang; Ye, Dony; Steven Shaffer; Qu, Yunyun; S, Veera
  Sent      2020年8月24日 14:01
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell Customer Communication - Confidential

 

Hi Edward,

 

Thanks for the confirmation.

 

Regards,

 

Antti Huang

Senior Principal Engineer \| Solutions Support Team (SST), APJC

Dell Technologies \| APJC ISG Support Services

Certified: RHCA: Architect in HA, Openstack, Ceph, Virtualization, Gluster.

RHCE7 \| SAP HANA Specialist \| Nutanix NPP5 \| CCAH Hadoop \| VCP5.5 \| ITIL \| JNCIA

Working Hours: Monday ‒ Friday \| 8:00 ‒ 16:30 (GMT+8)

 

From: Edward Jin \<xujin@redhat.com\>

Sent: Monday, August 24, 2020 13:53

To: Huang, Antti

Cc: Lim YH, Patrick; Wang, Xing Fang; Ye, Dony; Steven Shaffer; Qu, Yunyun; S, Veera

Subject: Re: Red Hat Product Update - RHEL 5 and 6 - Lifecycle Dates You Need to Know

 

\[EXTERNAL EMAIL\]

Hi Antti, 

 

As we confirmed, Dell\'s support contract offers RHEL ELS (Extended Life-cycle Support) . To be specific, Dell can escalate the RHEL 6 issue to Red Hat after RHEL 6 ends its life cycle on November 30th.

Please make sure that the end customers have the valid RHEL ELS Add-On Subscription. 

 

Besides, the L3-Only support procedure for ELS has no change. Dell handles the L1-L2 issues, and escalates L3 issue to Red Hat. 

 

 

What is Extended Life-cycle Support Add-On?

 

Extended Life-cycle Support (ELS) is an optional Add-On subscription for certain Red Hat Enterprise Linux subscriptions. The ELS Add-on is available during the Extended Life Phase for Red Hat Enterprise Linux 5 and 6.

- Red Hat Enterprise Linux 5 ELS delivers certain critical-impact security fixes and selected (at Red Hat discretion) urgent priority bug fixes and troubleshooting for the [last minor release](https://access.redhat.com/support/policy/updates/errata#Life_Cycle_Dates) (RHEL 5.11).
  - [RHEL 5 ELS Inclusion List](https://access.redhat.com/articles/2901071).
- Red Hat Enterprise Linux 6 ELS delivers certain qualified Critical and Important security fixes and selected (at Red Hat discretion) urgent priority bug fixes and troubleshooting for the [last minor release](https://access.redhat.com/support/policy/updates/errata#Life_Cycle_Dates) (RHEL 6.10).
  - [RHEL 6 ELS Inclusion List](https://access.redhat.com/articles/4997301).

For Red Hat Enterprise Linux 5, the ELS Add-On covers the IBM z Systems and the x86 architecture, both 32-bit and 64-bit variants with the exception of the Itanium architecture. For Red Hat Enterprise Linux 6, the ELS Add-On covers the IBM z Systems and the x86 architecture, both 32-bit and 64-bit variants. Add-Ons are not covered by ELS.

 

Feel free to let us know, if there are any questions. 

Thank you. 

 

 

Reference: 

\[1\] Red Hat Enterprise Linux Life Cycle - <https://access.redhat.com/support/policy/updates/errata>

\[2\] How am I supported on a specific RHEL release? - <https://access.redhat.com/articles/64664>

 

 

Best Regards

Edward

 

 

 

On Fri, Aug 14, 2020 at 9:36 AM \<<Antti.Huang@dell.com>\> wrote:

Dell Customer Communication - Confidential

 

Hi Edward,

 

Would like to double check with you whether our OEM support contract entitles for ELS (Extended life cycle support)? Thanks.

 

Regards,

 

Antti Huang

Senior Principal Engineer \| Solutions Support Team (SST), APJC

Dell Technologies \| APJC ISG Support Services

Certified: RHCA: Architect in HA, Openstack, Ceph, Virtualization, Gluster.

RHCE7 \| SAP HANA Specialist \| Nutanix NPP5 \| CCAH Hadoop \| VCP5.5 \| ITIL \| JNCIA

Working Hours: Monday ‒ Friday \| 8:00 ‒ 16:30 (GMT+8)

 

From: Xuegang Jin \<<xujin@redhat.com>\>

Sent: Thursday, December 12, 2019 12:09

To: S, Veera

Cc: Huang, Antti; Lim YH, Patrick; Wang, Xing Fang; Ye, Dony; Steven Shaffer; Qu, Yunyun

Subject: Red Hat Product Update - RHEL 5 and 6 - Lifecycle Dates You Need to Know

 

\[EXTERNAL EMAIL\]

Dear Veera, 

 

We would like to inform you that we are coming up on less than 12 months to two major RHEL Lifecycle Milestones:

 

 - RHEL 5.11 with Extended Life Support will end Nov 2020 (reminder - RHEL 5.11 with ELS is the only actively supported version of 5.x)

 

 - RHEL 6.10 will end Maintenance Support Nov 2020 - and will at that time, require ELS for further support.

 

 

 

Customers still running RHEL 5.11 with ELS need to look at how they can migrate off that platform - and additionally the hardware that runs this older version of RHEL is possibly (probably) not supported either. RHEL 5 was announced in March 2007 - and hardware vendors will have limited support for older hardware. So the customer may have additional support challenges in terms of hardware support, OS Support, and App support. What we call a "triple whammy".

 

For customers still running RHEL 6.10, (RHEL 6 was announced in Nov 2010) once we get to 1 Dec 2020 - an ELS Subscription (6.10 only) will be required to get support from RH. This ELS for 6.10 will be available from 1 Dec 2020, to June 30, 2024

 

Don't forget - Red Hat Insights is included with RHEL 5, 6, 7, and 8

 

For more information on the [RHEL Lifecycle Dates and options](https://access.redhat.com/support/policy/updates/errata/#Life_Cycle_Dates), visit the RHEL Lifecycle Information on [Red Hat Customer Access Portal](https://access.redhat.com/), 

Thank you. 

 

 

Best Regards

Edward

 

\--

Edward Jin

TECHNICAL ACCOUNT MANAGER, CEE APAC

[Red Hat Software (Beijing) Co.,Ltd](https://www.redhat.com/)

8 Floor, Tower A, Parkview Office, Beijing, China

[xujin@redhat.com](mailto:xujin@redhat.com)    T: [+86-10-6562-7436](tel:+86-10-6562-7436)    

[\@RedHat](https://twitter.com/redhat)   [Red Hat](https://www.linkedin.com/company/red-hat)  [Red Hat](https://www.facebook.com/RedHatInc)

[![[Technology_ALL_Linux 问题收集_065_RE_ Red Hat Product Update - RHEL 5 and 6_001.png]]](https://www.redhat.com/)

 

 

 

\--

Edward Jin

TECHNICAL ACCOUNT MANAGER, CEE APAC

[Red Hat Software (Beijing) Co.,Ltd](https://www.redhat.com/)

8 Floor, Tower A, Parkview Office, Beijing, China

[xujin@redhat.com](mailto:xujin@redhat.com)    T: [+86-10-6562-7436](tel:+86-10-6562-7436)    

[\@RedHat](https://twitter.com/redhat)   [Red Hat](https://www.linkedin.com/company/red-hat)  [Red Hat](https://www.facebook.com/RedHatInc)

[![[Technology_ALL_Linux 问题收集_065_RE_ Red Hat Product Update - RHEL 5 and 6_001.png]]](https://www.redhat.com/)

 

![[Technology_ALL_Linux 问题收集_065_RE_ Red Hat Product Update - RHEL 5 and 6_002.png]]

 

已使用 OneNote 创建。
