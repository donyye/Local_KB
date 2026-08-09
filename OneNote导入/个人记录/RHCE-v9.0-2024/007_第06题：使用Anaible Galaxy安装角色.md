第06题：使用Anaible Galaxy安装角色

2024年12月24日

15:07

\[greg@control ansible\]\$ vim /home/greg/ansible/roles/requirements.yml

\-\--

\- src: <http://classroom/materials/haproxy.tar>

  name: balancer

\- src: <http://classroom/materials/phpinfo.tar>

  name: phpinfo

  

\[greg@control ansible\]\$ ansible-galaxy install -r /home/greg/ansible/roles/requirements.yml

 

\#验证：查看到两个角色即可。(必做操作)

\[greg@control ansible\]\$ ansible-galaxy list

\# /home/greg/ansible/roles

\...

\- balancer, (unknown version)

\- phpinfo, (unknown version)

\...

 

已使用 OneNote 创建。
