设置windwos full dump

2019年9月20日

8:54

·如下创建新的注册表项。

              Key:       HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\kbdhid\\Parameters

              Name:   CrashOnCtrlScroll

              Type:     DWORD (32-bit) Value     ////// REG_DWORD

              Data:     0x00000001 (1)

 

·更改以下注册表项的数据，以根据需要定义内存类型。

              Key:       HKEY_LOCAL_MACHINE\\System\\CurrentControlSet\\Control\\CrashControl

              Name:   CrashDumpEnabled

              Type:     DWORD (32-bit) Value ////// REG_DWORD

              Data:[     1     //////]完全内存转储

 

·要收集完整的内存转储，请将页面文件大小更改为\[物理内存大小[\] + 257 MB]。

               - 当物理内存大小为32 GB时，如何计算页面文件大小

              Size：32 x 1024 + 257 = 33025 MB 

 

               - 系统属性中的虚拟内存设置

[              \[\]]自动管理所有驱动器的页面文件大小[  ]//////将其设置为Disabled。

              选定的驱动器：C：

[              \[X\]]自定义尺寸：

                            初始大小（MB）：33025

                            最大尺寸（MB）：33025

                            注意：

                            （1）确认C：驱动器中有足够的可用空间

                            （2）单击\[设置[\]]需要应用更改的设置。

虚拟内存设置参考图：

![[Technology_ALL_windows_case_013_设置windwos full dump_001.png]]

 

·重新启动操作系统以应用上述设置。

·发生挂起问题时，按住右【Ctrl】键并按两次[\[Scroll Lock\]]键触发蓝屏以产生内存转储。

              注意：建议在设置后进行测试。

              备注：以下信息将显示在蓝屏上。

                            \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- \-\-\--

                            您的PC遇到问题，需要重新启动。我们只是收集一些错误信息，并且

                            然后我们会为你重启。

                            

                            ??％完成

 

                            有关此问题和可能的修复的详细信息，请访问http://windows.com/stopcode

                            如果您致电支持人员，请告诉他们以下信息：

                            停止代码：手动启动崩溃

                            \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-- \-\-\--

 

·压缩以下内存转储文件，然后发送给Dell进行分析。

              C：\\ WINDOWS \\ MEMORY.DMP

 

 

================================

 

・Create a new registry entry as follows.

              Key:       HKEY_LOCAL_MACHINE\\SYSTEM\\CurrentControlSet\\Services\\kbdhid\\Parameters

              Name:   CrashOnCtrlScroll

              Type:     DWORD (32-bit) Value     ////// REG_DWORD

              Data:     0x00000001 (1)

・Change the data of following registry entry to define the memory type as your needs.

              Key:       HKEY_LOCAL_MACHINE\\System\\CurrentControlSet\\Control\\CrashControl

              Name:   CrashDumpEnabled

              Type:     DWORD (32-bit) Value     ////// REG_DWORD

              Data:     1                                        ////// Complete memory dump

・[To collect complete memory dump, change the paging file size to \[Physical Memory Size\] + 257 MB.]

              - How to calculate paging file size when the physical memory size is 32 GB

              Size: 32 x 1024 + 257 = 33025 MB              ////// Refer KB2860880.

 

              - Virtual Memory settings in System Properties

              \[ \] Automatically manage paging file size for all drives            ////// Set it to Disabled.

              Selected drive:    C:

              \[X\] Custom size:

                            Initial size (MB):                33025

                            Maximum size (MB):         33025

                            Note:

                            (1) Confirm there is sufficient free space in C: drive

                            (2) Clicking \[Set\] is required to apply the changed settings.

・Reboot OS to apply the above settings.

・[When hang-up issue occurs, keep pressing right \[Ctrl\] key and strike \[Scroll Lock\] key twice to trigger blue screen to generate memory dump. ]

              Note: Recommend to do a test after the settings.

              Remarks: Following information will be displayed on blue screen.

                            \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

                            Your PC ran into a problem and needs to restart. We\'re just collecting some error info, and

                            then we\'ll restart for you.

                           

                            ??% complete

 

                            For more information about this issue and possible fixes, visit <http://windows.com/stopcode>

                            If you call a support person, give them this info:

                            Stop Code: MANUALLY INITIATED CRASH

                            \-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

・Zip the following memory dump file and then send to Dell for analysis.

              C:\\Windows\\MEMORY.DMP

 

 

 

已使用 OneNote 创建。
