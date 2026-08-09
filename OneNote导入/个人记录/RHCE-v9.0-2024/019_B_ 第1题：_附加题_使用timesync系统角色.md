B: 第1题：\"附加题\"使用timesync系统角色

2025年2月6日

11:12

\# 此题环境没有NTP，记写法即可 

 

\[greg@control ansible\]\$sudo yum -y install rhel-system-roles

 

\[greg@control ansible\]\$cp -r /usr/share/ansible/roles/rhel-system-roles.timesync/ roles/timesync

 

\[greg@control ansible\]\$ vim timesync.yml

\-\--

\- name: Create NTP

  hosts: all

  vars:

    timesync_ntp_servers:

      - hostname: classroom.lab.example.com

        iburst: yes

  roles:

    - timesync

 

\[greg@control ansible\]\$ ansible-navigator run timesync.yml -m stdout

 

验证：

ansible all -a \'grep \"\^server\" /etc/chrony.conf\'

 

已使用 OneNote 创建。
