IPS 对MCE Error分析

2015年2月9日

15:09

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       MCE Error
  发件人     Xu, Xiaofeng
  收件人     Ye, Dony
  发送时间   2014年1月22日 15:52
  附件       \<\<253668.pdf\>\>
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Dell - Internal Use - Confidential 

Hi Dony,

                MCE Memory controller RD_CHANNELunspecified_ERR cannot 100% mean it's memory module error.

MCA: MEMORY CONTROLLER RD_CHANNELunspecified_ERR

Transaction: Memory read error

STATUS b00000000800009f MCGSTATUS 0

MCGCAP 1000c18 APICID 80 SOCKETID 2

 

Based on the MCi_Satus register b00000000800009f.

Both S flag (Bit 56) and AR flag show 0.

 

As you can see from Intel document Page#667

S flag 0 mean it was reported as a corrected machine check and no action need to be taken.

 

For the reason why all application will be move to another node in cluster environment.

Customer may need to check with Redhat.

 

You also can see similar result that MCE doesn't actually mean hardware error from below website.

[http://www-947.ibm.com/support/entry/portal/docdisplay?lndocid=MIGR-5084973](http://www-947.ibm.com/support/entry/portal/docdisplay?lndocid=MIGR-5084973)

 

If it's real HW problem.

Normally, The MCE will be logged in SEL.

 

                If issue continues happened on the system.

                I would suggest you to disable C-state/MWAIT and set power option to maximum performance in BIOS SETUP.

                The monitor it for a while.

 

                If customer insist to replace the HW.

                I would suggest to replace CPU#1 firstly.

 

Quote from Intel document:

In addition, the IA32_MCi_STATUS register bit fields, bits 56:55, are defined (see

Figure 15-5) to provide additional information to help system software to properly

identify the necessary recovery action for the UCR error:

 

• S (Signaling) flag, bit 56 - Indicates (when set) that a machine check exception

was generated for the UCR error reported in this MC bank and system software

needs to check the AR flag and the MCA error code fields in the

IA32_MCi_STATUS register to identify the necessary recovery action for this

error. When the S flag in the IA32_MCi_STATUS register is clear, this UCR error

was not signaled via a machine check exception and instead was reported as a

corrected machine check (CMC). System software is not required to take any

recovery action when the S flag in the IA32_MCi_STATUS register is clear.

 

• AR (Action Required) flag, bit 55 - Indicates (when set) that MCA error code

specific recovery action must be performed by system software at the time this

error was signaled. This recovery action must be completed successfully before

any additional work is scheduled for this processor When the RIPV flag in the

IA32_MCG_STATUS is clear, an alternative execution stream needs to be

provided; when the MCA error code specific recovery specific recovery action

cannot be successfully completed, system software must shut down the system.

When the AR flag in the IA32_MCi_STATUS register is clear, system software may

still take MCA error code specific recovery action but this is optional; system

software can safely resume program execution at the instruction pointer saved

on the stack from the machine check exception when the RIPV flag in the

IA32_MCG_STATUS register is set.

 

 

Xiaofeng Xu 徐晓锋

IPS Engineer - ESG

Dell \| PG IPS 

office +86 21 2203 0832,  fax +86 21 2203 1832, mobile +86 13301753507

Address

Dell (China) Co. Ltd. Shanghai Branch.

Room 501, Multi-media Park, No.999 Changning Road,

Shanghai, P.R.China. 200050

地址: 中国上海市长宁区长宁路999号多媒体生活广场501室戴尔中国上海分公司 邮编：200050

 

 

已使用 OneNote 创建。
