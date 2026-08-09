====== 1 =====

目标     | 从针对不同目标环境参数化的资源清单部署和更新应用。​

====== 2 =====

  | 培训目标 | - - 从存储为 YAML 文件的资源清单部署和更新应用。​                                                                                              |
  |                                      |   - 从 Kustomize 增强的资源清单部署和更新应用。​                                                                                              |

====== 3 =====

章节     | - - 资源清单 (及引导式练习)
|   - Kustomize 覆盖 (及引导式练习)

====== 4 =====

  | 实验     | - - 声明式资源管理                                                                                              |

====== 5 =====

  :::

  资源清单

  培训目标

  - 从存储为 YAML 文件的资源清单部署和更新应用。​

  Kubernetes 集群中的应用通常由协同工作的多个资源组成。​每种资源都有一个定义和一个配置。​许多资源配置共享共同的属性，​必须经过匹配才能正常运行。​命令式命令会配置每个资源，​一次一个。​但是，​使用命令式命令存在一些问题：

  - 可再现性受损
  - 缺乏版本控制
  - 缺乏对 GitOps 的支持

  与命令式命令相比，​声明式命令是通过使用资源清单来管理资源的首选方式。​资源清单是 JSON 或 YAML 格式的文件，​内含资源定义和配置信息。​资源清单通过将应用的所有属性封装在一个文件或一组相关文件中，​简化了 Kubernetes 资源的管理。​Kubernetes 使用声明式命令来读取资源清单，​并将更改应用到集群，​以满足资源清单定义的状态。​

  资源清单采用 YAML 或 JSON 格式，​因此可以进行版本控制。​资源清单的版本控制可以跟踪配置更改。​因此，​不利的更改可以回滚到较早的版本以支持可恢复性。​

  资源清单可确保精确再现应用，​通常通过一个命令来部署许多资源。​资源清单的可再现性为持续集成和持续交付（CI/CD）的 GitOps 实践的自动化提供了支持。​

  命令式工作流与声明式工作流

  Kubernetes CLI 使用命令式和声明式命令。​命令式命令执行基于命令的操作，​并且使用与操作密切相关的命令名称。​相比之下，​声明式命令使用资源清单文件来声明资源的预期状态。​

  Kubernetes 清单是一种 YAML 或 JSON 格式的文件，​包含 Kubernetes 资源（如部署、​容器集或服务）的声明语句。​清单文件不使用命令式命令来创建 Kubernetes 资源，​而是在单个文件中提供资源的所有详细信息。​使用清单文件可以使用可再现性更高的流程。​清单文件包含资源的完整定义，​并且可以在一个步骤中应用，​而不是再现命令式命令序列。​使用清单文件也可用于跟踪源代码管理系统中的系统配置更改。​

  在给定新的或更新的资源清单的情况下，​Kubernetes 提供的命令可将资源清单中指定的预期状态与资源的当前状态进行比较。​然后，​这些命令将转换应用到当前状态，​以匹配预期的状态。​

  命令式工作流

  命令式工作流可用于开发和测试。​下例使用 kubectl create deployment 命令式命令，​为 MYSQL 数据库创建部署。​

  [[user@host ~]$ ]kubectl create deployment db-pod --port 3306 \\
  [  --image registry.ocp4.example.com:8443/rhel8/mysql-80]
  deployment.apps/db-pod created

  除了使用反映命令操作的动词外，​命令式命令还使用选项来提供详细信息。​示例命令使用 --port 和 --image 选项提供创建部署所需的详细信息。​

  使用命令式命令会影响将更改应用到实时资源。​例如，​上一部署中的容器集会因为缺少环境变量而无法启动。​下方 kubectl set env deployment 命令式命令通过将所需的环境变量添加到部署来解决问题：

  [[user@host ~]$ ]kubectl set env deployment/db-pod \\
  [  MYSQL_USER='user1' \\
    MYSQL_PASSWORD='mypa55w0rd' \\
    MYSQL_DATABASE='items']
  deployment.apps/db-pod updated

  执行此 kubectl set env deployment 命令会更改名为 db-pod 的部署资源，​并且提供额外需要的变量来启动容器。​开发人员可以通过使用命令式命令添加组件（如服务、​路由、​卷挂载和持久卷声明）来继续构建应用。​添加每个组件后，​开发人员可以运行测试以确保组件正确执行预期的功能。​

  命令式命令可用于开发和试验。​借助命令式命令，​开发人员可以按组件逐一构建应用。​添加组件时，​Kubernetes 集群会提供特定于该组件的错误消息。​该过程类似于使用调试器逐行执行代码。​使用命令式命令通常会提供更清楚的错误消息，​因为添加特定组件后会发生错误。​

  但是，​较长的命令行和零散的应用部署并不适合在生产中部署应用。​对于命令式命令，​更改是一系列命令，​必须进行维护以反映资源的预期状态。​命令序列必须得到跟踪并保持最新。​

  使用声明式命令

  清单文件不跟踪命令序列，​而是捕获序列的预期状态。​与使用命令式命令相比，​声明式命令使用一个或一组清单文件，​将用于创建这些组件的所有详细信息组合到 YAML 文件中，​这些文件可以在单个命令中应用。​将来对清单文件的更改只需重新应用清单即可。​版本控制系统可以跟踪清单文件的更改，​而不是跟踪一系列复杂的命令。​

  虽然清单文件也可以使用 JSON 语法，​但 YAML 通常是首选且更受欢迎。​继续以调试为例，​调试从清单部署的应用类似于尝试调试完整的、​已运行的应用。​查找错误来源可能需要花费更多的努力，​尤其是在错误不是由清单错误引起的情况下。​

  创建 Kubernetes 清单

  从头开始创建清单文件可能需要一些时间。​您可以使用以下技术为清单文件提供起点：

  - 从 Web 控制台使用资源的 YAML 视图。​
  - 使用带有 --dry-run=client 选项的命令式命令生成与命令式命令对应的清单。​

  kubectl explain 命令提供清单中任何字段的详细信息。​例如，​使用 kubectl explain deployment.spec.template.spec 命令来查看指定部署清单内容器集对象的字段描述。​

  如需创建入门部署清单，​请使用 kubectl create deployment 命令通过 --dry-run=client 选项生成清单：

  [[user@host ~]$ ]kubectl create deployment hello-openshift -o yaml \\
  [  --image registry.ocp4.example.com:8443/redhattraining/hello-world-nginx:v1.0 \\
    --save-config \ ][      (1)]

  --dry-run=client \ [   (2)]

  \> ~/my-app/example-deployment.yaml

  (1)[  --save-config ]选项添加声明式命令使用的配置属性。​对于 deployments 资源，​此选项将资源配置保存在 kubectl.kubernetes.io/last-applied-configuration 注释中。​

  (2)[  --dry-run=client ]选项可防止命令在集群中创建资源。​

  下例演示了 hello-openshift 部署的最小部署清单文件，​非生产就绪型：

  apiVersion: apps/v1
  kind: Deployment\
  metadata:\
  [  annotations:\
        ]...output omitted...\
  [  creationTimestamp: null\
    labels:\
      app: hello-openshift\
    name: hello-openshift\
  spec:\
    replicas: 1
    selector:\
      matchLabels:\
        app: hello-openshift\
    strategy: \
    template:\
      metadata:\
        creationTimestamp: null\
        labels:\
          app: hello-openshift\
      spec:\
        containers:\
        - image: quay.io/redhattraining/hello-world-nginx:v1.0
          name: hello-world-nginx\
          resources: \
  status: ]

  使用命令式命令创建清单时，​生成的清单可能包含创建资源所不需要的字段。​例如，​下例通过删除空字段和 null 字段来更改清单。​删除不必要的字段可以显着缩短清单长度，​进而减少处理它们的开销。​

  此外，​您可能需要进一步自定义清单。​例如，​在部署中，​您可以自定义副本数量，​或声明部署提供的端口。​下方注释说明了其他更改：

  apiVersion: apps/v1
  kind: Deployment\
  metadata:\
  [  namespace: ]resource-manifests [  (1)]

  labels:\
  [    app: hello-openshift\
    name: hello-openshift\
  spec:\
    ]replicas: 2 [     (2)]

  selector:\
  [    matchLabels:\
        app: hello-openshift\
    template:\
      metadata:\
        labels:\
          app: hello-openshift\
      spec:\
        containers:\
        - image: quay.io/redhattraining/hello-world-nginx:v1.0
          name: hello-world-nginx\
          ports:\
          - ]containerPort: 8080 [    (3)]

  protocol: TCP

  （1） 添加命名空间属性，​以防止部署到错误的项目。​

  （2） 需要两个副本，​而不是一个。​

  （3） 指定供服务使用的容器端口。​

  您可以为所管理的每个资源创建清单文件。​或者，​将每个清单添加到单个多部分 YAML 文件中，​然后使用 --- 行分隔清单。​

  ---\
  apiVersion: apps/v1
  kind: Deployment\
  metadata:\
  [  namespace: resource-manifests\
    annotations:\
    ]...output omitted...\
  ---\
  apiVersion: v1
  kind: Service\
  metadata:\
  [  namespace: resource-manifests\
    labels:\
      app: hello-openshift\
    name: hello-openshift\
  spec:\
    ]...output omitted...

  使用具有多个清单的单个文件与使用在多个清单文件中定义的清单是企业性偏好。​单个文件方法的优点是可以将相关清单保存在一起。​使用单个文件方法时，​可以更加方便地更改必须在多个清单中反映出来的资源。​相比之下，​将清单保存在多个文件中可以更方便地与他人共享资源定义。​

  创建清单后，​您可以在非生产环境中测试清单，​或继续部署清单。​在生产环境中部署应用之前，​请验证资源清单。​

  声明式工作流

  声明式命令使用资源清单，​而不是在命令行上的许多选项中添加详细信息。​如需创建资源，​请使用 kubectl create -f resource.yaml 命令。​您可以将目录而不是文件名传递给命令，​以处理目录中的所有资源文件。​添加 --recursive=true 或 -R 选项，​以递归方式处理多个子目录中提供的资源文件。​

  下例从 my-app 目录中的清单创建资源。​在本例中，​my-app 目录包含先前的 example-deployment.yaml 和 service/example-service.yaml 文件。​

  [[user@host ~]$ ]tree my-app\
  my-app\
  ├── example_deployment.yaml\
  └── service\
  [    └── example_service.yaml]

  [[user@host ~]$ ]kubectl create -R -f ~/my-app\
  deployment.apps/hello-openshift created\
  service/hello-openshift created

  命令也接受 URL：

  [[user@host ~]$ ]kubectl create -f \\
  [  ][https://example.com/example-apps/deployment.yaml](https://example.com/example-apps/deployment.yaml)
  deployment.apps/hello-openshift created

  更新资源

  kubectl apply 命令也可以使用与 kubectl create 命令所示相同的 -f 选项来创建资源。​但是，​kubectl apply 命令也可以更新资源。​

  更新资源比创建资源更复杂。​kubectl apply 命令实施了多种技术来应用更新且不会导致问题。​

  kubectl apply 命令将配置文件的内容写入到 kubectl.kubernetes.io/last-applied-configuration 注释。​kubectl create 命令也可以通过使用 --save-config 选项生成此注释。​kubectl apply 命令使用 last-applied-configuration 注释来标识已从配置文件中删除并且必须从实时配置中清除的字段。​

  虽然 kubectl create -f 命令可以从清单创建资源，​但命令是命令式的，​因此不考虑活动资源的当前状态。​对实时资源的清单执行 kubectl create -f 会出错。​相比之下，​kubectl apply -f 命令是声明式的，​会考虑集群中当前资源状态与清单中表示的预期资源状态之间的差异。​

  例如，​如需将容器的镜像从版本 v1.0 更新为 latest，​首先要更新 YAML 资源清单，​以指定镜像上的新标签。​然后，​使用 kubectl apply 命令指示 Kubernetes 使用清单中指定的更新后镜像版本来创建部署资源的版本。​

  YAML 验证

  在将更改应用到资源之前，​请使用 --dry-run=server 和 --validate=true 标志来检查文件是否有错误。​

  - --dry-run=server 选项提交服务器端请求，​但不持久保留资源。​
  - --validate=true 选项使用模式来验证输入，​如果输入无效，​则请求失败。​

  YAML 中的任何语法错误都包含在输出中。​最重要的是，​--dry-run=server 选项可防止将任何更改应用到 Kubernetes 运行时。​

  [[user@host ~]$ ]kubectl apply -f ~/my-app/example-deployment.yaml \\
  [  --dry-run=server --validate=true]
  deployment.apps/hello-openshift created (server dry-run) [    (1)]

  (1)[  ]以 (server dry-run) 结尾的输出行提供了应用资源文件将执行的操作。​

  注意:

  --dry-run=client 选项仅显示要发送到服务器的对象。​即使语法为有效的 YAML，​集群资源控制器也可以拒绝清单。​相比之下，​--dry-run=server 选项将请求发送到服务器，​以确认清单符合当前的服务器策略，​而不在服务器上创建资源。​

  比较资源

  使用 kubectl diff 命令检查活动对象和清单之间的差异。​更新资源清单时，​您可以跟踪更改的文件之间的差异。​但是，​许多清单更改在应用时不会改变集群资源的状态。​基于文本的 diff 工具将显示所有此类差异，​并导致嘈杂的输出。​

  相比之下，​使用 kubectl diff 命令可能会更方便地预览更改。​kubectl diff 命令强调了 Kubernetes 集群的重大变化。​检查差异，​以验证清单更改是否具有预期效果。​

  [[user@host ~]$ ]kubectl diff -f example-deployment.yaml\
  ...output omitted...\
  diff -u -N /tmp/LIVE-2647853521/apps.v1.Deployment.resource...\
  --- /tmp/LIVE-2647853521/apps.v1.Deployment.resource-manife...\
  +++ /tmp/MERGED-2640652736/apps.v1.Deployment.resource-mani...\
  @@ -6,7 +6,7 @@\
  [     kubectl.kubernetes.io/last-applied-configuration: |\
         ]...output omitted...\
  [   creationTimestamp: "2023-04-27T16:07:47Z"\
  -  generation: 1 ][     (1)]

  +[  ]generation: 2
  [   labels:\
       app: hello-openshift\
     name: hello-openshift\
  @@ -32,7 +32,7 @@\
           app: hello-openshift\
       spec:\
         containers:\
  -      - image: registry.ocp4.example.com:8443/.../hello-world-nginx:v1.0 ][   (2)]

  +[      - ]image: registry.ocp4.example.com:8443/.../hello-world-nginx:latest\
  [         imagePullPolicy: IfNotPresent\
           name: hello-openshift\
           ports:]

  (1) 以 - 字符开头的行显示当前部署是第 1 代。​以下以 + 字符开头的行显示了应用清单文件后，​generation 将更改为 2。​

  (2) 以 - 字符开头的镜像行显示当前镜像使用的是 v1.0 版本。​以下以 + 字符开头的行显示了应用清单文件后，​版本将更改为 latest。​

  Kubernetes 资源控制器自动向实时资源添加注释和属性，​报告许多不影响资源配置的差异，​从而使其他基于文本的差异工具的输出具有误导性。​从实时资源中提取清单，​并使用 diff 命令等工具进行比较，​这会报告许多毫无价值的差异。​使用 kubectl diff 命令可确认实时资源与清单提供的资源配置匹配。​GitOps 工具依靠 kubectl diff 命令来确定是否有人更改了 GitOps 工作流以外的资源。​由于工具本身无法了解任何控制器如何更改资源的所有详细信息，​因此工具将按照集群来确定更改是否有意义。​

  更新注意事项

  使用 oc diff 命令时，​识别出在应用清单更改时不会生成新的容器集。​例如，​如果更新的清单仅更改了机密或配置映射中的值，​则应用更新的清单不会生成使用这些值的新容器集。​由于容器集在启动时会读取机密和配置映射，​因此在这种情况下，​应用更新后的清单会使容器集处于易受攻击状态，​其陈旧值不会与更新后的机密或配置映射同步。​

  作为解决方案，​使用 oc rollout restart deployment deployment-name 命令强制重启与部署关联的容器集。​强制重启会生成容器集，​它们使用更新后的机密或配置映射中的新值。​

  在具有单个副本的部署中，​您也可以通过删除容器集来解决问题。​作为响应，​Kubernetes 会自动创建容器集来替换已删除的容器集。​但是，​对于多个副本，​最好使用 oc rollout 命令重新启动容器集，​因为容器集将以智能方式停止和更换，​从而最大限度地减少停机时间。​

  本课程涵盖了可以自动化或消除其中一些挑战的其他资源管理机制。​

  应用更改

  kubectl create 命令尝试在清单文件中创建指定的资源。​如果集群中已存在目标资源，​则使用 kubectl create 命令会生成错误。​相比之下，​kubectl apply 命令比较三个来源，​以确定如何处理请求并应用更改。​

  1. 清单文件

  2. 集群中资源的实时配置

  3. 存储在 last-applied-configuration 注释中的配置

  如果清单文件中指定的资源不存在，​则 kubectl apply 命令创建该资源。​如果实时资源的 last-applied-configuration 注释中的任何字段不在清单中，​则命令将从实时配置中删除这些字段。​将更改应用到实时资源后，​kubectl apply 命令会更新实时资源的 last-applied-configuration 注释以考虑更改。​

  创建资源时，​kubectl create 命令的 --save-config 选项会生成所需的注释，​供将来的 kubectl apply 命令操作。​

  为 Kubernetes 资源打补丁

  您可以使用 oc patch 命令以可重复的方式修改 OpenShift 中的对象。​oc patch 命令从提供的 JSON 或 YAML 代码段或文件中更新字段或添字段到现有的对象中。​软件开发人员可能会在完整更新可用之前分发修复问题的补丁文件或代码段。​

  若要通过代码段为对象打补丁，​请使用带 -p 选项的 oc patch 命令和代码段。​下例使用 JSON 代码段将 hello 部署更新为具有 100m 的 CPU 资源请求：

  [[user@host ~]$ ]oc patch deployment hello -p \\
  [    ]'{"spec":{"template":{"spec":{"containers":[{"name": \\
  [    ]["hello-rhel7","resources": {"requests": }}]}}}}']
  deployment/hello patched

  若要通过补丁文件为对象打补丁，​请使用带 --patch-file 选项的 oc patch 命令和补丁文件的位置。​下例将 hello 部署更新为包含 ~/volume-mount.yaml 补丁文件的内容：

  [[user@host ~]$ ]oc patch deployment hello --patch-file ~/volume-mount.yaml\
  deployment.apps/hello patched

  补丁文件的内容描述了将持久卷声明挂载为卷：

  spec:\
  [  template:\
      spec:\
        containers:\
          - name: hello\
            volumeMounts:\
              - name: www\
                mountPath: /usr/share/nginx/html/\
        volumes:\
          - name: www\
            persistentVolumeClaim:\
              claimName: nginx-www]

  此补丁会为 hello 部署生成以下清单：

  apiVersion: apps/v1
  kind: Deployment\
  metadata:\
  [  name: hello\
    ]...output omitted...\
  spec:\
  [  ]...output omitted...\
  [  template:\
      ]...output omitted...\
  [    spec:\
        containers:\
          ]...output omitted...\
  [        name: server\
          ]...output omitted...\
  [        volumeMounts:\
          - ]mountPath: /usr/share/nginx/html/\
  [          name: www]
  [        - mountPath: /etc/nginx/conf.d/\
            name: tls-conf\
        ]...output omitted...\
  [      volumes:\
        - configMap:\
            defaultMode: 420
            name: tls-conf\
          name: tls-conf\
        - ]persistentVolumeClaim:\
  [          claimName: nginx-www\
          name: www]
  ...output omitted...

  无论 www 卷挂载是否存在，​补丁都会应用于 hello 部署。​oc patch 命令修改补丁中指定的对象中的现有字段：

  apiVersion: apps/v1
  kind: Deployment\
  metadata:\
  [  name: hello\
    ]...output omitted...\
  spec:\
  [  ]...output omitted...\
  [  template:\
      ]...output omitted...\
  [    spec:\
        containers:\
          ]...output omitted...\
  [        name: server\
          ]...output omitted...\
  [        volumeMounts:\
          - mountPath: ]/usr/share/nginx/www/ [    (1)]

  name: www\
  [        - mountPath: /etc/nginx/conf.d/\
            name: tls-conf\
        ]...output omitted...\
  [      volumes:\
        - configMap:\
            defaultMode: 420
            name: tls-conf\
          name: tls-conf\
        - persistentVolumeClaim:\
            claimName: ]deprecated-www [  (2)]

  name: www\
  ...output omitted...

  (1)(2) www 卷已存在。​该补丁会将现有数据替换为新数据。​

  参考

  有关更多信息，​请参阅红帽 OpenShift 容器平台 4.14 CLI 工具文档中 OpenShift CLI（oc）章节的 OpenShift CLI 开发人员命令参考部分，​访问地址： [https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html-single/cli_tools/index#cli-developer-commands](https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html-single/cli_tools/index#cli-developer-commands)

  有关更多信息，​请参阅红帽 OpenShift 容器平台 4.14 构建应用文档中部署章节的使用部署策略部分，​访问地址： [https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html-single/building_applications/index#deployment-strategies](https://docs.redhat.com/en/documentation/openshift_container_platform/4.14/html-single/building_applications/index#deployment-strategies)

  有关 oc patch 命令的更多信息，​请参阅红帽 OpenShift 容器平台 4.14 CLI 工具文档中 OpenShift CLI 开发人员命令参考章节的 oc 补丁部分，​访问地址： [https://access.redhat.com/documentation/en-us/openshift_container_platform/4.14/html-single/cli_tools/index#oc-patch](https://access.redhat.com/documentation/en-us/openshift_container_platform/4.14/html-single/cli_tools/index#oc-patch)

  [Kubernetes 文档 - 副本集](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

  [Kubernetes 文档 - 部署策略](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#strategy)

  [Kubernetes 文档 - 部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
