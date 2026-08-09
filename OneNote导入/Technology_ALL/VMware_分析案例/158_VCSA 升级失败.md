VCSA 升级失败

2023年10月23日

17:31

 

1. 停止 VCSA 服务

2. 确保这里 version 看到的版本是一样的。这里的版本是 7.0.3.01100 ，升级前的版本。之后再升级到新版本。

![[Technology_ALL_VMware_分析案例_158_VCSA 升级失败_001.png]]

 

![[Technology_ALL_VMware_分析案例_158_VCSA 升级失败_002.png]]

 

 

3. 而这个例子看到的是不一样的。

cat etc/applmgmt/appliance/software_update_state.conf 

{

    \"state\": \"INSTALL_FAILED\",

  [  \"version\": \"7.0.3.01500\",]

    \"latest_query_time\": \"2023-10-13T02:39:09Z\",

    \"operation_id\": \"/storage/core/software-update/install_operation\"

 

 

cat etc/applmgmt/appliance/update.conf

.......

    \"latestUpdateInstallTime\": \"2023-02-13T04:09:10.333Z\",

    \"latestUpdateQueryTime\": \"2023-10-13T02:39:09.623Z\",

    \"name\": \"VC-7.0U3d\",

    \"password\": \"\",

    \"product\": \"VMware vCenter Server\",

    \"releasedate\": \"March 29, 2022\",

    \"summary\": \"Patch for VMware vCenter Server 7.0\",

    \"time\": \"00:00:00\",

    \"type\": \"DEFAULT-REPO\",

    \"username\": \"\",

[    \"version\": \"7.0.3.01100\"]

 

 

![[Technology_ALL_VMware_分析案例_158_VCSA 升级失败_003.png]]

 

![[Technology_ALL_VMware_分析案例_158_VCSA 升级失败_004.png]]

 

![[Technology_ALL_VMware_分析案例_158_VCSA 升级失败_005.png]]

 

![[Technology_ALL_VMware_分析案例_158_VCSA 升级失败_006.png]]

 

![[Technology_ALL_VMware_分析案例_158_VCSA 升级失败_007.png]]

 

![[Technology_ALL_VMware_分析案例_158_VCSA 升级失败_008.png]]

 

![[Technology_ALL_VMware_分析案例_158_VCSA 升级失败_009.png]]

 

 

![[Technology_ALL_VMware_分析案例_158_VCSA 升级失败_010.png]]

 

删除完后修改完成后启动VC然后再重新升级

 

已使用 OneNote 创建。
