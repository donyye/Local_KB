OpenShift Administration Configuring a Production Cluster

1. 添加两个用户，new_admin 和 new_developer， new_admin 密码redhat， new_developer 密码 developer

 

[[student@workstation ~]$ ]htpasswd -c -B -b ~/DO280/labs/auth-providers/htpasswd new_admin redhat

Adding password for user new_admin

 

[ ]将密码为 developer 的用户 new_developer 添加到文件 ~/DO280/labs/auth-providers/htpasswd 中。​new_developer 用户的密码通过 MD5 算法进行哈希处理，​因为没有指定算法，​而 MD5 是默认的哈希算法。​

 

[[student@workstation ~]$] htpasswd -b ~/DO280/labs/auth-providers/htpasswd new_developer developer

Adding password for user new_developer

 

注意：新穿件密码文件是要 -c , 后续再添加就不需要了，否则会清空之前的信息。

 

可以看到创建的两个用户的密码文件

[[student@workstation ~]$ ]cat ~/DO280/labs/auth-providers/htpasswd

new_admin:$2y$05$qQaFbpx4hbf4uZe.SMLSduTN8uN4DNJMJ4jE5zXDA57WrTRlpu2QS

new_developer:$apr1$S0TxtLXl$QSRfBIufYP39pKNsIg/nD1

 

 

2. 已 admin身份登录到 openshift 并为 new_admin 新用户添加角色

