[ ]第16题：更新Ansible库密钥

2025年2月6日

11:12

\[greg@control ansible\]\$ wget <http://classroom/materials/salaries.yml>

 

\[greg@control ansible\]\$ ansible-vault rekey salaries.yml

Vault password: \`insecure8sure\`

New Vault password: \`bbs2you9527\`

Confirm New Vault password: \`bbs2you9527\`

Rekey successful

 

\# 验证： 用新密码验证是否可以解密查看内容。(必做操作)

\[greg@control ansible\]\$ ansible-vault view salaries.yml

Vault password: \`bbs2you9527\`

haha

 

\[greg@control ansible\]\$ vim ansible.cfg  

vault_password_file=/home/greg/ansible/secret.txt  

 

已使用 OneNote 创建。
