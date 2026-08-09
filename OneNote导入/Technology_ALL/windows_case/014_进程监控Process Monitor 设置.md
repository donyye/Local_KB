进程监控Process Monitor 设置

2019年9月20日

9:48

 

Process Monitor是监视进程的。录制操作步骤的是叫Step Record Tool

 

如果问题仍然存在，请收集以下日志以供进一步调查。

              （a）准备Process Monitor工具。

              ·从以下URL下载Process Monitor工具。

                            <https://docs.microsoft.com/en-us/sysinternals/downloads/procmon>

              ·解压ProcessMonitor.zip。

              ·在C：驱动器下创建一个新文件夹"Tools"。

              ·将Procmon.exe复制到C：\\ Tools。

              ·双击Procmon.exe。

              ·在"Process Monitor许可协议"窗口中，单击\[同意[\]]。

                            注意：此窗口仅出现一次。

              ·单击"捕获"和"清除"按钮以停止并清理跟踪。

                            注意：目的是保持日志大小较小，因为日志大小增加得非常快。

 

              （b）准备步骤记录器工具。

              ·右键单击"开始"按钮，单击"运行"，输入"psr"，然后单击\[确定[\]]

              ·在"步骤记录器"窗口中，单击最右侧的箭头，然后单击\[设置[\]]。

              ·将"最近屏幕捕获的数量"从25更改为100，然后单击\[确定[\]]

 

              （c）开始追踪并重现问题。

              ·在"Step Recorder"窗口中，单击[\[Start Record\]]。

              ·在"Process Monitor"窗口中，单击"Capture"按钮开始跟踪。

              ·重现问题。

 

              （d）停止跟踪并收集日志。

              ·在"Process Monitor"窗口中，单击"Capture"按钮以停止跟踪。

              ·单击"文件" - \>"保存"并使用以下设置保存日志。

                            要保存的事件：

[                                          \[X\]]所有事件

[                                          \[\]]使用当前过滤器显示的事件

[                                          \[\]]突出事件

                            格式：

[                                          \[X\]]本机进程监视器格式（PML）

[                                          \[\]]逗号分隔值（CSV）

[                                          \[\]]可扩展标记语言（XML）

                            路径：

                                          C：\\工具\\ \<文件名\> .PML

              ·在"Step Recorder"窗口中，单击[\[Stop Record\]]。

              ·如果弹出"Internet Explorer"窗口，请单击\[关闭[\]]。

              ·单击\[保存[\]]图标。

              ·将日志保存到名为"Psr.zip"的zip文件中。

 

 

已使用 OneNote 创建。
