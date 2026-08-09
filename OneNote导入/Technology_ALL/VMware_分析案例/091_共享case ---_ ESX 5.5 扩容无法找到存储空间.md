共享case \-\--\> ESX 5.5 扩容无法找到存储空间

2018年6月15日

9:17

- ::: 
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    主题       共享case \-\--\> ESX 5.5 扩容无法找到存储空间
    发件人     Zeng, Jackie
    收件人     GC VCP Club
    抄送       Xie, YuXuan; Wu, Phillip; Cao, Leo (VMware); Song, Shubao
    发送时间   2018年6月14日 8:53
    -------------------------------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  :::

   

  Dell - Internal Use - Confidential 

   

  各位VCP 专家、工程师：

   

  最近TSM Shubao遇到一个VM 扩容的case，处理之后觉得很有意思，将此case 处理情况及结果共享给各位VCP专家，希望后续如果有遇到类似问题可以快速解决。

  1.  硬件环境：R930/940\*10+M1000e(M630\*16)+SC5020+SC2080,以及几台IBM 服务器。 其中M630 通过刀箱的MXL以FCOE+ Brocade SAN switch 方式与SC2080相连，R930+IBM Server + Brocade SAN switch与SC5020和SC2080相连
  2.  软件/OS 环境： VMware 5.5 ，Linux + Oracle 集群
  3.  问题：客户SC2080 添加硬盘柜扩容,需要添加到M630 的data store 里面，但是无法扩容进去，详情参考图片。客户表示如果R930 同样的操作是没有问题的，从现有情况来看唯一的区别就是M630以及FCOE链接，由于是生产环境，不敢用命令行尝试。
  4.  解决过程：从客户的描述来看，操作是肯定没有问题的，基于R930/940扩容没有问题，找区别： FC直连和FCOE --卡的驱动？后建议客户尝试将生产环境迁移，腾出一台M630做测试（工作量太大了L）。寻求强大的Google各种关键词搜索，终于找到解决方法：[https://serverfault.com/questions/672880/no-lun-available-to-extend-datastore](https://serverfault.com/questions/672880/no-lun-available-to-extend-datastore)

  Try connecting directly to the ESXi host (using root user), and not vCenter server through the client. I\'ve experienced this issue, and connecting directly to the host resolved the issue.

               客户通过该方法解决了问题，Very HappyJ

   

  ![[Technology_ALL_VMware_分析案例_091_共享case ---_ ESX 5.5 扩容无法找到存储空间_001.jpg]]

   

  再次感谢TSM Shubao 共享！

   

   

  Jackie Zeng

  Resolution Manager, Great China Customer Support Services

  Dell EMC \| Support and Deployment Services

  Dell line: 8886135

  Phone: +86 592 8186135

  [Jackie_Zeng@dell.com](mailto:Jackie_Zeng@dell.com)

   

   

 

已使用 OneNote 创建。
