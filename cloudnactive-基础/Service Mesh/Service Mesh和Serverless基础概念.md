# Service Mesh和Serverless基础概念

## Service Mesh



### Service Mesh 的定义

Buoyant 的 CEO William Morgan，也就是Service Mesh这个词的发明人，对Service Mesh的定义：

服务网格是一个`基础设施层`， 用于处理服务间通信。云原生应用有着复杂的服务拓扑，服务网格保证 `请求在这些拓扑中可靠地穿梭`。在实际应用当中，服务网格通常是由一系列轻量级的 `网络代理` 组成的，它们与应用程序部署在一起，但 `对应用程序透明`。

## Service Mesh和Serverless

 Service Mesh 和 Serverless 是两个不同的维度的技术：

- Service Mesh 技术的关注点是在于服务间通讯，目的是脱离客户端 SDK ，以 Proxy 独立进程运行，减轻应用负担，帮助应用云原生化，提供的能力主要包括安全性、路由、策略执行、流量管理等等
- Serverless 技术的关注点在于服务运维，目标是客户无需关注服务运维，提供服务实例的自动伸缩，以及按照实际使用付费等等

但是目前开始出现了 Service Mesh 和 Serverless 两个技术的融合，未来可能出现一种新型的服务模式， Service Mesh 和 Serverless 合二为一，部署服务以后，就可以同时得到 Service Mesh 的服务间通讯能力和 Serverless 的无服务器运维，这可能是未来云原生应用的标准形态之一，但是目前还没有标准的落地形式。

## 











