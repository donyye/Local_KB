ST:B6G1ST2\| SR:986622022 \|redhat:02319499 [ Case #02371943 (\[HA Architecture Review\]  RHEL 6.10 HA Cluster Configuration) ref:\_00DA0HxWH.\_5002KdXzok:ref]

2019年5月31日

9:53

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   [FW: (WoC) (SEV 3) Case #02371943 (\[HA Architecture Review\]  RHEL 6.10 HA Cluster Configuration) ref:\_00DA0HxWH.\_5002KdXzok:ref]
  From      Huang, Antti
  To        Niu, Jane
  Cc        Ye, Dony
  Sent      2019年5月23日 14:53
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Hi Dony, Jane,

 

The following is the update from Red Hat for HA review. Thanks.

 

 

Regards,

 

Antti Huang

Senior Principal Engineer, Infrastructure & Client Solutions

Solutions Support Team (SST)

Dell EMC \| Support & Deployment Services

Certified: RHCA: Architect in HA, Openstack, Ceph, Virtualization, Gluster.

RHCE7 \| SAP HANA Specialist \| Nutanix NPP5 \| CCAH Hadoop \| MCSE 2008 \| VCP5.5 \| ITIL \| JNCIA

Working Hours: Monday ‒ Friday \| 9:00 ‒ 17:30 (GMT+8)

 

 

\-\-\-\--Original Message\-\-\-\--

From: Red Hat Support \<support@redhat.com\>

Sent: Tuesday, May 21, 2019 21:24

To: Huang, Antti

Subject: (WoC) (SEV 3) Case #02371943 (\[HA Architecture Review\] RHEL 6.10 HA Cluster Configuration) ref:\_00DA0HxWH.\_5002KdXzok:ref

 

 

\[EXTERNAL EMAIL\]

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

\|         Case Information            \|

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

<https://access.redhat.com/support/cases/#/case/02371943>

Case Title       : \[HA Architecture Review\]  RHEL 6.10 HA Cluster Configuration

Case Number      : 02371943

Case Open Date   : 2019-04-30 18:40:28

Severity         : 3 (Normal)

Problem Type     : Configuration issue

 

Most recent comment: On 2019-05-21 21:14:36, Zimek, Pepa commented:

\"Hello,

 

My name is Josef Zimek and I am a member of Red Hat\'s High Availability support team. I was asked to assist with this case. My understanding is that you were suggested to raise cluster architecture review to check your cluster environment. I would like to point out that architecture review process is nowadays obsolete and Red Hat support doesn\'t provide any complex review of cluster environments but rather provides assistance with cases on per problem basis (for each problematic area there is support case created). More details about this approach can be found here:

 

 

How can Red Hat assist me in assessing the design of my RHEL High Availability or Resilient Storage cluster?

<https://access.redhat.com/articles/2359891>

 

 

I however checked sosreports of your for possible areas for improvement and here are my comments regarding your cluster configuration - there are multiple concerns with this cluster that should be addressed:

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

1\) post_fail_delay=\"900\" in /etc/cluster/cluster.conf

 

   Increasing post_fail_delay while fence_kdump is configured is counter-productive. (Increasing post_fail_delay was required earlier in to allow capture vmcore before fencing kicks in but fence_kdump takes care of the delay automatically = no need to set post_fail_delay=\"900\". It is recommended to keep it at default value \"0\")

 

 

2\) This is 3 node cluster with qdisk. Qdisk has default amount of votes (1) - this makes 4 possible votes in cluster in total however expected_votes is set to \"5\" for some reason. With current configuration the expected_votes should be \"4\". Any specific reason why is it set to 5?

 

 

3\) Out of 2 SAPInstance resources and one orainstance only one SAPInstance is actually used in service:

 

        \<service domain=\"SAP\" name=\"sap\" recovery=\"disable\"\>

            \<ip ref=\"10.1.192.9\"/\>

            \<SAPInstance ref=\"PRD_ASCS00_prd1vip\"/\>

        \</service\>

 

(not sure why there are unused resources configured)

 

 

