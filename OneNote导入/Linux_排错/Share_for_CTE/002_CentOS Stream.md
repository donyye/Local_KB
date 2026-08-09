CentOS Stream

2022年7月20日

8:32

2020.12.08日这天，红帽官网宣布不再对CentOS社区进行投入，而相关的投资将转到CentOS Stream项目上。

 

Gregory Kurtzer创建了CentOS项目（全名是Community Enterprise Operating System）

基于红帽的源代码对RHEL进行翻版，在重新编译的过程中去仅去除了RHEL源代码中红帽的LOGO、商标或者其他会可能会造成商业纠纷的部分并替换了主题。

 

对于CentOS的兴起，红帽在大多数情况下都是听之任之，毕竟CentOS毫不避讳自己就是RHEL的复刻，所以它的成功其实也是在给红帽做市场宣传，它从另外一个角度证明了红帽的RHEL是成功的。此外，还是会有一些用户会因为生产需要将会部分CentOS切换到RHEL，红帽多少也能从中获益。

 

在2014年，CentOS宣布加入红帽，红帽出资对CentOS项目进行赞助，出人直接参与CentOS项目的开发，并占据了CentOS社区当时9个常任董事中3个名额。

 

红帽的上游优先的政策。

 

CentOS 6生命周期结束时间为2020年11月30日

CentOS 7生命结束时间2024年6月30日

CentOS 8生命结束时间2021年12月31日

不会再有CentOS 9发布

 

所有已发布版本的生命周期结束后，CentOS项目将全面终止，已有的CentOS用户可以通过简单的两条命令将现有的CentOS切换到CentOS Stream，详见：https://www.centos.org/centos-stream/

 

整个CentOS Stream的角色和定位发生了变化，它将作为RHEL的上游，即开发版本来完善RHEL的生态和加速其创新，它和RHEL的关系简单说来包含以下几方面内容：

》其定位为RHEL的开发版本，即它现在是RHEL的上游

》CentOS Stream处于Fedora Project 和RHEL之间，提供一个有新特性的RHEL内核以及新特性的"滚动预览"（rolling preview），也就是说CentOS Stream并没有8.0\\8.1\\8.2等版本，只有"最新版"

》滚动的另外一个含义是CentOS Stream中的补丁是实时发布的，不像在RHEL里那样经过严格的测试和认证之后才会发布。

》CentOS Stream提供的包普遍要比RHEL版本更"新"， RHEL会基于CentOS Stream来做减法，选择其中成熟稳定的功能。言外之意，CentOS Stream里的软件组件的稳定性和成熟度要逊色于RHEL。

 

 

已使用 OneNote 创建。
