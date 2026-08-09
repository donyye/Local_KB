自行查看Error Code

2021年10月15日

11:21

只要是Windows OS都可以执行。不一定要在出问题的机器上执行的。

 

 

例子：

![[Technology_ALL_windows_case_074_自行查看Error Code_001.png]]

 

在任意windwos cmd输入下面命令\
slui.exe 0x2a 0xc0000022

![[Technology_ALL_windows_case_074_自行查看Error Code_002.png]]

 

 

Option 0x2a 是为了看 Error Code 的具体解释和对应方法的。

slui.exe 没有 Help 所以没有官方的解释。

 

 

[https://docs.microsoft.com/en-us/windows-server/get-started/activation-troubleshoot-mak-issues](https://docs.microsoft.com/en-us/windows-server/get-started/activation-troubleshoot-mak-issues)

 

Slmgr.vbs /ato returns an error code

If Slmgr.vbs returns a hexadecimal error code, determine the corresponding error message by running the following script:

cmdCopy

slui.exe 0x2a 0x \<ErrorCode\>

For more information about specific error codes and how to address them, see [Resolving common activation error codes](https://docs.microsoft.com/en-us/windows-server/get-started/activation-error-codes).

 

 

 

 

已使用 OneNote 创建。
