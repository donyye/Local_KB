Linux 又一个 bug

Monday, January 25, 2016

1:33 PM

- ::: 
    -------------------------------------- -----------------------------------------------------------------------
    主题       Linux 又一个 bug
    发件人     Ye, Dony
    收件人     CN XMN TS ENT L2 SME
    发送时间   Monday, January 25, 2016 11:20 AM
    -------------------------------------- -----------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

  Hi, All

   

  目前Perception Point研究团队已经在Linux OS 的内核中发现了一个0 day bug,它是通过一个keyring的功能获取一个本地提权，也就是获取OS root 权限。这个bug所影响的Linux OS范围非常广，包括了服务器版本、个人计算机版本、安卓版本（手机和平板电脑）。而此bug在2012年已存在kernel里，但是最近才被发现，但是到目前为止，该组织还没发现针对此漏洞的攻击事件。

  <http://perception-point.io/2016/01/14/analysis-and-exploitation-of-a-linux-kernel-vulnerability-cve-2016-0728/>

  Redhat也对此"CVE-2016-0728"做出了声明，列出了影响的OS版本。

  Redhat KB：https://access.redhat.com/node/2131021

   

  Diagnostic Steps:

  This issue was introduced in commit [3a50597de8635cd05133bd12c95681c82fe7b878](http://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=3a50597de8635cd05133bd12c95681c82fe7b878) , which was introduced in the kernel version 3.10. All Red Hat Enterprise Linux kernels after this version are affected. This document will be updated with versions containing the fix when available.

  The version of the kernel a system is running can be confirmed with the uname command:

  [Raw](https://access.redhat.com/articles/2131021)

  \# uname -r

  3.10.0-327.el7.x86_64

   

  ![[Technology_ALL_Linux 问题收集_017_Linux 又一个 bug_001.jpg]]

  毫无疑问，RHEL7又榜上有名，看来不到7.4都不会是个稳定的版本。

  但关于此bug的kernel补丁目前redhat的补丁库里还没找到，但是有提到如果怀疑被攻击可以提交ticket给它解决。

  那怎样发现是否存在攻击了，你可以通过下面命令查看：

  \$ cat /proc/keys \| grep TEST_KEY

  01cca8cf I\--Q\-\-- 50076394 perm 3f3f3f3f     0     0 keyring   TEST_KEY: empty

  如果有信息输出，恭喜你很大可能会已被黑了，而在其它的OS log不会留下任何痕迹，除非你做了其它的安全审计。解决方法有两个，有redhat服务的可提交ticket到redhat，没服务的就只能重新安装OS，不要想有什么其它方法拯救你的OS，被黑了root权限就是死路一条。

  最后说一下有可能对我们的影响和可能遇到的问题：

  1.  文件权限的突然改变。
  2.  某些文件突然消失或是多了奇怪的文件。
  3.  机器负载异常，进程异常。
  4.  Unstable 问题。

   

  因这些问题的隐藏性非常强，也不太好判断，而Redhat目前也没给出一个正式的kernel版本去fix这个issue，所以遇到此问题最好还是loop OS vendor进行检查。

  另外补充一些受影响的Linux版本：

  RHEL7、CentOServer7、Scientific Linux 7 、Debinan Linux 8.x （jessie）与 9.x (stretch)、SUSE Enterprise 12 等。

   

   

   

   

   

   

   

   

  Best Regards

   

  Ye Jian Yuan

  Dell \| Enterprise Support Services

  Mail Address:dony_ye@dell.com

   

 

已使用 OneNote 创建。
