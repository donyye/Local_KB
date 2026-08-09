FW: Share: vSphere 6.5下 webclient中上传文件到datastore失败

2017年8月29日

12:24

  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  主题       FW: Share: vSphere 6.5下 webclient中上传文件到datastore失败
  发件人     Wang, Xing Fang
  收件人     CN XMN TS ENT L2 Coach; APJ Ent Resolution Managers China
  抄送       CN XMN TS ENT L2 SME; Yin, Guoxun
  发送时间   2017年8月28日 17:14
  -------------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 

Share technical VMware document for you study.

 

XingFang Wang

Manager Customer Support Services

Great China

Dell Services

Office +86-592-818-5846

Mobile +86-180-3023-3742

Email [Xing_Fang_Wang@Dell.com](mailto:Your%20name@Dell.com)

 

From: Yin, Guoxun

Sent: Monday, August 28, 2017 4:27 PM

To: CN XMN TS ENT L2 SME \<CN_XMN_TS_ENT_L2_SME@Dell.com\>

Subject: Share: vSphere 6.5下 webclient中上传文件到datastore失败

Importance: Low

 

Hi Team,

6.5的web client中上传文件到datastore会出现下面报错，直接在浏览器里面导入证书到trusted store是无效的，经过反复测试，确认下面的解决办法有效。

另外该办法仅针对客户使用自签名证书的情况，客户如果使用的是正式签发的证书，并且已经做了证书的导入，那么不会遇到这样的问题的。

除此以外，经过测试，反复在vCenter中上传datastore会建立ssl连接，如果直接通过host client上传文件反而不会，时间有限，还没完全确认，如果有机会可以让客户试试host client直接上传。

 

 

报错：

![[Technology_ALL_VMware_分析案例_062_FW_ Share_ vSphere 6.5下 webclient中上传文件到da_001.jpg]]

 

解决办法：

 

打开[https://vcenter-IP](https://vcenter-IP)，到如下的界面，选择右下角的"Download trusted root CA certificates", 下载根证书

![[Technology_ALL_VMware_分析案例_062_FW_ Share_ vSphere 6.5下 webclient中上传文件到da_002.jpg]]

 

下载下来后是个压缩包，解包后，依次进入download目录，certs目录，win目录，会看到如下的三个位置文件（名称会有不同，正常）

![[Technology_ALL_VMware_分析案例_062_FW_ Share_ vSphere 6.5下 webclient中上传文件到da_003.png]]

将其中名字后面带0的，Type为"Security Certificate"的证书，改下名字将0去掉，改为cer，改完后的应该如下所示(注意尾数非0的文件用不到，不需要改，也不需要在后面的步骤使用)：

![[Technology_ALL_VMware_分析案例_062_FW_ Share_ vSphere 6.5下 webclient中上传文件到da_004.png]]

在浏览器所在的系统，打开证书管理器，在"Trusted Root Certification Authorities"下面的Certificates标签下面，点右键，选择All task，选额import，分别将上面步骤中的cer文件导入，之后重开浏览器，登录，即可上传文件。

![[Technology_ALL_VMware_分析案例_062_FW_ Share_ vSphere 6.5下 webclient中上传文件到da_005.png]]

 

 

 

已使用 OneNote 创建。
