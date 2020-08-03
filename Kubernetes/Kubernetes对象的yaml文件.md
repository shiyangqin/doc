# Kubernetes对象的yaml文件

+ [Deployment](#Deployment)
+ [Service](#Service)
+ [ConfigMap](#ConfigMap)

## Deployment

```yaml
apiVersion: apps/v1                 # 指定api版本
kind: Deployment                    # 指定创建资源的类型为Deployment
metadata:                           # Deployment的元数据
  name: redis-deploy                # 名称
  labels:                           # 标签
    label-1: redis-deploy
  annotations:                      # 注解
    annotations-1: redis-annotations
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
          volumeMounts:             # 数据卷
            - mountPath: /data      # 容器路径
              name: redis-data      # volumes name
      volumes:                      # volumes
        - name: redis-data
          emptyDir: {}
```

注：

1. pod模板和创建Pod的yaml文件相同，只是不需要再写 apiVersion、kind、metadata.name
2. Always：默认值，当容器失效时，由kubelet自动重启该容器；Never：不重新启动；OnFailure：当容器终止运行且退出码不为0时，由kubelet自动重启该容器

## Service

```yaml
apiVersion: v1                      # 指定api版本
kind: Service                       # 指定创建资源的类型为Service
metadata:                           # Service的元数据
  name: service-name                # 名称
  namespace: default                # 命名空间,默认为default
  labels:                           # 标签
    labels-1: service-label
  annotations:                      # 注解
    annotations-1: service-annotations
spec:                               # Service详细定义
  selector:                         # 选择器
    label-1: redis-pod
  type: ClusterIP                   # 类型，默认ClusterIP
  ports:                            # 暴漏端口列表
    - name: redis                   # 名称
      protocol: TCP                 # 端口协议，支持TCP和UDP，默认TCP
      port: 5432                    # 服务器监听端口
      targetPort: 5432              # 需要转发到pod的端口
    - name: http                    # 名称
      protocol: TCP                 # 端口协议，支持TCP和UDP，默认TCP
      port: 80                      # 服务器监听端口
      targetPort: 9000              # 需要转发到pod的端口
```

## ConfigMap

有些配置文件特别长，所以对于ConfigMap，我的建议时从文件导入创建

```shell
kubectl create configmap web --from-file=sap.ini -o yaml
```

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  creationTimestamp: "2020-07-28T01:58:40Z"
  managedFields:
  - apiVersion: v1
    fieldsType: FieldsV1
    fieldsV1:
      f:data:
        .: {}
        f:sap.ini: {}
    manager: kubectl
    operation: Update
    time: "2020-07-28T01:58:40Z"
  name: web
  namespace: default
  resourceVersion: "1823372"
  selfLink: /api/v1/namespaces/default/configmaps/web
  uid: 531d949d-9e09-4abd-9685-4d16cecdfc8e
data:
  sap.ini: |
    [LOG]
    file_name = flask.log
    level = NOSET

    [PG]
    host = pg-server
    port = 5432
    user = postgres
    pwd = postgres
    name = flask_data

    [REDIS]
    host = redis-server
    port = 6379
    pwd = redis
```

## 测试

flask.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-server
  labels:
    server: flask-deploy
spec:
  selector:
    matchLabels:
      server: flask-pod
  template:
    metadata:
      labels:
        server: flask-pod
    spec:
      containers:
        - name: flask-server
          image: flask-server:1.0
          ports:
            - containerPort: 80
          volumeMounts:
            - name: config
              mountPath: /opt/FlaskTest/sap.ini
              subPath: opt/FlaskTest/sap.ini
      volumes:
        - name: config
          configMap:
            name: web
            items:
              - key: sap.ini
                path: opt/FlaskTest/sap.ini
      nodeSelector:
        server: flask
---
apiVersion: v1
kind: Service
metadata:
  name: flask-server
  labels:
    server: flask-ser
spec:
  type: NodePort
  clusterIP: 10.20.1.1
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30000
  selector:
    server: flask-pod
```

pg.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: pg-server
  labels:
    server: pg-deploy
spec:
  selector:
    matchLabels:
      server: pg-pod
  template:
    metadata:
      labels:
        server: pg-pod
    spec:
      containers:
        - name: pg-server
          image: postgres:12.2
          env:
            - name: POSTGRES_USER
              value: postgres
            - name: POSTGRES_PASSWORD
              value: postgres
            - name: POSTGRES_DB
              value: flask_data
          ports:
            - containerPort: 5432
          volumeMounts:
            - name: pg-data
              mountPath: /var/lib/postgresql/data
      volumes:
        - name: pg-data
          hostPath:
            path: /opt/k8s/data/pg
      nodeSelector:
        server: pg
---
apiVersion: v1
kind: Service
metadata:
  name: pg-server
  labels:
    server: pg-ser
spec:
  clusterIP: 10.20.1.2
  ports:
  - port: 5432
    targetPort: 5432
  selector:
    server: pg-pod
```

redis.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-server
  labels:
    server: redis-deploy
spec:
  selector:
    matchLabels:
      server: redis-pod
  template:
    metadata:
      labels:
        server: redis-pod
    spec:
      containers:
        - name: redis-server
          image: redis:6.0
          command:
            - redis-server
            - --requirepass
            - redis
          ports:
            - containerPort: 6379
          volumeMounts:
            - name: redis-data
              mountPath: /data
      volumes:
        - name: redis-data
          hostPath:
            path: /opt/k8s/data/redis
      nodeSelector:
        server: redis
---
apiVersion: v1
kind: Service
metadata:
  name: redis-server
  labels:
    server: redis-ser
spec:
  clusterIP: 10.20.1.3
  ports:
  - port: 6379
    targetPort: 6379
  selector:
    server: redis-pod
```

```shell
kubectl delete deployment flask-server
kubectl delete deployment pg-server
kubectl delete deployment redis-server

kubectl delete service flask-server
kubectl delete service pg-server
kubectl delete service redis-server

kubectl get all

kubectl create -f flask.yaml
kubectl create -f pg.yaml
kubectl create -f redis.yaml

rm -rf flask-server.yaml
vim flask-server.yaml

```
