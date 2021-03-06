## TSF Mesh 介绍

TSF Service Mesh (以下简称为 TSF Mesh) 是一个基础设施层，用于处理服务间的通信。TSF Mesh 是由一系列轻量级的网络代理组成，这些代理（又称 sidecar），与应用程序部署在一起，而应用程序不感知 sidecar 的存在。

TSF Mesh 是处于 TCP / IP 之上的一个抽象层。TCP 解决了网络端点键字节传输问题，TSF Mesh 解决服务节点间请求的路由问题。



## TSF Mesh 的优势

TSF Mesh 具有如下优势：

- 多编程语言应用兼容；

- 业务代码零侵入，代码无须改造。

  

## 基本实现原理

TSF  Mesh 可以代理使用虚拟机或者容器部署的应用。下面以容器为例说明 TSF Service Mesh 的实现原理。Sidecar 是 L7 层代理，和服务运行在同一个 Pod 中，与 Pod 共享网络和存储，其中 Sidecar 与服务的关系如下：

- Sidecar 代理服务向注册中心注册服务相关信息，以便其他服务发现自身。
- Sidecar 作为 Pod 内服务的 HTTP 代理，可以自动发现其他服务。


## 	使用场景

TSF Mesh 主要有三种使用场景：

- 仅服务消费者作为 Mesh 应用部署；
- 仅服务提供者作为 Mesh 应用部署；
- 服务消费者和服务提供者均作为 Mesh 应用部署。



#### 场景1：仅服务消费者作为 Mesh 应用部署

- 服务提供者使用 TSF - Spring Cloud 框架实现，注册到服务注册中心；
- 服务消费者作为 Mesh 应用部署，由 Sidecar 注册到服务注册中心。
  ![](https://main.qcloudimg.com/raw/c1f43fc9a07c2961b97b8dbc15b4b347.png)


#### 场景2：仅服务提供者作为 Mesh 应用部署

- 服务提供者作为 Mesh 应用部署，由 Sidecar 注册到服务注册中心；
- 服务消费者使用 TSF - Spring Cloud 框架实现，注册到服务注册中心。
  ![](https://main.qcloudimg.com/raw/432b2ae025346fe3afb97cd585daded8.png)


#### 场景3：服务消费者和服务提供者均作为 Mesh 应用部署

- 服务提供者作为 Mesh 应用部署，由 Sidecar 注册到服务注册中心；
- 服务消费者作为 Mesh 应用部署，由 Sidecar 注册到服务注册中心。
  ![](https://main.qcloudimg.com/raw/0627256912a9d300d7cce74b353342e9.png)
