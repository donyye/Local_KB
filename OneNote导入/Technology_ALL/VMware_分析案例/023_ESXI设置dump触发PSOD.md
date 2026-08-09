ESXI设置dump触发PSOD

Tuesday, February 23, 2016

9:07 AM

Dump没有生成是因为psod的时候local storage 已经无法访问了，这属于特殊情况。 

如果客户担心以后还会出现类似情况时候得不到dump，可以安装设置ESXi Net-dump。相关的安装和设置方式可以参考以下KB文章。

按照下面的文档部署好以后，可以通过Idrac里面的电源管理界面中的NMI 开关让ESXi  主动发生一次人为的PSOD，然后等待并观察net-dump collector端是否已经捕获到了dump，如果能够捕获则证明设置OK，如果不能可联系我们检查。

 

<https://kb.vmware.com/selfservice/search.do?cmd=displayKC&docType=kc&docTypeID=DT_KB_1_1&externalId=2002955>

 

已使用 OneNote 创建。
