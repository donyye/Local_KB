使用Winobj获取盘符和物理设备对应关系

Friday, June 17, 2016

10:54 AM

- 从以下微软的网站下载Winobj这个工具

  <https://technet.microsoft.com/en-us/sysinternals/bb896657.aspx?f=255&MSPPError=-2147217396>

   

   

  解压后得到如下图中的文件，请双击运行齿轮状的Winobj这个程序

  ![[Technology_ALL_windows_case_027_使用Winobj获取盘符和物理设备对应关系_001.jpg]]

   

   

  打开后，请调整窗口大小至合适状态，调整左右分栏，如下图，然后按照以下顺序处理

  1.  鼠标点中左边导航栏中的"GLOBAL??"[  ]如标记A
  2.  在右边窗口中点"Symlink"标签，使该列表排序显示
  3.  调整下面的Name标签，将其下面的内容除个别极长的项目外都显示出来，
  4.  拉动滚动条，使"Symlink"下面的列表中标记为"\\Device\\Harddisk"开头的项目全部显示出来，然后截图给我们，如果"\\Device\\Harddisk"开头的项目太多，一屏无法截图完成屏幕，请分成多个图片给我们。

  ![[Technology_ALL_windows_case_027_使用Winobj获取盘符和物理设备对应关系_002.jpg]]

   

   

   

  补充下，

  好像我理解和回复Ruyang的问题有偏差，修正如下

  下面这一串都是第一个物理硬盘相关的内容，

  他们的逻辑串联关系是，

  \\Device\\Harddisk0\\DR0\-\--àPhysicalDriver0-àHarddisk0

  所以\\Device\\HarddiskVolume1就是第一个硬盘上的第一个卷了，也就是C盘。

  ![[Technology_ALL_windows_case_027_使用Winobj获取盘符和物理设备对应关系_003.jpg]]

   

   

 

已使用 OneNote 创建。
