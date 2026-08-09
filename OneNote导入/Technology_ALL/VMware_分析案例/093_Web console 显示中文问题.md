Web console 显示中文问题

2018年8月21日

15:24

 VMware Support Request 18895668508

========================\
\
客户发现所有datastore名字中带有中文的VM的web console 通过vCenter无法打开, console中提示\"the console has been disconnected, close this windows and re-launch the console to reconnect\", 我们做了以下测试并且结果如下：

1: web直接登陆esxi的client后打开web console成功

2: 在vCenter中打开VM的web console，只要该VM的datastore目录中带中文就无法成功。

3: VMRC console则不管是否带中文的VM都没有影响，可以使用。

4: 我们参考KB 2152043 对vsphere-client.json文件做了修改发现无效，该workaround不好用。

5:[  clone]该VM并且设置为不带中文字体，webconsole工作正常

 

我们检查vsphere_client_virgo.log，发现有以下异常片段，看起来好像在握手阶段出现了java exception，片段太长无法全部粘贴。

\[2018-08-17T09:03:18.742+08:00\] \[ERROR\] http-bio-9443-exec-1          o.a.c.c.C.\[.\[localhost\].\[/vsphere-client\].\[AuthdAdapter\]          Servlet.service() for servlet \[AuthdAdapter\] in context with path \[/vsphere-client\] threw exception java.lang.RuntimeException: AuthdException: 501 Command \'CONNECT/vmfs/volumes/5a032596-6d69d450-6190-801844e9044a/??-test/??-test.vmx\' not authorized for specified VM

at com.vmware.vise.vim.commons.mks.tomcat.TomcatAuthdAdapterServlet.doGet(TomcatAuthdAdapterServlet.java:107)

at javax.servlet.http.HttpServlet.service(HttpServlet.java:735)

at javax.servlet.http.HttpServlet.service(HttpServlet.java:848)

at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:303)

at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:208)

at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:52)

at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:241)

Date Entered: 20/08/2018

Support Request ID: 18895668508

Preferred Contact Method: Email

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

新資料裡的 vsphere-client.json 是修改過的，看起來是回滾前收集的資料，裡面的確有照著 KB 加了 \"-Dfile.encoding=UTF-8\" 

 

        \"-Djava.ext.dirs=%VMWARE_JAVA_HOME%/lib/ext;%VMWARE_CIS_HOME%/jre_ext\",

        \"-Djava.library.path=%VMWARE_CIS_HOME%/vsphere-client/server/wrapper/lib\",

        \"-Dgemini.blueprint.default.timeout=900\",

        \"-Dvlsi.client.vecs.certstore=false\",

        \"-Dorg.apache.tomcat.websocket.DISABLE_BUILTIN_EXTENSIONS=true\",

        \"-Dfile.encoding=UTF-8\"

        \"-classpath\",

 

不過仔細看一下前後幾行，您不覺得 \"-Dfile.encoding=UTF-8\" 的最後面應該要加個 , 嗎？

 

請再試一次停掉 web client service，把那一行改成

 

\"-Dfile.encoding=UTF-8\", 

 

然後啟動 web client service 再看看結果怎麼樣，謝謝。

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

 

Hi Vic,

确实是那个逗号的问题，加上后web console就正常了。

这个CASE请关闭吧，谢谢。

 

 

 

已使用 OneNote 创建。
