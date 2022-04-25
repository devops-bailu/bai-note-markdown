# Kubernetes 调度器

在 Kubernetes 中，调度是指将 Pod 放置到合适的 Node 上，然后对应 Node 上的 Kubelet 才能够运行这些 pod

调度器通过 kubernetes 的监测（Watch）机制来发现集群中新创建且尚未被调度到 Node 上的 Pod。 调度器会将发现的每一个未调度的 Pod 调度到一个合适的 Node 上来运行。 调度器会依据下文的调度原则来做出调度选择

## Kubernetes 调度流程介绍

Kubernetes Scheduler 的作用是将待调度的 Pod 按照一定的调度算法和策略绑定到集群中一个合适的 Worker Node  上，并将绑定信息写入到 etcd 中，之后目标 Node 中 kubelet 服务通过 API Server 监听到 Scheduler 产生的 Pod 绑定事件获取 Pod 信息，然后下载镜像启动容器

Scheduler 提供的调度流程分为预选（Predicates）和优选（Priorities）两个步骤：

- 预选，K8S会遍历当前集群中的所有 Node，筛选出其中符合要求的 Node 作为候选
- 优选，K8S将对候选的 Node 进行打分

经过预选筛选和优选打分之后，K8S选择分数最高的 Node 来运行 Pod，如果最终有多个 Node 的分数最高，那么 Scheduler 将从当中随机选择一个 Node 来运行 Pod

## Kubernetes Scheduler 工作原理

#### Kubernetes 中 pod 创建的流程

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com/cloudnactive-k8s/k8s-base/k8s-base-202204241735425.png)

1. 用户发起创建请求
2. api-server 通过 kubeconfig 进行认证，然后将创建信息写入到 etcd 中
3. Controller-Manager 通过 api-server 发现了有 pod 信息更新，将该资源的拓扑结构进行整合，整合后将信息通过 api-server 写入到 etcd 中
4. Scheduler 同样通过 api-server 的 watch 接口获取到 pod 的更新信息，需要被调度，通过一系列的调度策略给 pod 分配合适的运行节点，并且将 pod 和对应调度的节点的绑定信息交给 api-server，然后 api-server 写入到 etcd 中
5. 对应节点的 kubelet 从 api-server 获取需要创建的 pod 信息，调用 CNI 接口创建 pod 网络，调用 CRI 接口去启动容器，调用 CSI 进行存储卷的挂载
6. 以上全部完成以后，pod 创建成功，容器内业务进行正常启动以后，pod 运行成功





