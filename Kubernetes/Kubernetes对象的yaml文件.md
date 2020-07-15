# Kubernetes对象的yaml文件

+ [Deployment](#Deployment)
+ [Service](#Service)

## Deployment

```yaml
apiVersion: apps/v1                 # 指定api版本
kind: Deployment                    # 指定创建资源的类型为Deployment
metadata:                           # Deployment的元数据
  name: redis-deploy                # 名称
  labels:                           # 标签
    label-1: redis-deploy
spec:                               # Deployment详细定义
  replicas: 2                       # 运行2个pod
  selector:                         # 选择器
    matchLabels:                    # 匹配标签
      label-1: redis-pod
  template:                         # pod模板，注：1
    metadata:                       # pod元数据
      labels:                       # pod标签
        label-1: redis-pod
    spec:                           # pod详细定义
      restartPolicy: Always         # pod重启策略，注：2
      containers:                   # 容器
        - name: redis-pod           # 容器名称
          image: redis:latest       # 容器镜像
          ports:                    # 容器暴露端口
            - containerPort: 5432   # 容器端口
```

注：
1. pod模板和创建Pod的yaml文件相同，只是不需要再写apiVersion和kind，因为会创建多个pod，所以metadata.name字段会自动生成，无法自定义
2. Always：默认值，当容器失效时，由kubelet自动重启该容器；Never：不重新启动；OnFailure：当容器终止运行且退出码不为0时，由kubelet自动重启该容器

## Service

```yaml
apiVersion: v1
kind: Service

```
