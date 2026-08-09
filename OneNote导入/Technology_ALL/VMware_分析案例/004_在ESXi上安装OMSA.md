在ESXi上安装OMSA

Friday, June 13, 2014

3:49 PM

  -------------------------------------- ----------------------------------------------------------------------------------
  主题       【第 14 期】Dell SupportAssist 专刊 
  发件人     Zeng, Richa
  收件人     CN ENT ProSupport; GSD_TAM_APJ_GC
  抄送       Lee, Ernest; Chen, Jeff; Xue, Tina; CN XMN GSD TS MGMT; CN XMN GSD TS Enterprise
  发送时间   Friday, June 13, 2014 3:42 PM
  -------------------------------------- ----------------------------------------------------------------------------------

 

 

Dell - Internal Use -- Confidential

 

![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_001.png]]

案例分享：HK Infitack Technologies Limited SupportAssist真实案例(及时发现EQL电源故障，避免存储上的重要应用异常中断)

(特别注意，本案例分享仅限在 Dell 内部分享，请勿直接用于客户分享)

 

以下是发生在这位Dell客户的一个真实案例，通过这个案例，我们可以切身感受到 SupportAssist 主动式服务的优越性。SupportAssist 是

ProSupport以上服务的一个重要卖点，我们要鼓励购买 ProSupport 以上服务的客户安装和使用SupportAssist, 以便可以享受 SupportAssist

的主动式服务。

![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_002.png]]

 

 

问题：OME/SupportAssist如何管理ESXi主机？ （OME安装在ESXi）

伴随着虚拟化应用的普及，硬件的管理显得更加重要。下面将以ESXi 5.5版本为准，介绍如何把ESXi主机加入到OME/SupportAssist管理系统。

1.    软件环境的准备

a)     访问戴尔技术支持的官方网站下载OMSA for ESXi 5.5安装包。[http://www.dell.com/support/home/cn/zh/cnbsd1?c=cn&l=zh&s=bsd](http://www.dell.com/support/home/cn/zh/cnbsd1?c=cn&l=zh&s=bsd) 

b)    通过vSphere client连接ESXi主机，打开SSH

         

![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_003.jpg]]

c)    使用winscp或其他ssh工具将omsa for esxi 5.5上传到ESXi服务器的/var/log/vmware目录下

d)     以管理员身份，SSH登录ESXi主机，解压安装包

cd /var/log/vmware

unzip OM-SrvAdmin-Dell-Web-7.3.0-588_A00.VIB-ESX55i.zip

![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_004.jpg]]

2.    在ESXi上安装OMSA

a)     关闭ESXi服务器上运行的所有虚机，并使用VMware vSphere Client连接上ESXi服务器，右击服务器名，

选择"Enter Maintenance Mode"，将ESXi服务器进入维护模式

[![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_005.png]]](http://zh.community.dell.com/cfs-file.ashx/__key/communityserver-wikis-components-files/00-00-00-00-46/1856.omsa_5F00_esxi_5F00_07.png)

b)     在安装好VMware vSphere CLI的windows操作机上，打开命令行，并进入VMware vSphere CLI的程序目录下

运行以下命令：esxcli \--server 10.102.17.114 \--username root software vib install -d /var/log/vmware

![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_006.jpg]]

c)     返回VMware vSphere Client控制台，重启ESXi服务器

[![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_007.png]]](http://zh.community.dell.com/cfs-file.ashx/__key/communityserver-wikis-components-files/00-00-00-00-46/7271.omsa_5F00_esxi_5F00_09.png)

d)     待ESXi服务器起来后，检查确认OMSA包已经正确安装。Windows命令行如下

esxcli \--server 10.102.17.114 \--username root software vib list

[![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_008.png]]](http://zh.community.dell.com/cfs-file.ashx/__key/communityserver-wikis-components-files/00-00-00-00-46/4426.omsa_5F00_esxi_5F00_10.png)

e)     运行VMware vSphere Client连接到ESXi服务器。左栏选择ESXi服务器，右栏点击"Configuration"标签，