[[student@workstation ~]$ ]oc login -u admin -p redhatocp[  ][https://api.ocp4.example.com:6443](https://api.ocp4.example.com:6443)

Login successful.

 

...output omitted...

 

从 ~/DO280/labs/auth-providers/htpasswd 文件创建机密。​若要使用 HTPasswd 身份提供程序，​您必须使用名为 htpasswd 且包含 HTPasswd 用户文件 ~/DO280/labs/auth-providers/htpasswd 的密钥来定义机密。​

 

[[student@workstation ~]$] oc create secret generic localusers \

    --from-file htpasswd=~/DO280/labs/auth-providers/htpasswd \

    -n openshift-config

secret/localusers created

 

为 new_admin 用户分配 cluster-admin 角色。​并忽略用户未找到的警告。

[[student@workstation ~]$] oc adm policy add-cluster-role-to-user cluster-admin new_admin

Warning: User 'new_admin' not found

clusterrole.rbac.authorization.k8s.io/cluster-admin added: "new_admin"

 

 

3. 更新集群的HTPasswd的身份，以便可以身份验证。

先把现有的 OAuth 导出到[  ]~/DO280/labs/auth-providers 目录中名为 oauth.yaml 的文件。​

[[student@workstation ~]$ ]oc get oauth cluster[  -o yaml \> ~/DO280/labs/auth-providers/oauth.yaml]

 

编辑 oauth.yaml 文件，并在最后添加加粗的htpasswd认证信息

apiVersion: config.openshift.io/v1

kind: OAuth

...output omitted...

spec:

  identityProviders:

  - ldap:

...output omitted...

    type: LDAP

 [ - htpasswd:]

[      fileData:]

[        name: localusers]

[    mappingMethod: claim]

[    name: myusers]

[    type: HTPasswd]

 

修改上一步自定义的配置

[[student@workstation ~]$] oc replace -f ~/DO280/labs/auth-providers/oauth.yaml

oauth.config.openshift.io/cluster replaced

 

注意：

身份验证更改需要在 openshift-authentication 命名空间中重新部署 pod。​

使用 watch 命令可检查 openshift-authentication 命名空间中的工作负载状态。​

 

[[student@workstation ~]$ ]watch oc get all -n openshift-authentication

NAME                                   READY   STATUS    RESTARTS   AGE

pod/oauth-openshift-6d68ffb9dc-6f8dr   1/1     Running   3          2m

...output omitted...

这里需要等待大概几分钟。

如果之前创建的机密创建正确，​您可以使用 HTPasswd 身份提供程序进行登录。​

 

 

4. 检测之前创建的用户是否能登录

 

[[student@workstation ~]$ ]oc login -u new_admin -p redhat

Login successful.

 

...output omitted...

 

[[student@workstation ~]$ ]oc get users

NAME            UID                                   ...  IDENTITIES

admin           6126c5a9-4d18-4cdf-95f7-b16c3d3e7f24  ...  ...

new_admin       489c7402-d318-4805-b91d-44d786a92fc1  ...  myusers:new_admin

new_developer   8dbae772-1dd4-4242-b2b4-955b005d9022  ...  myusers:new_developer

 

[[student@workstation ~]$] oc login -u new_developer -p developer

Login successful.

 

...output omitted...

 

[[student@workstation ~]$ ]oc get nodes

Error from server (Forbidden): nodes is forbidden: User "new_developer" cannot list resource "nodes" in API group "" at the cluster scope

 

注意：new_developer 用户因为没有赋予cluster-admin 角色，所以没有权限查看。

 

new_admin 用可以显示当前身份列表

[[student@workstation ~]$ ]oc get identity

NAME                   IDP NAME   IDP USER NAME  USER NAME

                                            USER UID

...                    ...        ...            admin

                                            6126c5a9-4d18-4cdf-95f7-b16c3d3e7f24

myusers:new_admin      myusers    new_admin      new_admin

                                            489c7402-d318-4805-b91d-44d786a92fc1

myusers:new_developer  myusers    new_developer  new_developer

                                            8dbae772-1dd4-4242-b2b4-955b005d9022

 

5. 以 new_admin 用户身份，​创建一个名为 manager 且密码为 redhat 的 HTPasswd 用户。

将机密中的文件数据提取到 ~/DO280/labs/auth-providers/htpasswd 文件。​

 

[[student@workstation ~]$] oc extract secret/localusers -n openshift-config \

[    ]--to ~/DO280/labs/auth-providers/ --confirm

/home/student/DO280/labs/auth-providers/htpasswd  

 

[[student@workstation ~]$] htpasswd -b ~/DO280/labs/auth-providers/htpasswd manager redhat

Adding password for user manager

 

[[student@workstation ~]$ ]cat ~/DO280/labs/auth-providers/htpasswd

new_admin:$2y$05$qQaFbpx4hbf4uZe.SMLSduTN8uN4DNJMJ4jE5zXDA57WrTRlpu2QS

new_developer:$apr1$S0TxtLXl$QSRfBIufYP39pKNsIg/nD1

manager:$apr1$HZ/9tC6b$j2OcHHg2GO2SSu1wyGOge.

 

注意：运行下面命令添加其他用户后，​必须更新机密。​使用 oc set data secret 命令来更新机密。​如果命令失败，​则稍等片刻，​让 oauth operator完成重新加载，​然后再次运行命令。

 

[[student@workstation ~]$] oc set data secret/localusers \

    --from-file htpasswd=~/DO280/labs/auth-providers/htpasswd \

    -n openshift-config

secret/localusers data updated

 

使用 watch 命令可检查 openshift-authentication 命名空间中的工作负载状态。​

 

[[student@workstation ~]$ ]watch oc get all -n openshift-authentication

NAME                                   READY   STATUS    RESTARTS   AGE

pod/oauth-openshift-6d68ffb9dc-6f8dr   1/1     Running   3          2m

...output omitted...

 

使用 manager 用户身份登录集群

[[student@workstation ~]$ ]oc login -u manager -p redhat

Login successful.

 

...output omitted...

 

 

6. 以 manager 用户身份创建一个 auth-providers 项目，​然后验证 new_developer 用户无法访问该项目。​

 

[[student@workstation ~]$ ]oc new-project auth-providers

Now using project "auth-providers" on server <https://api.ocp4.example.com:6443>".

...output omitted...

 

[[student@workstation ~]$ ]oc login -u new_developer -p developer

Login successful.

 

...output omitted...

 

尝试删除 auth-providers 项目。​

[[student@workstation ~]$ ]oc delete project auth-providers

Error from server (Forbidden): projects.project.openshift.io "auth-providers" is forbidden: User "new_developer" cannot delete resource "projects" in API group "project.openshift.io" in the namespace "auth-providers"

 

 

7. 以 new_admin 用户身份登录，改 manager 用户的密码。​

 

[[student@workstation ~]$ ]oc login -u new_admin -p redhat

Login successful.

 

...output omitted...

 

[[student@workstation ~]$] oc extract secret/localusers -n openshift-config \

    --to ~/DO280/labs/auth-providers/ --confirm

/home/student/DO280/labs/auth-providers/htpasswd

 

使用"openssl rand -hex 15"命令随机生成一个秘密

[[student@workstation ~]$ ]MANAGER_PASSWD="$(openssl rand -hex 15)"

 

[[student@workstation ~]$] htpasswd -b ~/DO280/labs/auth-providers/htpasswd \

    manager $

Updating password for user manager

 

更新机密：

[[student@workstation ~]$] oc set data secret/localusers \

    --from-file htpasswd=~/DO280/labs/auth-providers/htpasswd \

    -n openshift-config

secret/localusers data updated

 

使用 watch 命令可检查 openshift-authentication 命名空间中的工作负载状态。​

 

[[student@workstation ~]$ ]watch oc get all -n openshift-authentication

NAME                                   READY   STATUS    RESTARTS   AGE

pod/oauth-openshift-6d68ffb9dc-6f8dr   1/1     Running   3          2m

...output omitted...

 

[student@workstation ~]$ oc login -u manager -p $

Login successful.

 

...output omitted...

 

 

以 manager 用户身份登录，​以验证更新的密码。​

 

[[student@workstation ~]$ ]oc login -u manager -p $

Login successful.

 

...output omitted...

 

 

 

8. 删除 manager 用户

 

以 new_admin 用户身份登录。​

 

[[student@workstation ~]$ ]oc login -u new_admin -p redhat

Login successful.

 

...output omitted...

 

将机密中的文件数据提取到 ~/DO280/labs/auth-providers/htpasswd 文件。​

 

[[student@workstation ~]$] oc extract secret/localusers -n openshift-config \

    --to ~/DO280/labs/auth-providers/ --confirm

/home/student/DO280/labs/auth-providers/htpasswd

 

将 manager 用户从 ~/DO280/labs/auth-providers/htpasswd 文件中删除

 

[[student@workstation ~]$] htpasswd -D ~/DO280/labs/auth-providers/htpasswd manager

Deleting password for user manager

 

更新该机密：

[[student@workstation ~]$ ]oc set data secret/localusers \

    --from-file htpasswd=~/DO280/labs/auth-providers/htpasswd \

    -n openshift-config

secret/localusers data updated

 

使用 watch 命令可检查 openshift-authentication 命名空间中的工作负载状态。​

 

[[student@workstation ~]$ ]watch oc get all -n openshift-authentication

NAME                                   READY   STATUS    RESTARTS   AGE

pod/oauth-openshift-6d68ffb9dc-6f8dr   1/1     Running   3          2m

...output omitted...

 

运行 oc set data 命令几分钟后，​将开始重新部署。​等待新 pod 都处于运行状态。​按 Ctrl+C 退出 watch 命令。​

 

以 manager 用户身份登录。​如果登录成功，​则重试，​直到登录失败。​

 

[[student@workstation ~]$ ]oc login -u manager -p $

Login failed (401 Unauthorized)

Verify you have provided correct credentials.

 

以 new_admin 用户身份登录。​

 

[[student@workstation ~]$ ]oc login -u new_admin -p redhat

Login successful.

 

...output omitted...

 

删除 manager 用户的身份资源。​

 

[[student@workstation ~]$ ]oc delete identity "myusers:manager"

identity.user.openshift.io "myusers:manager" deleted

 

删除 manager 用户的用户资源。​

 

[[student@workstation ~]$ ]oc delete user manager

user.user.openshift.io manager deleted

 

列出当前用户，​以验证 manager 用户已被删除。​

 

[[student@workstation ~]$ ]oc get users

NAME            UID                                   ...  IDENTITIES

admin           6126c5a9-4d18-4cdf-95f7-b16c3d3e7f24  ...  ...

new_admin       489c7402-d318-4805-b91d-44d786a92fc1  ...  myusers:new_admin

new_developer   8dbae772-1dd4-4242-b2b4-955b005d9022  ...  myusers:new_developer

 

显示当前身份的列表，​以验证 manager 身份已被删除。​

 

[[student@workstation ~]$ ]oc get identity

NAME                   IDP NAME   IDP USER NAME  USER NAME

                                            USER UID

...                    ...        ...            admin

                                            6126c5a9-4d18-4cdf-95f7-b16c3d3e7f24

myusers:new_admin      myusers    new_admin      new_admin

                                            489c7402-d318-4805-b91d-44d786a92fc1

myusers:new_developer  myusers    new_developer  new_developer

                                            8dbae772-1dd4-4242-b2b4-955b005d9022

 

提取机密，​并验证是否仅显示了 new_admin 和 new_developer 用户。​使用 --to - 会将机密发送到 STDOUT，​而不是将其保存到文件。​

 

[[student@workstation ~]$ ]oc extract secret/localusers -n openshift-config --to -

# htpasswd

new_admin:$2y$05$qQaFbpx4hbf4uZe.SMLSduTN8uN4DNJMJ4jE5zXDA57WrTRlpu2QS

new_developer:$apr1$S0TxtLXl$QSRfBIufYP39pKNsIg/nD1

 

 

9. 删除身份提供程序，​并清理所有用户。

以 admin 用户身份登录。​

 

[[student@workstation ~]$ ]oc login -u admin -p redhatocp

Login successful.

 

...output omitted...

 

删除 auth-providers 项目。​

[[student@workstation ~]$ ]oc delete project auth-providers

project.project.openshift.io "auth-providers" deleted

 

在原地编辑资源，​以从 OAauth 中删除身份提供程序：

[[student@workstation ~]$ ]oc edit oauth

 

删除第 34 行 ldap 身份提供程序定义后的所有行。​您的文件应与以下内容相符：

apiVersion: config.openshift.io/v1

kind: OAuth

metadata:

  name: cluster

spec:

  identityProviders:

  - ldap:

...output omitted...

    type: LDAP

[  # Delete all lines below]

[  - htpasswd:]

[      fileData:]

[        name: localusers]

[    mappingMethod: claim]

[    name: myusers]

[    type: HTPasswd]

 

保存您的更改，​然后验证 oc edit 命令是否已应用这些更改：

oauth.config.openshift.io/cluster edited

 

使用 watch 命令可检查 openshift-authentication 命名空间中的工作负载状态。​

[[student@workstation ~]$ ]watch oc get all -n openshift-authentication

NAME                                   READY   STATUS    RESTARTS   AGE

pod/oauth-openshift-6d68ffb9dc-6f8dr   1/1     Running   3          2m

...output omitted...

运行 oc edit 命令几分钟后，​将开始重新部署。​等待新 pod 都处于运行状态。​按 Ctrl+C 退出 watch 命令。​

 

删除 openshift-config 命名空间中的 localusers 机密。​

[[student@workstation ~]$] oc delete secret localusers -n openshift-config

secret "localusers" deleted

 

删除所有身份资源。​

[[student@workstation ~]$] oc delete identity --all

identity.user.openshift.io "Red Hat Identity Management:dWlk...jb20" deleted

identity.user.openshift.io "myusers:new_admin" deleted

identity.user.openshift.io "myusers:new_developer" deleted

 

删除所有用户资源。​

[[student@workstation ~]$] oc delete user --all

user.user.openshift.io "admin" deleted

user.user.openshift.io "developer" deleted

user.user.openshift.io "new_admin" deleted

user.user.openshift.io "new_developer" deleted
