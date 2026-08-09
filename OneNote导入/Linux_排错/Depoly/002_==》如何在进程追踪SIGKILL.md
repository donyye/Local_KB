==》如何在进程追踪SIGKILL

2024年2月27日

16:57

How to track who/what is sending SIGKILL to a process?\
[https://access.redhat.com/solutions/165993](https://access.redhat.com/solutions/165993)

 

1. 你可能在message看到下面信息。httpd进程收到SIGKILL而结束了。

\# less /var/log/messages\
Jul[  5 11:44:45 RHEL9 systemd\[1\]: httpd.service: Main process exited, code=killed, status=9/KILL\
Jul  5 11:44:45 RHEL9 systemd\[1\]: httpd.service: Killing process 15201 (httpd) with signal SIGKILL.\
Jul  5 11:44:45 RHEL9 systemd\[1\]: httpd.service: Killing process 15202 (httpd) with signal SIGKILL.\
Jul  5 11:44:45 RHEL9 systemd\[1\]: httpd.service: Killing process 15203 (httpd) with signal SIGKILL.\
\[..\]\
Jul  5 11:44:45 RHEL9 systemd\[1\]: httpd.service: Killing process 15413 (n/a) with signal SIGKILL.\
Jul  5 11:44:45 RHEL9 systemd\[1\]: httpd.service: Killing process 15414 (n/a) with signal SIGKILL.\
Jul  5 11:44:45 RHEL9 systemd\[1\]: httpd.service: Killing process 15415 (n/a) with signal SIGKILL.\
Jul  5 11:44:45 RHEL9 systemd\[1\]: httpd.service: Failed with result \'signal\'.]

 

2\. 可以配置一个审核规则来捕获终止信号，这有助于我们识别哪个用户发起了该信号。 规则应该是这样的：

\# auditctl  -a exit,always -F arch=b64 -F a1=9 -S kill\
\# auditctl  -a exit,always -F arch=b64 -F a1=9 -S tkill\
\# auditctl  -a exit,always -F arch=b64 -F a2=9 -S tgkill\
\# auditctl  -a exit,always -F arch=b32 -F a1=9 -S kill\
\# auditctl  -a exit,always -F arch=b32 -F a1=9 -S tkill\
\# auditctl  -a exit,always -F arch=b32 -F a2=9 -S tgkill

 

3. 相关信息会记录到此LOG里。

/var/log/audit/audit.log

 

 

测试 httpd 服务，然后使用 killall -9 httpd 命令后，就会在audit.log留下记录。

time-\>Tue Jul  5 11:44:45 2022\
type=PROCTITLE msg=audit(1657001685.727:558): proctitle=6B696C6C616C6C002D39006874747064\
type=OBJ_PID msg=audit(1657001685.727:558): opid=15200 oauid=-1 ouid=0 oses=-1 obj=system_u:system_r:httpd_t:s0 ocomm=\"httpd\"\
type=SYSCALL msg=audit(1657001685.727:558): arch=c000003e syscall=62 success=yes exit=0 a0=3b60 a1=9 a2=0 a3=a items=0 ppid=1264 pid=15588 auid=1000 uid=0 gid=0 euid=0 suid=0 fsuid=0 egid=0 sgid=0 fsgid=0 tty=pts0 ses=3 comm=\"killall\" exe=\"/usr/bin/killall\" subj=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023 key=(null)

syscall=62 代表审计拦截到的syscall的代码，即sys_kill。 

pid=15588 是调用 sys_kill 函数来终止 HTTPD 服务的进程的 PID。

SYSCALL_DEFINE2 函数接受需要终止的信号和进程 ID，然后将参数传递给函数 prepare_kill_siginfo ，然后返回到 PID 首先得到验证的函数 kill_something_info ，并调用最终函数 kill_proc_info 来终止进程。

 

 

上面配置是临时的，重启系统会失效，永久保存自建规则。

\# cat /etc/audit/rules.d/myrules.rules

-a exit,always -F arch=b64 -F a1=9 -S kill

-a exit,always -F arch=b64 -F a1=9 -S tkill

-a exit,always -F arch=b64 -F a2=9 -S tgkill

-a exit,always -F arch=b32 -F a1=9 -S kill

-a exit,always -F arch=b32 -F a1=9 -S tkill

-a exit,always -F arch=b32 -F a2=9 -S tgkill

 

\# augenrules[   ]重新加载一下配置

 

查看LOG

\# ausearch -i -if /var/log/audit/audit.log

 

 

\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\--

在 auditd 中，-k 选项用于指定一个自定义的键值（key）。这个键值可以是任何你认为有助于标识和分类审计事件的字符串。

 

auditctl -a exit,always -F arch=b64 -S open -k file_access

 

在这个例子中，当有一个 64 位架构的系统上发生文件打开事件时，会被打上 file_access 这个标签。之后你就可以使用 ausearch 来搜索具有这个标签的事件：

ausearch -k file_access

 

 

 

 

 

FYI: [https://access.redhat.com/solutions/3897401](https://access.redhat.com/solutions/3897401)

\# yum install bcc bcc-tools kernel-devel-\`uname -r\` -y

 

\# /usr/share/bcc/tools/killsnoop\
TIME[      PID    COMM             SIG  TPID   RESULT\
17:43:00  2480   bash             15   9      0\
17:43:00  2480   bash             15   513]

 

[\# sleep 1000000 &\
\[3\] 5133\
\# kill 9 5133]

 

 

已使用 OneNote 创建。
