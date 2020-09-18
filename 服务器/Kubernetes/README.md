# Kubernetes

Kubernetes官方文档：http://kubernetes.kansea.com/docs/

+ [使用kubeadm部署Kubernetes](使用kubeadm部署Kubernetes.md)

+ [Kubernetes对象的yaml文件](Kubernetes对象的yaml文件.md)

+ [kubectl命令](kubectl命令.md)

## Kubernetes是什么

Kubernetes 是一个用于容器集群的自动化部署、扩容以及运维的开源平台。

下面是一些关键点:

+ 灵活的创建和部署应用: 相对使用虚拟机镜像，容器镜像的创建更加轻巧高效。
+ 持续开发，持续集成以及持续部署: 提供频繁可靠地构建和部署容器镜像的能力，同时可以快速简单地回滚(因为镜像是固化的)。
+ 开发和运维的关注点分离: 提供构建和部署的分离；这样也就将应用从基础设施中解耦。
+ 开发，测试，生产环境保持高度一致: 无论是在笔记本电脑还是服务器上，都采用相同方式运行。
+ 兼容不同的云平台或操作系统上: 可运行于Ubuntu，RHEL，CoreOS，on-prem或者Google Container Engine，或者任何其他环境.
+ 以应用程序为中心的管理: 将抽象级别从在虚拟硬件上运行操作系统上升到了在使用特定逻辑资源的操作系统上运行应用程序。
+ 松耦合，分布式，弹性，自由的微服务: 应用被分割为若干独立的小型程序，可以被动态地部署和管理 – 而不是一个运行在单机上的超级臃肿的大程序。
+ 资源分离: 带来可预测的程序性能。
+ 资源利用: 高性能，大容量。

## 随记

先了解集群的搭建，master节点，node节点，节点间通讯

然后了解deployment、DaemonSet、pod、servie等概念

然后了解配置管理（configMap），持久化数据（PV），服务暴漏（NodePort？）等操作
