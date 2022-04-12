# Docker基础

## Docker是什么

Docker官方网站：`https://www.docker.com/`

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com//cloudnactive-docker基础-202204121257038.png)

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com//cloudnactive-docker基础-202204121301887.png)

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com//cloudnactive-docker基础-202204121302635.png)

## Docker VS 虚拟机

任何一个对外提供的服务，都首先需要一个端口，然后需要一系列存储、计算、网络资源

###### 为什么使用虚拟机

- 物理机在使用过程中会出现资源浪费的情况，提升资源利用率
- 一台物理机可以分割为多个虚拟机，虚拟机之间具备很强的隔离性，具有独立的操作系统，可以避免端口占用等问题；以及可以避免多个服务因为一个服务的内存OOM而出现集体不可用的情况，爆炸半径比较大

###### Docker的优势和劣势：

- 轻量级，降低资源损耗
- 共享内核，隔离性和安全性存在一定缺陷，docker Engine 本质上来说是一个服务，可能会存在攻击通过pod和docker Engine 进而攻击到物理机

###### Docker和虚拟机的对比



|    特性    |      虚拟机      |       容器       |
| :--------: | :--------------: | :--------------: |
|  隔离级别  |    操作系统级    |      进程级      |
|  隔离策略  |    Hypervisor    | CGroup/Namespace |
|  系统资源  |      5-15%       |       0-5%       |
|  启动时间  |      分钟级      |       秒级       |
|  镜像大小  |      GB-TB       |      KB-MB       |
|  集群规模  |       上百       |       上万       |
| 高可用策略 | 备份、容灾、迁移 | 弹性、负载、动态 |



## Docker架构

- C/S 架构

- docker CLI
  - docker
- docker daemon
  - dockerd
- DOCKER_HOST支持 unix domain socket、TCP 和 SSH 进行集群切换操作



**目前大家使用到的基本都是 docker CLI，docker daemon承载了很多的功能，像是拉取镜像或者启动容器等**

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com//cloudnactive-docker基础-202204121456907.png)

 ## Docker镜像和容器

docker开源时候的 slogen 就是`Once building run anywhere`，所以 docker image 可以看作是标准化交付单元，解决了锅王常说的我本地可以运行呀，你看看你的环境这种问题。。。

#### 容器和容器镜像是什么

容器镜像就是一系列文件和配置的组成，打包而成；容器镜像描述了容器运行的基础环境

容器就是容器镜像的实例化单元，可以通过镜像创建出来容器

- 标准化交付单元：由开始的交付rpm包、jar包、二进制文件，到交付image
- DockerHub：最大的镜像存储仓库
- 容器只读层：就是从docker容器镜像中拿来的
- 容器可写层：就是在每一次实例化一个容器的时候，增加一个可写层，可以在上面对资源文件进行调整和修改，但是如果不提交相关的变更，这些修改就是临时的，随着容器的重启和消亡就没有了

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com//cloudnactive-docker基础-202204121542230.png)

容器镜像的构建是通过dockerfile来完成的，到目前为止，dockerfile已经成为了容器镜像构建的标准；dockerfile中每次执行的命令，对于容器镜像来说就是一层（layer），附加到基础镜像之上

###### 如何构建操作系统镜像

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com/cloudnactive-docker基础-20220413001549.png)

#### 容器镜像的分层

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com//cloudnactive-docker基础-202204121617893.png)

- 通过不断操作进行叠加
- 不是所有的操作都会增加镜像的体积

#### Docker镜像分层技术

- 内容寻址技术（CAS）

  在使用docker镜像的tag是不可靠的，只是一种代称，真正重要的还是docker镜像中的HASH值

- sha256 散列

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com//cloudnactive-docker基础-202204121708990.png)

容器镜像就是通过一个配置文件，通过每一层的哈希值将每一层组织起来的，哈希值分别对应到了不同的layer，哈希值的产生都使用过sha256进行散列，以上是可以通过保存容器镜像后，解压，然后进行查看

#### Docker容器

利用 Linux 所提供的能力来完成进程的组织，本质上，每一个容器就是一个进程

- cgroup

​		主要是控制资源的消耗，或者限制可用资源大小

- namespace

​		主要是进行进程资源隔离，主要有以下几种 

| namespace名称 |  系统调用参数   |                             含义                             |
| :-----------: | :-------------: | :----------------------------------------------------------: |
|     Mount     |   CLONE_NEWNS   |                          隔离挂载点                          |
|      PID      |  CLONE_NEWPID   |                           进程编号                           |
|    Network    |  CLONE_NEWNET   |                    网络设备、堆栈、端口等                    |
|      IPC      |  CLONE_NEWIPC   |                    系统IPC、POSIX消息队列                    |
|      UTS      |  CLONE_NEWUTS   | 系统主机名和NIS（Network Information Service）和主机名（有时候称为域名） |
|     User      |  CLONE_NEWUSER  |                          用户和组ID                          |
|    Cgroup     | CLONE_NEWCGROUP |                         Cgroup根目录                         |



## 容器化技术和工具

#### 容器化技术

- chroot
- Linux vserver / openVZ
- namespace / cgroup

#### 容器化工具

![](https://bai-images-1258524516.cos.ap-beijing.myqcloud.com/cloudnactive-docker基础-20220413003541.png)
