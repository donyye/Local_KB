[Section 1.2: 指导练习: 资源清单](https://rol.redhat.com/rol/app/courses/do280-4.14/pages/ch01s02)

- 指导练习: 资源清单

  从存储在 Git 服务器中的 YAML 文件的资源清单部署和更新应用。​

  成果

  - 从存储在 GitLab 存储库中的 YAML 文件的资源清单部署应用。​
  - 检查新清单中是否存在潜在的更新问题。​
  - 从新的 YAML 清单更新应用部署。​
  - 必要时，​强制重新部署容器集。​

  在 workstation 计算机上，​以 student 用户身份使用 lab 命令来为本练习做好系统准备。​

  \
   

  [[student@workstation ~]$ ]lab start declarative-manifests

  \
   

  1. 登录 OpenShift 集群，​创建 declarative-manifests 项目。​

  1.1. 以 developer 用户身份登录群集。​

  [[student@workstation ~]$ ]oc login -u developer -p developer \\
  [  ][https://api.ocp4.example.com:6443](https://api.ocp4.example.com:6443)
  Login successful.

  ...output omitted...

   

  1.2. 创建 declarative-manifests 项目。

  [[student@workstation ~]$ ]oc new-project declarative-manifests\
  Now using project "declarative-manifests" on server ...\
  ...output omitted...

   

  2. 从 Git 存储库克隆 declarative-manifest 项目。​

  2.1 将目录更改到项目 labs 目录。​

  [[student@workstation ~]$ ]cd ~/DO280/labs

   

  2.2 从 [https://git.ocp4.example.com/developer/declarative-manifests.git](https://git.ocp4.example.com/developer/declarative-manifests.git) 克隆 Git 存储库。​

  [[student@workstation lab]$ ]git clone[  ][https://git.ocp4.example.com/developer/declarative-manifests.git](https://git.ocp4.example.com/developer/declarative-manifests.git)
  Cloning into 'declarative-manifests'...\
  remote: Enumerating objects: 24, done.\
  remote: Counting objects: 100% (24/24), done.\
  remote: Compressing objects: 100% (21/21), done.\
  remote: Total 24 (delta 8), reused 0 (delta 0), pack-reused 0
  Receiving objects: 100% (24/24), done.\
  Resolving deltas: 100% (8/8), done.

   

  2.3. 前往 declarative-manifest 目录。​

  [[student@workstation lab]$ ]cd declarative-manifests[\
  [student@workstation declarative-manifests]$]

   

   

  3. 检测 Git 存储库的内容

  3.1 列出 declarative-manifests 目录的内容

  [[student@workstation declarative-manifests]$ ]ls -lA\
  total 12
  -rw-rw-r--. 1 student student 3443 Jun 21 16:39 database.yaml\
  -rw-rw-r--. 1 student student 2278 Jun 21 16:39 exoplanets.yaml\
  drwxrwxr-x. 8 student student[  163 Jun 21 16:39 .git\
  -rw-rw-r--. 1 student student    0 Jun 21 16:39 .gitkeep\
  -rw-rw-r--. 1 student student   11 Jun 21 16:39 README.md]

   

   

  3.2 查看Git存仓库中的提交

  [[student@workstation declarative-manifests]$ ]git log --oneline\
  4045336 (HEAD -\> main, tag: third, origin/v1.1.1, origin/main, origin/HEAD) Exoplanets v1.1.1[  ][ --\> ]第一个版本

  ad455b2 Database v1.1.1
  821420c (tag: second, origin/v1.1.0) Exoplanets v1.1.0[  ]--\> 第二个版本

  d9abeb0 (tag: first, origin/v1.0) Exoplanets v1.0[  ][  --\> ]第一个版本

  a11396e Database v1.0
  e868a90 README\
  18ddf3c Initial commit

   

  4. 部署第一个应用版本的资源清单。

  4.1 切换到 v1.0 分支，​其中包含应用第一个版本的 YAML 清单。​

   

  [[student@workstation declarative-manifests]$ ]git checkout v1.0
  branch 'v1.0' set up to track 'origin/v1.0'.\
  Switched to a new branch 'v1.0'

   

   

  4.2 验证应用的 YAML 资源清单。​

  [[student@workstation declarative-manifests]$ ]oc apply -f .[  --validate=true --dry-run=server]
  configmap/database created (server dry run)
  secret/database created (server dry run)
  deployment.apps/database created (server dry run)
  service/database created (server dry run)
  configmap/exoplanets created (server dry run)
  secret/exoplanets created (server dry run)
  deployment.apps/exoplanets created (server dry run)
  service/exoplanets created (server dry run)
  route.route.openshift.io/exoplanets created (server dry run)

  # 运行说明服务器可以接受这些产生，结果是可以创建。

   

  4.3 部署 exoplanets 应用。​

  [[student@workstation declarative-manifests]$ ]oc apply -f .\
  configmap/database created\
  secret/database created\
  deployment.apps/database created\
  service/database created\
  configmap/exoplanets created\
  secret/exoplanets created\
  deployment.apps/exoplanets created\
  service/exoplanets created\
  route.route.openshift.io/exoplanets created

  # 没有问题后，真的创建这些资源

  # 可以使用下面命令查看进度

  # Kubectl get deployment

  # oc get pod

   

   

  4.4 列出部署和容器集。​如果 exoplanets 容器集在数据库变为可用之前尝试访问数据库，​则容器集可能会进入临时的崩溃循环回退状态。​等待容器集就绪。​按 Ctrl+C 退出 watch 命令。

  [[student@workstation declarative-manifests]$ ]watch oc get deployments,pods

  Every 2.0s: oc get deployments,pods ...

  NAME[                       ]READY[   UP-TO-DATE   AVAILABLE   AGE\
  deployment.apps/database   ]1/1[     1            1           32s\
  deployment.apps/exoplanets ]1/1[     1            1           32s]

  NAME[                            ]READY[   STATUS    RESTARTS   AGE\
  pod/database-6fddbbf94f-2pghj   ]1/1[     Running   0          32s\
  pod/exoplanets-64c87f5796-bw8tm ]1/1[     Running   0          32s]

   

  4.5 列出 exoplanets 应用的路由。​\<-- 这个是openshift的功能

  [[student@workstation declarative-manifests]$ ]oc get routes -l app=exoplanets[  ]
  NAME[         HOST/PORT                                                ...\
  exoplanets   ]exoplanets-declarative-manifests.apps.ocp4.example.com[   ...]

  # 只查看具有 exoplanets 的路由

  # 反回的是你访问这个pod的方法，一个主机名。

   

  4.6 在 Web 浏览器中打开路由 URL。​应用版本为 v1.0。​

  [http://exoplanets-declarative-manifests.apps.ocp4.example.com/](http://exoplanets-declarative-manifests.apps.ocp4.example.com/)

  ![[My-Case_runing_018_Section 1.2_ 指导练习_ 资源清单_001.png]]

   

   

  5. 部署 exoplanets 应用的第二个版本。​

  5.1 切换到 Git 存储库的 v1.1.0 分支

  [[student@workstation declarative-manifests]$ ]git checkout v1.1.0
  branch 'v1.1.0' set up to track 'origin/v1.1.0'.\
  Switched to a new branch 'v1.1.0'

  # 可以使用 "git branch"来确认。

   

  5.2 检查新清单中的更改。​

  [[student@workstation declarative-manifests]$ ]oc diff -f .\
  ...output omitted...\
  [         - secretRef:\
               name: exoplanets\
  -        image: registry.ocp4.example.com:8443/redhattraining/exoplanets:v1.0
  +        image: registry.ocp4.example.com:8443/redhattraining/exoplanets:v1.1.0
           imagePullPolicy: Always\
           livenessProbe:\
             failureThreshold: 3
  ]# checkout 后内容就会发生变化，可以查看到有那些变化。

   

  5.3 应用新清单中的更改。​

  [[student@workstation declarative-manifests]$ ]oc apply -f .\
  configmap/database unchanged\
  secret/database configured\
  deployment.apps/database configured\
  service/database configured\
  configmap/exoplanets unchanged\
  secret/exoplanets configured\
  deployment.apps/exoplanets configured\
  service/exoplanets unchanged\
  route.route.openshift.io/exoplanets configured

  # 确认发生了什么变化后发布

   

  5.4 列出部署和容器集。​等待应用容器集就绪。​按 Ctrl+C 退出 watch 命令。​

   

  [[student@workstation declarative-manifests]$ ]watch oc get deployments,pods\
  Every 2.0s: oc get deployments,pods ...

  NAME[                       READY   UP-TO-DATE   AVAILABLE   AGE\
  deployment.apps/database   1/1     1            1           6m32s\
  deployment.apps/exoplanets 1/1     1            1           6m33s]

  NAME[                            ]READY[   STATUS    RESTARTS   AGE\
  pod/database-6fddbbf94f-2pghj   1/1     Running   0          6m33s\
  pod/exoplanets-74c85f5796-tw8tf ]1/1[     Running   0          32s]

   

  5.5 列出 exoplanets 应用的路由。​

  [[student@workstation declarative-manifests]$ ]oc get routes -l app=exoplanets\
  NAME[         HOST/PORT                                                ...\
  exoplanets   ]exoplanets-declarative-manifests.apps.ocp4.example.com[   ...]

  # 通过命令再去查看地址，访问地址是没有变化的。

   

  5.6 在 Web 浏览器中打开路由 URL。​应用版本为 v1.1.0。​

  [http://exoplanets-declarative-manifests.apps.ocp4.example.com/](http://exoplanets-declarative-manifests.apps.ocp4.example.com/)

  ![[My-Case_runing_018_Section 1.2_ 指导练习_ 资源清单_002.png]]

   

   

   

  6. 部署 exoplanets 应用的第三个版本。​

  6.1. 切换到 Git 存储库的 v1.1.1 分支。​

  [[student@workstation declarative-manifests]$ ]git checkout v1.1.1
  branch 'v1.1.1' set up to track 'origin/v1.1.1'.\
  Switched to a new branch 'v1.1.1'\
  # 可以使用 "git branch"来确认。

   

  6.2. 查看当前部署的应用版本和更新后的资源清单之间的差异。​

  [[student@workstation declarative-manifests]$ ]oc diff -f .\
  ...output omitted...\
  [ kind: Secret ][   -]-\> 机密资源已更改。​

  metadata:\
  [   annotations:\
  ]...output omitted...\
  -[  DB_USER: '*** (b]efore)' [   --\>机密资源的] DB_USER 字段已更改。​

  +[  DB_USER: '*** (after)'\
   kind: Secret\
   metadata:\
     annotations:]

   

  6.3. 检查当前应用容器集

  [[student@workstation declarative-manifests]$ ]oc get pods\
  NAME[                          READY   STATUS    RESTARTS   AGE\
  database-6fddbbf94f-brlj6     1/1     Running   0          44m\
  exoplanets-674cc57b5d-mv8kd   1/1     Running   0          18m]

   

   

  6.4. 部署应用的新版本。

  [[student@workstation declarative-manifests]$ ]oc apply -f .\
  configmap/database unchanged\
  secret/database configured\
  deployment.apps/database configured\
  service/database configured\
  configmap/exoplanets unchanged\
  secret/exoplanets configured\
  deployment.apps/exoplanets unchanged\
  service/exoplanets unchanged\
  route.route.openshift.io/exoplanets configured

   

   

  6.5. 再次检查当前应用容器集

  [[student@workstation declarative-manifests]$ ]oc get pods\
  NAME[                          READY   STATUS             RESTARTS     AGE\
  database-6fddbbf94f-brlj6     1/1     Running            0            10m\
  exoplanets-674cc57b5d-mv8kd   0/1     CrashLoopBackOff   4 (14s ago)  2m][    \<-- ]已经崩溃了

  虽然机密已更新，​但已部署的应用容器集不会更改。​这些未更新的容器集是个问题，​因为容器集会在启动时加载机密和配置映射。​目前，​容器集具有先前配置中的陈旧值，​因此可能会崩溃。​\
  # 如果发现此问题，可以使用下面第7步进行强制重启。

   

   

  7. 强制重启 exoplanets 应用，​以清除所有过时的配置数据。​

  7.1. 使用 oc get deployments 命令确认部署。​

  [[student@workstation declarative-manifests]$ ]oc get deployments\
  NAME[         READY   UP-TO-DATE   AVAILABLE   AGE\
  database     1/1     1            1           32m\
  exoplanets   0/1     1            0           32m]

   

  7.2. 使用 oc rollout 命令重新启动数据库部署。

  [[student@workstation declarative-manifests]$ ]oc rollout restart deployment/database\
  deployment.apps/database restarted

   

  7.3. 使用 oc rollout 命令重新启动 exoplanets 部署。​

  [[student@workstation declarative-manifests]$ ]oc rollout restart deployment/exoplanets\
  deployment.apps/exoplanets restarted

   

  7.4. 列出容器集。​如果 exoplanets 容器集在数据库变为可用之前尝试访问数据库，​则容器集可能会进入临时的崩溃循环回退状态。​等待应用容器集就绪。​按 Ctrl+C 退出 watch 命令。​

   

  [[student@workstation declarative-manifests]$ ]watch oc get pods\
  Every 2.0s: oc get deployments,pods ...

  NAME[                          READY   STATUS    RESTARTS   AGEE\
  database-7c767c4bd7-m72nk     1/1     Running   0          32s\
  exoplanets-64c87f5796-bw8tm   1/1     Running   0          32s]

   

  7.5. 使用带有 -o yaml 选项的 oc get deployment 命令来查看 last-applied-configuration 注释。​

  [[student@workstation declarative-manifests]$ ]oc get deployment exoplanets -o yaml\
  apiVersion: apps/v1
  kind: Deployment\
  metadata:\
  [  annotations:\
  ][    deployment.kubernetes.io/revision: "3"\
      description: Defines how to deploy the application server\
      kubectl.kubernetes.io/last-applied-configuration: |\
        {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":...\
      template.alpha.openshift.io/wait-for-ready: "true"]
  ...output omitted...

   

  7.6. 在 Web 浏览器中打开路由 URL。​应用版本为 v1.1.1。​

  [http://exoplanets-declarative-manifests.apps.ocp4.example.com/](http://exoplanets-declarative-manifests.apps.ocp4.example.com/)

  ![[My-Case_runing_018_Section 1.2_ 指导练习_ 资源清单_003.png]]

   

  8. 清理资源。​

  8.1. 删除应用资源。

  [[student@workstation declarative-manifests]$ ]oc delete -f .\
  configmap "database" deleted\
  secret "database" deleted\
  deployment.apps "database" deleted\
  service "database" deleted\
  configmap "exoplanets" deleted\
  secret "exoplanets" deleted\
  deployment.apps "exoplanets" deleted\
  service "exoplanets" deleted\
  route.route.openshift.io "exoplanets" deleted

   

  8.2 更改为学员主目录。​

  [[student@workstation declarative-manifests]$ ]cd[\
  [student@workstation ~]]

   

  完成

  在 workstation 计算机上，​使用 lab 命令来完成本练习。​此步骤很重要，​可确保前面练习中的资源不会影响后续练习。​

  [[student@workstation ~]$ ]lab finish declarative-manifests

   

   

  == done ==
