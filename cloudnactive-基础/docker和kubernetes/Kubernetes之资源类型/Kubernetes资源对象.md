# Kubernetes 核心概念和资源对象

Kubernetes 中所有的配置都是通过 API 对象的 spec 去设置的，也就是用户通过配置系统的理想状态来改变系统，这是  Kubernetes  重要设计理念之一，即所有的操作都是声明式（Declarative）的而不是命令式（Imperative）的。声明式操作在分布式系统中的好处是稳定，不怕丢操作或运行多次，例如设置副本数为 3 的操作运行多次也还是一个结果，而给副本数加 1 的操作就不是声明式的，运行多次结果就错了

## Pod

Pod 的设计理念是支持多个容器在一个 Pod 中共享网络地址和文件系统，可以通过进程间通信和文件共享这种简单高效的方式组合完成服务。Pod 对多容器的支持是 K8 最基础的设计理念

- Pod 是 k8s 中的最小调度单位

- 1个 pod 中可以运行多个 container（容器），他们共享 UTS + NETWORK + IPC 命名空间

#### Pod 控制器

Pod 控制器是 Pod 启动的一种模板，用来保证 k8s 中启动的 pod 应该按照预期运行（副本数、生命周期、健康状态检查等等）

常见的 Pod 控制器如下：

- Deployment
- DaemonSet
- ReplicaSet
- StatefulSet
- Job
- Cronjob

## ReplicationController（不用）

用于确保每个 pod 副本在任一时刻均能满足目标数量，换言之，即它用于保证每个容器或容器组总是运行并可访问；它是上一代的无状态 Pod 应用控制器，建议读者使用新型控制器 Deployment 和 ReplicaSet 来取代它

## ReplicaSet（一般不用）

ReplicaSet 它是用来确保我们有指定数量的 Pod 副本正在运行的 Kubernetes 控制器，意在保证系统当前正在运行的 Pod 数等于期望状态里指定的 Pod 数目

一般来说，Kubernetes建议使用 Deployment 控制器而不是直接使 ReplicaSet，Deployment 是一个管理ReplicaSet 并提供 Pod 声明式更新、应用的版本管理以及许多其他功能的更高级的控制器。所以 Deployment 控制器不直接管理 Pod 对象，而是由 Deployment 管理 ReplicaSet，再由 ReplicaSet 负责管理 Pod 对象

## Deployment

用于管理无状态的持久化应用，例如 http 服务等；它用于为 Pod 和 ReplicaSet 提供声明式更新，是建构在ReplicaSet 之上的更为高级的控制器

- 定义一组 Pod 期望数量，Controller 会维持 Pod 数量与期望数量一致
- 配置 Pod 的发布方式，Controller 会按照给定的策略更新 Pod，保证更新过程中不可用 Pod 维持在限定数量范围内
- 动态扩容/缩容，回滚等

## StatefulSet

StatefulSet是为了解决有状态服务的问题（对应 Deployments 和 ReplicaSets 是为无状态服务而设计），其应用场景包括：

- 稳定的持久化存储，即 Pod 重新调度后还是能访问到相同的持久化数据，基于 PVC 来实现
- 稳定的网络标志，即 Pod 重新调度后其 PodName 和 HostName 不变，基于 Headless Service（即没有Cluster IP 的 Service ）来实现
- 有序部署，有序扩展，即 Pod 是有顺序的，在部署或者扩展的时候要依据定义的顺序依次依次进行（即从0到N-1，在下一个 Pod 运行之前所有之前的Pod必须都是 Running 和 Ready 状态），基于init containers来实现
- 有序收缩，有序删除（即从N-1到0）

从上面的应用场景可以发现，StatefulSet由以下几个部分组成：

- 用于定义网络标志（DNS domain）的Headless Service
- 用于创建 PersistentVolumes 的 volumeClaimTemplates
- 定义具体应用的 StatefulSet

StatefulSet中每个 Pod 的 DNS 格式为 statefulSetName-{0..N-1}.serviceName.namespace.svc.cluster.local，其中-

- serviceName 为 Headless Service 的名字
- 0..N-1为 Pod 所在的序号，从0开始到N-1
- statefulSetName 为 StatefulSet 的名字
- namespace 为服务所在的 namespace，Headless Servic 和 StatefulSet 必须在相同的 namespace
- .cluster.local 为 Cluster Domain

## DaemonSet

DaemonSet 的主要作用，是在 Kubernetes 集群里，运行一个 Daemon Pod。 DaemonSet 只管理 Pod 对象，然后通过 nodeAffinity 和 Toleration 这两个调度器的小功能，保证了每个节点上有且只有一个 Pod。
 所以，这个 Pod 有如下三个特征：

- 这个 Pod 运行在 Kubernetes 集群里的每一个节点（Node）上。
- 每个节点上只有一个这样的 Pod 实例。
- 当有新的节点加入 Kubernetes 集群后，该 Pod 会自动地在新节点上被创建出来。
- 当旧节点被删除后，它上面的 Pod 也相应地会被回收掉。

Daemon Pod 的意义确实是非常重要的，主要是用如下：

- 网络插件的 Agent 组件，都必须运行在每一个节点上，用来处理这个节点上的容器网络
- 存储插件的 Agent 组件，也必须运行在每一个节点上，用来在这个节点上挂载远程存储目录，操作容器的 Volume 目录
- 监控组件和日志组件，也必须运行在每一个节点上，负责这个节点上的监控信息和日志搜集

## Job

Job 一般用于数据处理、迁移等一次性任务处理场景，Job 会创建 Pod 进行作业并确保完成

## Cronjob

CronJob 则就是在Job上加上了时间调度

## Service









## ConfigMap 和 Secret

ConfigMap 和Secret 是 Kubernetes 系统上两种特殊类型的存储卷，ConfigMap 对象用于为容器中的应用提供配置文件等信息。但是比较敏感的数据，例如密钥、证书等由 Secret 对象来进行配置。它们将相应的配置信息保存于对象中，而后在 Pod 资源上以存储卷的形式挂载并获取相关的配置，以实现配置与镜像文件的解耦











## Persistent Volume





## Persistent Volume Claim











## Namespace

随着项目增多，使用人员增加，集群规模变大，需要对k8s集群使用一定的手段来隔离k8s中的各种资源；命名空间是一个逻辑的概念，类似于AZ的概念











