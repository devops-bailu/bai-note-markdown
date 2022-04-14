# Kubernetes 核心概念和资源对象

Kubernetes 中所有的配置都是通过 API 对象的 spec 去设置的，也就是用户通过配置系统的理想状态来改变系统，这是  Kubernetes  重要设计理念之一，即所有的操作都是声明式（Declarative）的而不是命令式（Imperative）的。声明式操作在分布式系统中的好处是稳定，不怕丢操作或运行多次，例如设置副本数为 3 的操作运行多次也还是一个结果，而给副本数加 1 的操作就不是声明式的，运行多次结果就错了。

## Pod

Pod 的设计理念是支持多个容器在一个 Pod 中共享网络地址和文件系统，可以通过进程间通信和文件共享这种简单高效的方式组合完成服务。Pod 对多容器的支持是 K8 最基础的设计理念。

- Pod 是 k8s 中的最小调度单位

- 1个 pod 中可以运行多个 container（容器），他们共享 UTS + NETWORK + IPC 命名空间

## Pod 控制器

Pod 控制器是 Pod 启动的一种模板，用来保证 k8s 中启动的 pod 应该按照预期运行（副本数、生命周期、健康状态检查等）

常见的 Pod 控制器如下：

- Deployment
- DaemonSet
- ReplicaSet
- StatefulSet
- Job
- CronJob



