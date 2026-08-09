第01题：安装和配置Ansible

2024年12月24日

15:04

\$ ssh greg@control

\$ sudo yum -y install ansible-core ansible-navigator

\$ mkdir -p /home/greq/ansible/roles

\$ cd /home/greg/ansible

\$ cat /etc/ansible/ansible.cfg

\...\...

ansible-config init \--disabled \>

\...\...

\$ ansible-config init \--disabled \> /home/greg/ansible/ansible.cfg

 

\# 创建集合存储目录

\$ mkdir /home/greg/ansible/mycollection

\# 编辑配置⽂件

\$ vim ansible.cfg

\[defaults\]

inventory=/home/greg/ansible/inventory

remote_user=greg

host_key_checking=False

roles_path=/home/greg/ansible/roles:/usr/share/ansible/roles[   \--]》复制一下改一下

collections_path=/home/greg/ansible/mycollection:\~/.ansible/collections:/usr/share/ansible/collections

 

\[privilege_escalation\]

become=True

 

\# 确认生效的配置文件(必做操作)

\[greg@control ansible\]\$ ansible \--version

\[greg@control ansible\]\$ ansible-galaxy list

 

\[greg@control ansible\]\$ vim /home/greg/ansible/inventory

\[dev\]

node1

 

\[test\]

node2

 

\[prod\]

node3

node4

 

\[balancers\]

node5

 

\[webservers:children\]

prod

 

\# 验证： 如果可以ping通所有节点，证明配置⽂件、账户、清单都没有问题。(必做操作)

\[greg@control ansible\]\$ ansible-inventory \--graph

\[greg@control ansible\]\$ ansible all -m ping -o

\# ansible 现在所有模块都统一在容器的一个镜像里管理，所以这里为什么要登陆容器和下载镜像。

\[greg@control ansible\]\$ podman login utility.lab.example.com -u admin -p redhat

\[greg@control ansible\]\$ ansible-navigator images

\#验证：此命令可以验证podman的登录、执行环境下载正确，查看集合。(必做操作，不做后影响后续题目使用doc以及跑剧本)

 

\[greg@control ansible\]\$ ansible-navigator collections

 

已使用 OneNote 创建。
