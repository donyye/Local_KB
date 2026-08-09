FW: Support process for RH-HA add on order

2020年12月2日

11:20

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   FW: Support process for RH-HA add on order 
  From      Huang, Antti
  To        Su, Alehandoo; Ye, Dony
  Cc        Lim YH, Patrick; Chuah, Sharene
  Sent      2020年12月2日 11:14
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Internal Use - Confidential

 

FYI.

 

简而言之, 就是卖了OEM, 就一定要绑定support service一起卖. 如果没有, 那就是sale漏了.

即客户买了OEM OS但没有ProSupport, 理论上这种情况不应该存在. 但我们确实有发现. 如果发现, 我们case by case处理. highlight to sale不应该这样卖. 但我们还是要support, 因为不是客户的错.

同理, 对HA也应该是这样. 这是我对下面SPM的原话的理解.

 

Regards,

 

Antti Huang

Senior Principal Engineer \| Solutions Support Team (SST), APJC

Dell Technologies \| APJC ISG Support Services

Certified: RHCA: Architect in HA, Openstack, Ceph, Virtualization, Gluster.

RHCE7 \| SAP HANA Specialist \| Nutanix NPP5 \| CCAH Hadoop \| VCP5.5 \| ITIL \| JNCIA

Working Hours: Monday ‒ Friday \| 8:00 ‒ 16:30 (GMT+8)

 

From: Chuah, Sharene \<Sharene_Chuah@Dell.com\>

Sent: Wednesday, November 11, 2020 13:04

To: Lim YH, Patrick; Huang, Antti

Cc: S, Veera

Subject: FW: Support process for RH-HA add on order

 

Internal Use - Confidential

 

FYI.

 

From: Olsson, Kurt \<Kurt_Olsson@Dell.com\>

Sent: Tuesday, November 10, 2020 11:24 PM

To: Bookless, Gordon; Loeh, Huey Sze; Otero, Liz; Chuah, Sharene; Sigler, Robert; Garcia, Anthony; Swinney, Greg

Cc: Lim, Peggy; Su, Alehandoo

Subject: RE: Support process for RH-HA add on order

 

Internal Use - Confidential

 

Hi team,

This looks like a bunch of issues tied together. Let's simplify and separate them:

 

Sale of service is required on every SW purchase.

1.  Every SW sale of Red Hat / SUSE / Ubuntu requires a ProSupport entitlement. All of them\*. 
    a.  On HW sale with tied OS, some ProSupport or PS+ is required. Also, the matching vendor L3 contract is required.
    b.  If the purchase is made APOS (from standpoint of PowerEdge) then the same requirement applies; unless it is an S&P (A1234567) SKU w/o Dell EMC support.
    c.  No SW service tag is available for Linux add-ons which means certain enforcement mechanisms are missing.

 

In order to ensure that this is done, every sales training on SW sales should reinforce the point that the SW is unsupported unless the service SKU is sold with it. This practice is enforced via Validation Restrictions (VRs) in most cases at POS. If VMware SW is sold, there is a SW service tag generated and the global options in the sales tool combine both the product and its associated service.

Unfortunately, this is not available for the Linux variants as the volume is small relative to other offers. By training sales makers, in the beginning, that SW is unsupported without ProSupport these sorts of issues can be avoided in the future. This also makes the question of 911s saying or not saying "ProSupport required" moot; the rule is to sell SW with support.

 

\*The reason there is no requirement for additional ProSupport on the SKU list Gordon sent out with the DF launch is that the sales maker is responsible for ensuring that the customer is deploying that license onto a PowerEdge with a ProSupport contract with a duration equivalent to, or longer than, the SW license duration.

 

1.  Sharene's question about support of the particular customer
    a.  Support should assist, and escalate to RH if need be. I believe Liz requested "support by exception" in early October.
    b.  In other words, this is a case-by-case escalation for support -- the customer was sold the incomplete solution, but at this time we are obligated to assist them.

 

1.  A note can be attached to an order, but it is seen on the service tag in the CRM. If you don't know which tag(s) should be flagged, that mechanism is cumbersome and prone to error. 

 

1.  A new 911 is only a band-aid, not a solution. As I noted in item 1., the requirement is to sell support, or ensure pre-existing support for the OS based on the entitlement of the service tag to which the product will be installed. This is in keeping with the ProSupport Service Description which identifies PowerEdge plus operating environment (base OS/hypervisor) as a holistic entity when SW support is required on the particular PowerEdge product.

 

These issues are all artifacts of the tools which are provided and the lack of IT hours available to create tighter integrations or better links between HW assets and SW assets.

 

-Kurt

 

已使用 OneNote 创建。
