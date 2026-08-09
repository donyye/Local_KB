Windwos key 重新注册命令

2021年10月25日

16:37

1. 进入 slmgr /upk

 

系统将显示一条提示，表明产品密钥已卸载。

![Machine generated alternative text: Microsoft Windows C 10. o. 14393\] (c) 2016 Microsoft Corporationo : strator\> : /upk : strator\> Windows Script Host ](attachments/Technology_ALL_windows_case_076_Windwos%20key%20重新注册命令_001.png)

 

 

卸载后系统看到如下：

![[Technology_ALL_windows_case_076_Windwos key 重新注册命令_002.png]]

 

 

2. 进入 slmgr /cpky

 

![Machine generated alternative text: i crosoft Windows C 10. o. 14393\] (c) 2016 Microsoft Corporationo : strator\> : /upk : / cpky : strator\> Windows Script Host ](attachments/Technology_ALL_windows_case_076_Windwos%20key%20重新注册命令_003.png)

系统将显示提示，表明已从注册表中清除密钥。

 

slmgr /ipk 加产品密钥KEY .

![[Technology_ALL_windows_case_076_Windwos key 重新注册命令_004.png]]

 

key导入后看到的：

![[Technology_ALL_windows_case_076_Windwos key 重新注册命令_005.png]]

 

 

最后，要激活 windows，请使用 slmgr /ato. 前提是需要连接互联网。

 

如果没有连接互联网，那只能电话激活了。

![[Technology_ALL_windows_case_076_Windwos key 重新注册命令_006.png]]

输入 slui 4 会自动弹出电话激活窗口。

 

 

 

 

 

查看license信息[  slmgr /dlv]

![[Technology_ALL_windows_case_076_Windwos key 重新注册命令_007.png]]

 

 

已使用 OneNote 创建。