4\) fence_kdump is configured but is set as secondary method so power fencing has priority. fence_kdump must be configured as primary fencing method in order to be able capture vmcore:

 

            \<fence\>

                \<method name=\"Method\"\>

                    \<device name=\"prd1-fence\"/\>

                    \<device name=\"kdump\" nodename=\"prd1\"/\>

                \</method\>

            \</fence\>

 

 

How do I configure kdump for use with the RHEL 6, 7, 8 High Availability Add-On?

<https://access.redhat.com/articles/67570>

 

 

5\) Any heuristic that is using the ping command must enabled the -w (deadline timeout) with a value equal to or larger than one. The following heuristic program values were invalid:

    - <https://access.redhat.com/site/solutions/64633>

 

    program                  interval  min_score  tko

    \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- \-\-\-\-\-\-\--  \-\-\-\-\-\-\-\--  \-\-\-\-\--

    /bin/ping -c1 10.1.192.1 -1        1          60,000

 

 

6\) The service \"acpid\" is not disabled on all runlevels(0 - 6). This service should be disabled since a system management fence device(fence_ipmilan) was detected. If acpid is enabled the fencing operation may not work as intended.

    - <https://access.redhat.com/site/solutions/5414>

 

 

7\) There were GFS/GFS2 file-systems that did not have the mount option \"noatime\"(no \"nodiratime\" is implied. when noatime is set) enabled. Unless atime support is essential, Red Hat recommends setting the mount option \"noatime\" on every GFS/GFS2 mount point. This will significantly improve performance since it prevents reads from turning into writes because the access time attribute will not be updated.

    - <https://access.redhat.com/knowledge/solutions/35662>

 

 

8\) Node prd3 seems to export gfs2 filesystem over NFS - there is risk of data corruption. Any GFS/GFS2 filesystem that is exported with NFS should have the option \"localflocks\" set.The following GFS/GFS2 filesystem do not have the option set.

 

    - <https://access.redhat.com/knowledge/solutions/20327>

    - <http://docs.redhat.com/docs/en-US/Red_Hat_Enterprise_Linux/5/html-single/Configuration_Example_-_NFS_Over_GFS/index.html#locking_considerations>

 

    device_name        mount_point

    \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- \-\-\-\-\-\-\-\-\-\-\-\--

    /dev/mapper/mpathf /sapinterface

    /dev/mapper/mpathd /saptrans

    /dev/mapper/mpathe /usr/sap

    /dev/mapper/mpathc /sapmnt

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

 

Thank you.

 

Josef Zimek, RHCA

Software Maintenance Engineer

Global Support Services

Red Hat, Inc.\"

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

Thank you for your latest interaction with Red Hat Global Support Services. We are currently working to resolve your case.

 

Your case has transitioned to \"Waiting On Customer\" status. This means that the Red Hat associate working on your case needs information or action from you to proceed. To help us resolve your case as quickly as possible, please update your case online on the Customer Portal at <https://access.redhat.com>.

 

Once you update the case to provide the requested information, we can continue working to resolve your issue.

 

If you wish to contact Red Hat, visit the Customer Portal at <https://access.redhat.com> to find phone and web contact information relevant to your region and support contract.

 

 

Thank you,

 

Red Hat Global Support Services

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

To ensure the best support experience possible, please note the following:

 

\* Replying to this email should result in your comments being added to the case. However, we suggest adding comments to the case directly via the Customer Portal in case the email fails.

\* When replying to this email, do not change the subject.

\* Check to make sure you are replying to case emails from the email address that is listed as the case contact.

\* Attachments cannot be added to a case via email. Attachments must be uploaded to a case directly.

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

Supporting success. Exceeding expectations.

 

Red Hat Support on Social Media: <https://access.redhat.com/social/> Red Hat Customer Portal Discussions: <https://access.redhat.com/discussions/>

Red Hat Access Labs: <https://access.redhat.com/labs/>

 

If you need immediate assistance, please refer to <https://access.redhat.com/support/contact/technicalSupport/>

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

ref:\_00DA0HxWH.\_5002KdXzok:ref

 

已使用 OneNote 创建。
