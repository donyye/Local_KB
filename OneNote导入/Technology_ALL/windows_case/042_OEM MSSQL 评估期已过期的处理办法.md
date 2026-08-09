OEM MSSQL 评估期已过期的处理办法

2018年6月5日

9:35

- ::: 
    -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       OEM MSSQL 2014/16 评估期已过期的处理办法
    发件人     Yin, Guoxun
    收件人     CN XMN TS ENT L2 SME
    抄送       APJ Ent Resolution Managers China
    发送时间   2018年6月5日 9:22
    -------------------------------------- -----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dears,

  Dell OEM MSSQL有增多趋势，问题也越来越多，在此share下安装的时候没有输入license导致180天无法管理的解决办法：

   

  报错如下：很明显可以看到试用期过了

  ![[Technology_ALL_windows_case_042_OEM MSSQL 评估期已过期的处理办法_001.png]]

   

  Re-apply license很简单，完全不需要重装，按照以下步骤操作即可，只要客户没有选择错误，不会对数据有影响，但是切勿在不知情的情况下做出保证。

   

  Please re-enter the product key according to the following procedure.

  1.  Click Start button and run \"Microsoft SQL Server 2014\" -\> \"SQL Server 2014 Installation Center (64-bit)\".
  2.  In \"SQL Server Installation Center\" windows, on left pane, click \"Maintenance\", on right pane, click \"Edition Upgrade\".
  3.  [In \"Upgrade the Edition for SQL Server 2014\" windows and \"Product Key\" page, enter the product key as below and then click \[Next\].]
      a.  [\[ \] Specify a free edition]
      b.  [\[X\] Enter the product key:]
          i.  XXXXX-XXXXX-XXXXX-XXXXX-XXXXX
  4.  [In \"license Terms\" page, check following option then click \[Next\].]
      a.  [\[X\] I accept the license terms.]
  5.  [In \"Select Instance\" page, keep the default settings and click \[Next\].]
  6.  [In \"Ready to upgrade edition\" page, click \[Upgrade\].]
  7.  [In \"Complete\" page, confirm all features\' status are \"Succeeded\" then click \[Close\]. ]
  8.  Close \"SQL Server Installation Center\" window.
  9.  Start Microsoft SQL Server Management Studio to confirm whether it can start properly.

   

   

 

已使用 OneNote 创建。