在"Software"选项里，点击"Advanced Setting"

[![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_009.png]]](http://zh.community.dell.com/cfs-file.ashx/__key/communityserver-wikis-components-files/00-00-00-00-46/omsa_5F00_esxi_5F00_11.png)

f)      在"Advanced Setting"里，选择"UserVars"。请确认右边的"UserVars.CIMvmw_OpenManageProviderEnabled\"的值设为"1"

![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_010.jpg]]

g)     将ESXi服务器退出维护模式。现在，OMSA在ESXi服务器上的安装算完成了。

3.    在ESXi主机上配置SMNP服务

使用putty等ssh工具连接到ESXi主机

  

![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_011.png]]

使用命令设置snmp启用并设置陷阱及陷阱IP地址：

启用SNMP：esxcli system snmp set --e true

设置陷阱及陷阱IP地址：esxcli system snmp set --t [192.168.1.1@162/your_community_name](mailto:192.168.1.1@162/your_community_name)

![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_012.png]]

配置完成后需要在vSphere客户端手动启动服务，方法与SSH服务开启类似，ESXi的SNMP配置完成。

4.    ESXi资源清册

打开"管理/查找和资源清册/添加查找范围"，进入添加查找范围向导：

与Windows环境下的资源清册类似，ICMP默认配置，跳过WS-man以外的其他协议，到WS-Man凭证这边需要填写相关信息：

单选 "启用WS-Man查找"

输入"用户ID"：root

输入"密码"：\<root用户密码\> 

[![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_013.png]]](http://zh.community.dell.com/cfs-file.ashx/__key/communityserver-wikis-components-files/00-00-00-00-46/2465.image030.png)

清册完成后就可以在"管理/设备"里面找到ESXi主机

[![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_014.png]]](http://zh.community.dell.com/cfs-file.ashx/__key/communityserver-wikis-components-files/00-00-00-00-46/7217.image032.png)

通过上面的4个步骤，ESXi主机就加入到OME管理系统

工具下载链接：

Winscp:                  [http://winscp.net/eng/download.php](http://winscp.net/eng/download.php) 

vSphere Client 5.5: [http://vsphereclient.vmware.com/vsphereclient/1/2/8/1/6/5/0/VMware-viclient-all-5.5.0-1281650.exe](http://vsphereclient.vmware.com/vsphereclient/1/2/8/1/6/5/0/VMware-viclient-all-5.5.0-1281650.exe)

vShpere CLI 5.5:     [https://my.vmware.com/cn/web/vmware/details?downloadGroup=VCLI550&productId=353](https://my.vmware.com/cn/web/vmware/details?downloadGroup=VCLI550&productId=353)

 

![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_015.png]]

   

 

Richa Zeng

CCC Enterprise ProSupport

Dell \| Global Support and Deployment

office +86-592-818-5754

 

![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_016.png]]

 

[![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_017.png]]](http://zh.community.dell.com/support_forums/enterprise-solutions/f/260/t/8990.aspx)

 

[![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_018.png]]](http://support.dell.com.cn/)

 

[![[Technology_ALL_VMware_分析案例_004_在ESXi上安装OMSA_019.png]]](http://bbs.dell.com.cn/)

 

[Dell TechDirect](http://www.techdirect.com/) \| 戴尔在线报修门户网站: 提供在线报修，自主部件派单以及在线管理报修事件 

回复邮件获取详细资料或点击 Dell TechDirect 超链接了解更多信息！！

 

[Dell SupportAssist](http://zh.community.dell.com/techcenter/systems-management/w/wiki/616.dell-supportassist.aspx) \|一款软件插件，可实现远程监控硬件环境、自动收集数据和日志、主动创建案例和派遣零部件

回复邮件获取详细资料或点击 Dell SupportAssist 超链接了解更多信息！！

 

 

 

 

已使用 OneNote 创建。
