SUSE 补丁更新

2024年5月24日

9:45

SUSE 补丁更新

SUSE 补丁更新可以通过添加订阅更新与单独更新两种。单独更新会涉及到很多问题，包的下载与包的依赖关系需要解决，比较繁琐，而补丁更新本来就是需要付费的，所以这里主要介绍的是通过购买到订阅后的更新。

 

1\. 测试环境： VM SUSE 15 SP6

![[__ SUSE ___005_SUSE 补丁更新_001.png]]

 

2\. 查看当前是否有注册订阅

suse15sp6:\~ \# SUSEConnect -s

\[,,,,,\]

 

3\. 添加完成订阅后会自动添加zypper的外网源

![[__ SUSE ___005_SUSE 补丁更新_002.png]]

 

4\. 测试升级CVE补丁

\# zypper patch \--cve=CVE#cve-2015-2808

![[__ SUSE ___005_SUSE 补丁更新_003.png]]

 

5\. 通过图形化更新补丁

\# zypper install yast2-online-update

\# yast2 online_update

可选的更新补丁

![[__ SUSE ___005_SUSE 补丁更新_004.png]]

点击 Accept 开始打补丁

 

![[__ SUSE ___005_SUSE 补丁更新_005.png]]

 

然后开始安装补丁

![[__ SUSE ___005_SUSE 补丁更新_006.png]]

 

 

 

FYI： <https://documentation.suse.com/sles/15-SP1/html/SLES-all/cha-onlineupdate-you.html>

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

 

 如果输入的订阅与系统不匹配会有如下提示。

![[__ SUSE ___005_SUSE 补丁更新_007.png]]

错误:注册服务器返回\'提供注册码的订阅不包括所请求的产品\'SUSE Linux企业服务器12 SP5 x86_64\' (422)

 

 

 

​

 

已使用 OneNote 创建。
