# kubectl命令

```shell
kubectl annotate  # 更新某个资源的注解

kubectl api-versions  # 以“组/版本”的格式输出服务端支持的API版本

kubectl apply  # 通过文件名或控制台输入，对资源进行配置

kubectl attach  # 连接到一个正在运行的容器

kubectl autoscale  # 自动伸缩Deployment, ReplicaSet, 或 ReplicationController

kubectl cluster-info  # 显示集群信息

kubectl config  # 配置修改kubeconfig文件

kubectl config current-context  # 显示 current-context

kubectl config set-cluster  # 在kubeconfig配置文件中设置一个集群项

kubectl config set-context  # 在kubeconfig配置文件中设置一个环境项。

kubectl config set-credentials  # 在kubeconfig配置文件中设置一个用户项

kubectl config set  # 在kubeconfig配置文件中设置一个单独的值

kubectl config unset  # 在kubeconfig配置文件中清除一个单独的值

kubectl config use-context  # 使用kubeconfig中的一个环境项作为当前配置。

kubectl config view  # 显示合并后的kubeconfig设置，或者一个指定的kubeconfig配置文件

kubectl convert  # 在不同的API版本之间转换配置文件

kubectl cordon  # 将节点标记为不可调度

kubectl create  # 通过文件名或标准输入创建资源

kubectl create configmap  # 从本地文件，目录或文字值创建configMap。

kubectl create namespace  # 创建具有指定名称的名称空间。

kubectl create secret docker-registry  # 创建一个用于Docker注册表的secret

kubectl create secret  # 使用指定的子命令创建一个secret

kubectl create secret generic  # 从本地文件，目录或文字值创建机密。

kubectl create serviceaccount  # 创建具有指定名称的服务帐户

kubectl delete  # 按文件名，标准输入，资源和名称，或按资源和标签选择器删除资源

kubectl describe  # 显示特定资源或资源组的详细信息

kubectl drain  # 清理节点资源，删除节点，准备维护

kubectl edit  # 编辑服务端的资源

kubectl exec  # 在容器中执行命令

kubectl explain  # 资源文件

kubectl expose  # 公开新的Kubernetes服务

kubectl get  # 显示一种或多种资源

kubectl label  # 更新资源上的标签

kubectl logs  # 打印pod中容器的日志

kubectl patch  # 更新Api对象

kubectl port-forward  # 将一个或多个本地端口转发到Pod

kubectl proxy  # 运行Kubernetes API服务器的代理

kubectl replace  # 用文件名或标准输入替换资源

kubectl rolling-update  # 对给定的ReplicationController执行滚动更新。

kubectl rollout  # 管理deployment

kubectl rollout history  # 查看历史

kubectl rollout pause  # 将提供的资源标记为暂停

kubectl rollout resume  # 恢复暂停的资源

kubectl rollout undo  # 撤消先前的部署

kubectl run  # 在集群上运行指定镜像

kubectl scale  # 设置Deployment, ReplicaSet, Replication Controller, or Job的大小

kubectl uncordon  # 将节点标记为可调度

kubectl version  # 输出服务端和客户端的版本信息
```
