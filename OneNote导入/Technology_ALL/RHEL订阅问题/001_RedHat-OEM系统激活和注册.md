RedHat-OEM系统激活和注册

2019年8月19日

9:06

- ::: 
    ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    Subject   case 分享：RedHat-OEM系统激活和注册
    From      Cheng, Felson
    To        Xiong, John; Huang, Zhenxiong; He, Julian
    Cc        Wang9, Wei; Zhang, Janice; Gu, Steven; Dong, Peter; Ye, Dony; Yin, Guoxun; Lin, Yongliang
    Sent      2019年3月27日 12:19
    ------------------------------------- ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Hi 专员们

   

  近期处理一个linux  case ，遇到客户新采购oem系统然后进行激活和注册的问题。

   

  Redhat oem订单可能不多，TS遇到激活和注册类的问题可能也会直接升级专员处理。此文档可直接丢给TS并后续跟进即可。

   

  完整的使用oem系统分两步：激活和注册。方法如下：

   

  特别注意的是我们手上的资料和KB可能已经不是最新，而且redhat官网也许有改动。发送资料给客户许久无法进行激活和注册，帮客户webex远程发现界面有所改动而且中英文网站布局也不一样。

  成功注册后，会在redhat官网的"系统"标签下看到注册的redhat 系统（hostname），而且redhat官网的"系统"标签下的"新建"就相当于文档中的"注册新用户"。

   

  激活：

   

  Step 1 : 请准备好服务器服务编码Service Tag 7位序列号，激活地址如下：

  [https://www.redhat.com/wapps/activate/protected/dell.html](https://www.redhat.com/wapps/activate/protected/dell.html)

  点选"注册"按钮，先注册一个RHN ID账号。

  ![[Technology_ALL_RHEL订阅问题_001_RedHat-OEM系统激活和注册_001.jpg]]

  Step 2 成功注册ID之后，通过上面的页面，输入RHN ID,可以进入以下页面，输入Service Tag后按下一步，将OEM激活到客户ID名下

   

  ![[Technology_ALL_RHEL订阅问题_001_RedHat-OEM系统激活和注册_002.jpg]]

  如果用户输入Service Tag，之后错误，且确认客户此服务器有购买OEM RHEL系统。

  1 -- 可以请客户准备一下信息，在联系红帽客户 800 810 2100 #3，并把以下信息发送到cs-gcg@redhat.com

  A - Dell给客户购买OEM系统的发票

  B -  RHN账号

  C -- 客户自己的邮箱

   

  红帽处理时效为2\~3个工作日

   

  离线注册

  在可为某个系统授权前，必须将其添加到 Red Hat Network subscription service 清单中。这个过程我们称之为注册。虽然注册一般在本地操作，并且是设置或者管理机器的一部分。但在您管理整个设施并需要更多全局考虑，或者当您需要管理连接到外部网络的系统时，通过 RHN 订阅管理注册和取消注册更为便利。 

  有些系统可能没有互联网连接，但管理员仍想为那个系统分配并跟踪该订阅。您可以通过手动注册该系统，而不是依赖订阅管理程序执行注册达到此目的。主要有两步：首先在订阅服务中创建一个条目，然后配置该系统。

  1.  打开客户门户网站中的「订阅」标签，并选择「订阅管理」区域中的标签「概述」标签一项。 
  2.  在「使用」标签区域中，点击「注册新用户」链接创建新清单条目。 

  ![[Technology_ALL_RHEL订阅问题_001_RedHat-OEM系统激活和注册_003.png]]

  1.  为新用户类型输入所需信息。系统需要有关架构和硬件方面的信息以便确定那个系统可使用什么订阅。 

  ![[Technology_ALL_RHEL订阅问题_001_RedHat-OEM系统激活和注册_004.png]]

  1.  创建该系统后，请为其分配适当的订阅。 
      1.  打开 已应用订阅 标签。 
      2.  点击 添加订阅 链接。 
      3.  点击所有要分配的订阅的复选框，然后点击 添加所选 按钮。 

  ![[Technology_ALL_RHEL订阅问题_001_RedHat-OEM系统激活和注册_005.png]]

  1.  点击 下载所有证书 按钮。这样会为每个产品将其所有授权证书导出为一个 .zip 文件。将该文件保存到可移动介质中，比如闪存盘。 
  2.  您还可以选择点击 下载身份识别证书 按钮。这样可为注册用户保存身份证书，且用户可使用该证书连接到订阅服务。如果用户将永远离线，那么就没有这个必要。但如果用户有可能使用网络，那么这个操作就很有必要。 
  3.  将介质设备中的授权证书复制给该用户。 
  4.  如果将所有证书都下载成一个归档文件，那么在可下载的 certificates.zip 文件中就有多个归档。解压缩该目录，直到出现该授权证书的 PEM 文件为止。 
  5.  导入授权证书。您可以使用订阅管理 GUI 中的 导入证书 按钮，或者 import 命令完成此操作。例如： 

  <!-- -->

  1.  \# subscription-manager import \--certificate=/tmp/export/entitlement_certificates/596576341785244687.pem \--certificate=/tmp/export/entitlement_certificates/3195996649750311162.pem
  2.  Successfully imported certificate 596576341785244687.pem

  Successfully imported certificate 3195996649750311162.pem

  1.  如果您下载了身份识别证书，请直接将 cert.pem 文件复制到 /etc/pki/consumer 目录中。例如： 

  cp /tmp/downloads/cert.pem /etc/pki/consumer

   

   

  或者使用在线用户注册

  subscription-manager register \--username=xxxxxxxx  \--password=xxxxxx  \--auto-attach

   

   

  Best Regards.

   

  Felson Cheng

  Server Engineer

  Ent Tech Support, Great China Infrastructure & Client Solutions Support

  Dell EMC \| Support and Deployment Services

  [Felson_Cheng@Dell.com](mailto:Felson_Cheng@Dell.com)

  Tech Center [Server](http://en.community.dell.com/techcenter/servers)

  How am I doing? Please contact my manager <Wei_Wang9@Dell.com>

  ![[Technology_ALL_RHEL订阅问题_001_RedHat-OEM系统激活和注册_006.png]]

   

 

已使用 OneNote 创建。
