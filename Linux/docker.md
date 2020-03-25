+ [centos7安装docker](#centos7安装docker)
+ [docker命令](#docker命令)

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

#### 容器生命周期管理

#### 容器操作

#### 容器rootfs命令

#### 镜像仓库

#### 本地镜像管理

#### info | version
```
version:显示 Docker 版本信息。
info:显示 Docker 系统信息，包括镜像和容器数。

例：
docker version
docker info
```
