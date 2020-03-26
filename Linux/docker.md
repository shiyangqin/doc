+ [centos7安装docker](#centos7安装docker)
+ [docker命令](#docker命令)
    + [info|version](#info|version)
    + [镜像仓库](#镜像仓库)
    + [本地镜像管理](#本地镜像管理)
    + [容器操作](#容器操作)
    + [容器生命周期管理](#容器生命周期管理)
    + [容器rootfs命令](#容器rootfs命令)

## centos7安装docker

centos7安装docker官方文档：https://docs.docker.com/install/linux/docker-ce/centos/

+ 卸载旧版本

```
yum remove docker \
           docker-client \
           docker-client-latest \
           docker-common \
           docker-latest \
           docker-latest-logrotate \
           docker-logrotate \
           docker-engine
```

+ 安装Docker依赖

```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

+ 设置稳定的仓库

```
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

+ 安装 Docker Engine-Community

```
yum install docker-ce docker-ce-cli containerd.io
```

+ 启动docker

```
systemctl start docker
```

## docker命令

官方命令文档：https://docs.docker.com/engine/reference/commandline/docker/

#### info|version
```
version:显示 Docker 版本信息。
info:显示 Docker 系统信息，包括镜像和容器数。

例：
docker version
docker info
```

#### 镜像仓库

+ login/logout
```
docker login : 登陆到一个Docker镜像仓库，如果未指定镜像仓库地址，默认为官方仓库 Docker Hub
docker logout : 登出一个Docker镜像仓库，如果未指定镜像仓库地址，默认为官方仓库 Docker Hub

例：
docker login localhost:8080
docker logout localhost:8080
```

+ search
```
docker search:从Docker Hub查找镜像

例：
docker search centos
```

+ pull
```
docker pull:从镜像仓库中拉取或者更新指定镜像

例：
docker pull centos
```

+ push
```
docker push:将本地的镜像上传到镜像仓库,要先登陆到镜像仓库

例：
docker push myCentos:v1
```

#### 本地镜像管理

+ images
```
docker images：列出本地镜像

例：
docker images
```

+ rmi
```
docker rmi:删除本地镜像


docker images：
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              470671670cac        2 months ago        237MB

例：
docker rmi 470671670cac
docker rmi centos:latest
```

+ tag
```
docker tag:标记本地镜像，将其归入某一仓库。

docker images：
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              470671670cac        2 months ago        237MB

例：
docker tag centos:latest centos7:test

docker images：
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos7             test                470671670cac        2 months ago        237MB
centos              latest              470671670cac        2 months ago        237MB
```

+ build
```
docker build:命令用于使用Dockerfile创建镜像。
```

+ history
```
docker history:查看指定镜像的创建历史。

REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos7             test                470671670cac        2 months ago        237MB
centos              latest              470671670cac        2 months ago        237MB

例：
docker history centos

IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
470671670cac        2 months ago        /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
<missing>           2 months ago        /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B
<missing>           2 months ago        /bin/sh -c #(nop) ADD file:aa54047c80ba30064…   237MB
```

+ save/load
```
docker save:将指定镜像保存成tar存档
docker load:导入使用docker save命令导出的镜像。

例：
docker save centos > centos.tar
docker load < centos.tar
```

+ export/import
```
docker export:将容器的文件系统导出为tar存档
docker import:从tar存档文件中创建镜像。

例：
docker export centos > centos_export.tar
docker import http://example.com/exampleimage.tgz
```


#### 容器操作

+ ps
```
docker ps:列出容器

例：
docker ps
```

+ 

#### 容器生命周期管理

#### 容器rootfs命令

