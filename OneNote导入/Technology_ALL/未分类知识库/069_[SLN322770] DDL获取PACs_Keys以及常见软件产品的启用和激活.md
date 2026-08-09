[\[SLN322770\] DDL]获取PACs/Keys以及常见软件产品的启用和激活

2020年9月11日

12:56

  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Subject   [\[SLN322770\] DDL]获取PACs/Keys以及常见软件产品的启用和激活
  From      CCC_Enterprise_TS_L2
  To        CCC XMN TS ENTERPRISE
  Cc        Chen, Paul; Zeng, Mars; CN XMN TS ENT L2 Coach
  Sent      2020年9月11日 12:55
  ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

  ----------------------------------------------------------------------------- ---- ------------
  文档 ID                                                           
  版本：                                                                           
  状态：                                                                           
  发布日期：                                                                           
  创建日期：                                                                           
  ----------------------------------------------------------------------------- ---- ------------

 

  --------------------------------------------------------------------------- ---- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  类别:                                                         
  可用于:                                             
  作者:                                                         
  Owner:                                               [jarvis_zhuang](https://kb.dell.com/infocenter/index?page=user_profile&user=5c985924afc8466ebeffb702d05a95c9&rp=content&id=SLN322770)
  --------------------------------------------------------------------------- ---- --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

 

 

 

 

 

 

用户需要使用对应订单的管理员账户登陆到Dell Digital Locker中，才能获取到正确的订单内容。

\*关于DDL账户注册，订单注册等信息，请参考[SLN322064](https://kb.dell.com/infocenter/index?page=content&id=SLN322064&viewlocale=zh_CN) 步骤1-6，订单注册状态和权限确认，请联络CTE工程师。

 

1. 从DDL历史订单中搜索订单号

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_001.jpg]]

 

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_002.jpg]]

（注：如果用户DDL账户内找不到该订单号，请确认用户登陆的DDL账户是否为该订单的管理员）

点击订单号即可进入订单详情。

2. 获取PACs/Keys

第一种获取厂商Keys的方法（一键获取订单中所有PACs/Keys，适用于一个订单中有多个PACs/Keys）：

在订单详情中点击蓝色 "Get all text-based keys"

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_003.jpg]]

浏览器会自动下载CSV文件，请使用Excel打开，最后一列即为该订单所包含的所有PACs/Keys.

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_004.jpg]]

（如果CSV中包含中文字符，可能会出现中文乱码，不影响最后一列获取PACs/Keys）

 

第二种获取厂商Keys的方法：

在订单详情中，点击需要获取PAC/Key的产品

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_005.jpg]]

进入产品详情，从最底部的License Key的地方点击 "Get Key" 获取之后，对应的Key将直接显示在网页上。

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_006.jpg]]

注：产品详情中，黄色高亮区域为注册提示信息，上面有写了相关产品的注册方法，请务必详细阅读。

 

获取后的状态显示如下：

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_007.jpg]]

（注：如遇到License Key显示乱码，可先使用第一种方法获取）

这两种方法均适用多个厂商的软件PAC/Keys，例如VMware/Red Hat/SUSE 等。

 

3. 常见软件的启用、注册和激活

VMware PAC 示例:

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_008.jpg]]

通过上述方法获取到VMware PACs之后，启用/激活/注册地址：

[https://www.vmware.com/oem/code.do?Name=DELL-AC](https://www.vmware.com/oem/code.do?Name=DELL-AC)（仅支持L-DELL订单）

登陆VMware账户后，依照网页注册向导自助完成注册，请在如下页面中阅读相关注意事项并输入DDL获取到的PACs.

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_009.jpg]]

点击Continue后，根据实际情况填写正确真实的Account信息。

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_010.jpg]]

注：注册完成后通常需要一定时间才能够在用户的My VMware账户中显示兑换后的License Key, 如果PACs兑换后超过48小时仍然没有兑换出License Key 请联络VMware License Support.

联络方式：

China (Mainland): <https://www.vmware.com/support/contacts/china.html>

Hong Kong: <https://www.vmware.com/support/contacts/hongkong.html>

Taiwan: [https://www.vmware.com/support/contacts/taiwan.html](https://www.vmware.com/support/contacts/taiwan.html)

Email: [china-license-support@vmware.com](mailto:china-license-support@vmware.com) (如遇到Email无法使用，请使用电话联络)

 

My VMware: <https://my.vmware.com/web/vmware/login>

VMware License降级：https://kb.vmware.com/s/article/2006975

VMware License合并：https://kb.vmware.com/s/article/2006973

 

VMware PreInstalled项目出货的vSphere License Key将直接显示在DDL中，无需兑换，订单显示如下：

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_011.png]]

更多关于VMware PreInstalled License请参考：[QNA44737](https://kb.dell.com/infocenter/index?page=content&id=QNA44737&viewlocale=en_US)

 

Red Hat 示例:

Red Hat 有2种，第一种是License Key，如下图

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_012.jpg]]

 

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_013.jpg]]

 

第二种是Service Tag:

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_014.jpg]]

 

启用/激活/注册地址：

<https://access.redhat.com/subscriptions/activate/protected/activate.html?_flowId=activate-flow&_flowExecutionKey=e1s1>

需要登陆Red Hat账户后进行License注册。

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_015.jpg]]

依照网页注册向导自助完成注册，启用/激活失败或注册过程中关于Red Hat有任何问题，

请联络：[CS-GCG@redhat.com](mailto:CS-GCG@redhat.com)（中文），[customerservice@redhat.com](mailto:customerservice@redhat.com)（英文）

 

SUSE/SLES for SAP 示例:

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_016.jpg]]

 

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_017.jpg]]

启用/激活/注册地址：[https://www.suse.com/reg](https://www.suse.com/reg)

需要登陆SUSE账户后，依照网页向导进行License注册。

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_018.jpg]]

SUSE 电话技术支持：https://www.suse.com/support/handbook/#phone_numbers

 

 

\*所有图片仅供内部使用和参考，不同产品，不同软件版本将呈现不同的内容，请以实际为准。

\*涉及到软件资产和信息安全，以上所有提到的License Key获取和注册部分均由用户（最终用户）完成。

\*如需发送部分内容给客户，请和CTE工程师确认。

\*此KB将不定时进行更新和修正，请勿保存本地副本，最新版本以KB为准。

 

 

 

 

 

Jarvis Zhuang

Centralized Tech Expert \| Enterprise Technical Support

Dell Technologies \| ISG Support Services

office +86-592-818-2315

Mobile +86-156-0609-1047

[Jarvis_Zhuang@Dell.com](mailto:Jarvis_Zhuang@Dell.com)

![[Technology_ALL_未分类知识库_069_[SLN322770] DDL获取PACs_Keys以及常见软件产品的启用和激活_019.jpg]]

How am I doing? Please contact my manager <Mars_Zeng@Dell.com> to provide feedback. Thanks!

 

 

已使用 OneNote 创建。
