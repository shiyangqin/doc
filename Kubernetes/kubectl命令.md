# kubectl命令

## 查看帮助

```shell
kubectl --help
```

## 命令

```shell
kubectl create # 从文件或标准输入创建资源
例：kubectl create --filename=redis.yaml --record=true

kubectl expose # 将资源暴露为新的Kubernetes Service
例：kubectl expose rc nginx --port=80 --target-port=8000

kubectl run # 创建并运行一个或多个容器镜像。创建一个deployment 或job 来管理容器。
例：kubectl run nginx --image=nginx

kubectl set # 配置应用资源
例：kubectl set image deployment/nginx busybox=busybox nginx=nginx:1.9.1

kubectl explain #

kubectl get # 显示资源
例：kubectl get nodes

kubectl edit # 使用默认编辑器 编辑服务器上定义的资源。
例：kubectl edit deployment/mydeployment -o yaml --save-config

kubectl delete # 删除资源
例：kubectl delete deployment redis-deploy
```
