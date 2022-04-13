# Kubernetes 架构

kubernetes官方网站：`https://kubernetes.io/`

<font color="#00dddd">**传统部署时代：**</font>  

早期，各个组织机构在物理服务器上运行应用程序。无法为物理服务器中的应用程序定义资源边界，这会导致资源分配问题。 例如，如果在物理服务器上运行多个应用程序，则可能会出现一个应用程序占用大部分资源的情况， 结果可能导致其他应用程序的性能下降。 一种解决方案是在不同的物理服务器上运行每个应用程序，但是由于资源利用不足而无法扩展， 并且维护许多物理服务器的成本很高

<font color="#00dddd">**虚拟化部署时代：**</font>  

作为解决方案，引入了虚拟化。虚拟化技术允许你在单个物理服务器的 CPU 上运行多个虚拟机（VM）。 虚拟化允许应用程序在 VM 之间隔离，并提供一定程度的安全，因为一个应用程序的信息 不能被另一应用程序随意访问

虚拟化技术能够更好地利用物理服务器上的资源，并且因为可轻松地添加或更新应用程序 而可以实现更好的可伸缩性，降低硬件成本等等

每个 VM 是一台完整的计算机，在虚拟化硬件之上运行所有组件，包括其自己的操作系统。

<font color="#00dddd">**容器部署时代：**</font>  

容器类似于 VM，但是它们具有被放宽的隔离属性，可以在应用程序之间共享操作系统（OS）。 因此，容器被认为是轻量级的。容器与 VM 类似，具有自己的文件系统、CPU、内存、进程空间等。 由于它们与基础架构分离，因此可以跨云和 OS 发行版本进行移植

容器因具有许多优势而变得流行起来。下面列出的是容器的一些好处：

- 敏捷应用程序的创建和部署：与使用 VM 镜像相比，提高了容器镜像创建的简便性和效率
- 持续开发、集成和部署：通过快速简单的回滚（由于镜像不可变性），支持可靠且频繁的 容器镜像构建和部署
- 关注开发与运维的分离：在构建/发布时而不是在部署时创建应用程序容器镜像， 从而将应用程序与基础架构分离
- 可观察性：不仅可以显示操作系统级别的信息和指标，还可以显示应用程序的运行状况和其他指标信号。
- 跨开发、测试和生产的环境一致性：在便携式计算机上与在云中相同地运行
- 跨云和操作系统发行版本的可移植性：可在 Ubuntu、RHEL、CoreOS、本地、 Google Kubernetes Engine 和其他任何地方运行
- 以应用程序为中心的管理：提高抽象级别，从在虚拟硬件上运行 OS 到使用逻辑资源在 OS 上运行应用程序
- 松散耦合、分布式、弹性、解放的微服务：应用程序被分解成较小的独立部分， 并且可以动态部署和管理 - 而不是在一台大型单机上整体运行
- 资源隔离：可预测的应用程序性能
- 资源利用：高效率和高密度

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com/cloudnactive-k8s/k8s-base/k8s-base-202204131730030.png)

## K8s 是什么

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com/cloudnactive-k8s/k8s-base/k8s-base-202204131725983.png)

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com/cloudnactive-k8s/k8s-base/k8s-base-202204131726564.png)

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com/cloudnactive-k8s/k8s-base/k8s-base-202204131727189.png)

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com/cloudnactive-k8s/k8s-base/k8s-base-202204131727064.png)

## K8s 可以做什么

容器是打包和运行应用程序的好方式。在生产环境中，你需要管理运行应用程序的容器，并确保不会停机，Kubernetes 为你提供了一个可弹性运行分布式系统的框架。 Kubernetes 会满足你的扩展要求、故障转移、部署模式等。 例如，Kubernetes 可以轻松管理系统的 Canary 部署

Kubernetes 为你提供：

- **服务发现和负载均衡**

  Kubernetes 可以使用 DNS 名称或自己的 IP 地址公开容器，如果进入容器的流量很大， Kubernetes 可以负载均衡并分配网络流量，从而使部署稳定

- **存储编排**

  Kubernetes 允许你自动挂载你选择的存储系统，例如本地存储、公共云提供商等

- **自动部署和回滚**

  你可以使用 Kubernetes 描述已部署容器的所需状态，它可以以受控的速率将实际状态 更改为期望状态。例如，你可以自动化 Kubernetes 来为你的部署创建新容器， 删除现有容器并将它们的所有资源用于新容器

- **自动完成装箱计算**

  Kubernetes 允许你指定每个容器所需 CPU 和内存（RAM）。 当容器指定了资源请求时，Kubernetes 可以做出更好的决策来管理容器的资源

- **自我修复**

  Kubernetes 重新启动失败的容器、替换容器、杀死不响应用户定义的 运行状况检查的容器，并且在准备好服务之前不将其通告给客户端

- **密钥与配置管理**

  Kubernetes 允许你存储和管理敏感信息，例如密码、OAuth 令牌和 ssh 密钥。 你可以在不重建容器镜像的情况下部署和更新密钥和应用程序配置，也无需在堆栈配置中暴露密钥

## 为什么需要 K8s

- 大规模多接点的容器调度
- 扩容/缩容
- 故障自愈
- 弹性
- 云原生技术趋势
- 一致性/不锁定性：CNCF推出了一个一致性校验，无论是托管的，还是自建的k8s集群都可以去进行一致性校验，具备比较多的判定规则，一旦通过判定，你在这个集群上部署的服务，都可以无缝的直接迁移到其他k8s集群上

## K8s 整体架构和工作原理

#### K8s 架构

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com/cloudnactive-k8s/k8s-base/k8s-base-20220413214447.png)

###### 1. APIServer（kube-apiserver ）

负责处理来自用户的请求，其主要作用就是对外提供RESTful接口，包括查看集群状态和资源状态的读请求，以及改变集群状态和资源状态的写请求等等，也是唯一一个与etcd通信的组件

###### 2. Controller（kube-controller-manager ：控制器： 管理容器，监控容器）

管理器运行了一系列的控制器进程，这些进程会按照用户的期望状态在后台不断地调节整个集群中的对象

当服务状态发生了改变，控制器就会发现改变并且向目标状态迁移

是一系列控制器的集合，通过apiserver监控整个集群还有资源的状态，并且确保集群处于预期的工作状态

- Node Controller：节点控制器

- Deployment Controller：pod控制器
- Service Controller：服务控制器
- Volume Controller：存储卷控制器
- Endpoint Controller：接入点控制器
- Garbage Controller：垃圾回收控制器
- Namespace Controller：名称空间控制器
- Job Controller：任务控制器
- Resource quta Controller：资源配额控制器

###### 3. Scheduler（kube-scheduler）

调度器其实为kubernetes中运行的Pod选择部署的Worker节点
它会根据用户的需要选择最能满足请求的节点来运行Pod，它会在每次需要调度Pod时执行
主要功能是接收调度pod到适合的节点上
预选策略( predict )
优选策略( priorities )

## K8s 中服务发现和应用









## K8s 中应用的生命周期









## K8s 中业务架构的设计

