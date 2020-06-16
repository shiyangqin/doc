# docker

+ [centos7安装docker](#centos7安装docker)
+ [docker命令](#docker命令)
  + [info|version](#info|version)
  + [镜像仓库](#镜像仓库)
  + [本地镜像管理](#本地镜像管理)
  + [容器生命周期管理](#容器生命周期管理)
  + [容器操作](#容器操作)
+ [Dockerfile](#Dockerfile)
+ [Docker数据卷](#Docker数据卷)
+ [Docker网络](#Docker网络)
  + [使用Docker网络进行容器互联](#使用Docker网络进行容器互联)
+ [Docker-Compose](#Docker-Compose)

___

## centos7安装docker

centos7安装docker官方文档：https://docs.docker.com/install/linux/docker-ce/centos/

+ 卸载旧版本

```shell
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

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```

+ 设置稳定的仓库

```shell
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

+ 安装 Docker Engine-Community

```shell
yum install -y docker-ce docker-ce-cli containerd.io
```

+ 启动docker

```shell
systemctl start docker
systemctl enable docker
```

+ 查看是否安装成功

```shell
docker version
```

+ shell

```shell
tee ./docker_deploy.sh <<-'EOF'
#!/bin/bash
yum remove docker \
           docker-client \
           docker-client-latest \
           docker-common \
           docker-latest \
           docker-latest-logrotate \
           docker-logrotate \
           docker-engine
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install -y docker-ce docker-ce-cli containerd.io
systemctl start docker
systemctl enable docker
EOF
sh ./docker_deploy.sh
rm -rf docker_deploy.sh
docker version
```

+ 修改镜像源为阿里源

进入地址https://cr.console.aliyun.com/

点击左侧镜像加速器，复制下方代码，直接粘贴复制到centos执行即可

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://***.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## docker命令

本文档命令并不全，想看详细的文档还是看官方文档较好

官方命令文档：https://docs.docker.com/engine/reference/commandline/docker/

### info|version

```txt
version:显示 Docker 版本信息。
info:显示 Docker 系统信息，包括镜像和容器数。

例：
docker version
docker info
```

### 镜像仓库

+ login/logout

```txt
docker login : 登陆到一个Docker镜像仓库，如果未指定镜像仓库地址，默认为官方仓库 Docker Hub
docker logout : 登出一个Docker镜像仓库，如果未指定镜像仓库地址，默认为官方仓库 Docker Hub

例：
docker login localhost:8080
docker logout localhost:8080
```

+ search

```txt
docker search:从Docker Hub查找镜像

例：
docker search centos
```

+ pull

```txt
docker pull:从镜像仓库中拉取或者更新指定镜像

例：
docker pull centos

使用命令行无法查询镜像的版本，需要查询时，只能上官网查询：https://hub.docker.com/
```

+ push

```txt
docker push:将本地的镜像上传到镜像仓库,要先登陆到镜像仓库

例：
docker push myCentos:v1
```

### 本地镜像管理

+ images

```txt
docker images：列出本地镜像

例：
docker images
```

+ rmi

```txt
docker rmi:删除本地镜像
-f: 	强制删除

docker images：
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              470671670cac        2 months ago        237MB

例：
docker rmi 470671670cac
docker rmi centos:latest
```

+ tag

```txt
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

```txt
docker build:命令用于使用Dockerfile创建镜像。
```

+ history

```txt
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

```txt
docker save:将指定镜像保存成tar存档
docker load:导入使用docker save命令导出的镜像。

例：
docker save centos > centos.tar
docker load < centos.tar
```

+ export/import

```txt
docker export:将容器的文件系统导出为tar存档
docker import:从tar存档文件中创建镜像。

例：
docker export centos > centos_export.tar
docker import http://example.com/exampleimage.tgz
```

### 容器生命周期管理

+ run

```txt
docker run:创建一个新的容器并运行一个命令

--name="nginx-lb": 为容器指定一个名称；
-d: 后台运行容器，并返回容器ID
-i: 以交互模式运行容器，通常与-t同时使用
-t: 为容器重新分配一个伪输入终端，通常与-i同时使用
-p: 指定端口映射，格式为：主机(宿主)端口:容器端口
-v: 绑定挂载卷，只读
-w: 设置工作目录
--network：将容器连接到网络
--network-alias：为容器添加网络别名
--restart: 重新启动策略
           no-默认策略，在容器退出时不重启容器。
           on-failure，在容器非正常退出时（退出状态非0），才会重启容器
           on-failure:3，在容器非正常退出时重启容器，最多重启3次
           unless-stopped，在容器退出时总是重启容器，但是不考虑在Docker守护进程启动时就已经停止了的容器
           always：在容器退出时总是重启容器

例：
docker run -itd -p 443:443 -v /opt:/opt --name mycentos centos:latest /bin/bash
```

+ start/stop/restart

```txt
docker start :启动一个或多个已经被停止的容器
docker stop :停止一个运行中的容器
docker restart :重启容器

例：
docker start mycentos
docker stop mycentos
docker restart mycentos
```

+ kill

```txt
docker kill: 杀死一个或多个正在运行的容器

例：
docker kill mycentos
```

+ rm

```txt
docker rm 删除容器。
-f: 强制删除正在运行的容器
-l: 移除容器之间的链接
-v: 删除与容器关联的卷
例：

docker rm -f mycentos
```

+ pause/unpause

```txt
docker pause :暂停容器中所有的进程。
docker unpause :恢复容器中所有的进程。

例：
docker pause mycentos
docker unpause mycentos
```

+ create

```txt
docker create: 创建一个新的容器但不启动它
语法同run
```

+ exec

```txt
docker exec:在正在运行的容器中运行命令
-i:即使未连接STDIN也保持打开状态
-t:分配伪TTY

例：
docker exec -it mycentos bash
```

### 容器操作

+ ps

```txt
docker ps:列出容器
-a:显示所有容器，包括未运行的

例：
docker ps -a
```

+ inspect

```txt
docker inspect: 获取容器/镜像的信息

例：
docker inspect mycentos
```

+ top

```txt
docker top: :查看容器中运行的进程信息

例：
docker top mycentos
```

+ attach

```txt
docker attach: 连接到正在运行中的容器。

有缺陷，一般不用，一般使用docker exec -it mycentos bash进入容器，使用CTRL + D退出容器
```

+ events

```txt
docker events : 从服务器获取实时事件

具体用法看官方文档：https://docs.docker.com/engine/reference/commandline/events/
```

+ log

```txt
docker logs : 获取容器的日志

例：
docker logs mycentos
```

+ wait

```txt
docker wait : 阻塞运行直到容器停止，然后打印出它的退出代码。

例：
运行docker wait，它将阻塞直到容器退出。
docker wait mycentos

在另一个终端，停止容器。docker wait上面的命令返回退出代码。
docker stop mycentos

docker wait会输出0
```

+ export/import

```txt
docker export:将容器的文件系统导出为tar存档
docker import:从tar存档文件中创建镜像。

例：
docker export centos > centos_export.tar
docker import http://example.com/exampleimage.tgz
```

+ port

```txt
docker port: 列出端口映射或容器的特定映射

例：
docker port mycentos
```

+ commit

```txt
docker commit: 从容器创建一个新的镜像。

例：
docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
7b277c456aa7        centos:latest       "/bin/bash"         2 hours ago         Up 12 minutes       0.0.0.0:443->443/tcp   mycentos

docker commit mycentos mycentos 或 docker commit 7b277c456aa7 mycentos

docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mycentos            latest              998085ae3c1f        6 seconds ago       239MB
centos              latest              470671670cac        2 months ago        237MB
```

+ cp

```txt
docker cp: 在容器和本地文件系统之间复制文件/文件夹

例：
将主机/opt/asset文件夹拷贝到容器mycentos的/opt/目录下
docker cp /opt/asset 7b277c456aa7:/opt/ 或 docker cp /opt/asset mycentos:/opt/

将主机/opt/asset文件夹拷贝到容器mycentos的/opt/asset文件夹下
docker cp /opt/asset 7b277c456aa7:/opt/asset 或 docker cp /opt/asset mycentos:/opt/asset

将容器mycentos的/opt/asset拷贝到主机的/opt目录中
docker cp 7b277c456aa7:/opt/asset /opt/ 或 docker cp mycentos:/opt/asset /opt/
```

+ diff

```txt
docker diff: 检查容器文件系统上文件或目录的更改
A: 文件或目录已添加
D: 文件或目录已删除
C: 文件或目录已更改

例：
docker diff mycentos
```

## Dockerfile

官方文档：https://docs.docker.com/engine/reference/builder/

Dockerfile 是一个文本文件，其内包含了一条条的 指令(Instruction)，每一条指令构建一层，因此每一条指令的内容，就是描述该层应当如何构建。

+ FROM 指定基础镜像

```txt
格式：FROM <image>或FROM <image>:<tag>
例：FROM centos:centos7
```

+ USER 指定当前用户

```txt
格式：USER daemon
例：USER root
```

+ WORKDIR 指定工作目录，为后续的 RUN、CMD、ENTRYPOINT 指令配置工作目录

```txt
格式：WORKDIR /path/to/workdir
例：WORKDIR /opt
```

+ RUN 执行命令

```txt
格式为 RUN <command> 或 RUN ["executable", "param1", "param2"]
例：RUN mkdir /usr/local/python3
```

注：RUN越少越好，多个命令可以用&&连接

+ COPY 复制文件

```txt
格式：COPY <src> <dest>
例：COPY ./package.json /usr/src/app/
```

+ ADD 更高级的复制文件

```txt
格式：ADD <src> <dest>
例：ADD ./package.json /usr/src/app/
```

注：ADD 指令是在 COPY 基础上增加了一些功能。<源路径> 可以是一个 URL，这种情况下，Docker 引擎会试图去下载这个链接的文件放到 <目标路径> 去。如果 <源路径> 为一个 tar 压缩文件的话，压缩格式为 gzip, bzip2 以及 xz 的情况下，ADD 指令将会自动解压缩这个压缩文件到 <目标路径> 去。

+ ARG 设置构建参数

```txt
格式：ARG <key>=<value>
例：ARG REDIS_SET_PASSWORD=redis
```

注：ARG设置的参数只在build包括CMD和ENTRYPOINT中有效，在image被创建和container启动之后，无效。

+ ENV 设置环境变量

```txt
格式：ENV <key> <value>
例：ENV POSTGRES_PASSWORD postgres
```

注：在Dockerfile中使用，在build docker imag的过程中有效，在image被创建和container启动后作为环境变量依旧也有效，并且可以重写覆盖。

+ VOLUME 定义匿名卷

```txt
格式：VOLUME ["/data"]
例：VOLUME /data
```

注：可以被启动容器时的-v参数覆盖

+ EXPOSE 声明容器暴露的端口号

```txt
格式：EXPOSE <port> [<port>...]
例：EXPOSE 80
```

注：可以被启动容器时的-p参数覆盖

+ CMD 容器启动命令， 只能有一条

```txt
格式：CMD ["executable","param1","param2"]
例：CMD ["supervisord", "-n", "-c", "/etc/supervisord.conf"]
```

注：CMD只能执行有持续前台输出的命令，如果是后台任务，容器会直接退出

+ ENTRYPOINT 容器启动命令，只能有一条

```txt
格式：ENTRYPOINT ["executable", "param1", "param2"]
例：["supervisord", "-n", "-c", "/etc/supervisord.conf"]
```

注：ENTRYPOINT和CMD的区别是ENTRYPOINT启动容器时可以设置额外参数

+ HEALTHCHECK 健康检查

+ ONBUILD 为他人做嫁衣裳

## Docker数据卷

在Docker中，要想实现数据的持久化（所谓Docker的数据持久化即数据不随着容器的结束而结束），需要将数据从宿主机挂载到容器中。docker volume命令就是用于实现数据持久化的一种方式

docker默认存储数据卷的路径为：/var/lib/docker/volumes/<数据卷名称>/

+ docker volume create 创建一个卷

```txt
docker volume create mc_data
```

+ docker volume inspect 查看一个或多个数据卷信息

```txt
[root@bogon ~]# docker volume inspect af5c305219cb058bc0a63d001737686e8ce3c177fc8dfef14ab44854a36d291a
[
    {
        "CreatedAt": "2020-04-20T17:13:28+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/af5c305219cb058bc0a63d001737686e8ce3c177fc8dfef14ab44854a36d291a/_data",
        "Name": "af5c305219cb058bc0a63d001737686e8ce3c177fc8dfef14ab44854a36d291a",
        "Options": null,
        "Scope": "local"
    }
]
```

+ docker volume ls 查看现有数据卷列表

```txt
[root@bogon ~]# docker volume ls
DRIVER              VOLUME NAME
local               af5c305219cb058bc0a63d001737686e8ce3c177fc8dfef14ab44854a36d291a
```

+ docker volume prune 删除所有未使用的数据卷

```txt
-y 不提示确认

例：
docker volume prune
docker volume -y prune
```

+ docker volume rm 删除一个或多个数据卷

```txt
-f 强制删除

例：
docker volume rm af5c305219cb058bc0a63d001737686e8ce3c177fc8dfef14ab44854a36d291a
```

## Docker网络

客户端和守护程序API都必须至少为 1.21， 才能使用此命令。docker version在客户端上使用命令检查客户端和守护程序API版本。

+ docker network connect

```txt
将容器连接到网络
--alias：为容器添加别名
--driver-opt：网络的驱动程序选项
--ip：IPv4地址（例如172.30.100.104）
--ip6：IPv6地址（例如2001：db8 :: 33）

例：
启动容器时将其连接到网络：
docker run -itd --network=multi-host-network --network-alias my_net busybox

将正在运行的容器添加到网络：
docker network connect multi-host-network container1

将正在运行的容器添加到网络，并为容器创建网络别名：
docker network connect --alias mysql multi-host-network container2
```

+ docker network create

```txt
创建一个网络
-d: 驱动程序来管理网络,有2个值供选择：bridge：桥接网络（不给-d参数时，默认是桥接网络）；overlay：覆盖网络

例：
docker network create -d bridge my-net
```

+ docker network disconnect

```txt
断开容器与网络的连接
-f: 强制断开容器与网络的连接

例：
docker network disconnect -f multi-host-network container1
```

+ docker network inspect

```txt
在一个或多个网络上显示详细信息
-f: 使用给定的Go模板格式化输出
-v: 详细输出以进行诊断

例：
docker network inspect host
```

+ docker network ls

```txt
列出网络

例：
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
b362ee7d74e8        bridge              bridge              local
24a43bf8e2e7        host                host                local
ab5585fc37ff        none                null                local
```

+ docker network prune

```txt
删除所有未使用的网络
--filter： 提供过滤器值（例如'until ='）
-f: 不提示确认

例：
docker network prune
```

+ docker network rm

```txt
删除一个或多个网络

例：
docker network rm my-net
```

### 使用Docker网络进行容器互联

生成容器的镜像请查看[使用docker+supervisor+nginx+flask+gunicorn部署web项目](使用docker+supervisor+nginx+flask+gunicorn部署web项目.md)

+ 创建网络

```txt
docker network create my-net
```

+ 将容器连接到创建的网络

```txt
将运行中的容器添加到网络：
docker run -itd -p 80:80 --name mc mc:test /start.sh
docker network connect --alias mc1 my-net mc1

生成容器时设置网络：
docker run -itd --network my-net --network-alias mc2 --name mc2 mc:test
```

+ 验证：

```txt
在mc1中测试是否能够连接到mc2：
ping mc2

在mc2中访问mc1中的服务(返回“Hello World[”表示成功)：
curl mc1
```

## Docker-Compose

Compose是用于定义和运行多容器Docker应用程序的工具。通过Compose，您可以使用YAML文件来配置应用程序的服务。然后，使用一个命令，就可以从配置中创建并启动所有服务。

[YAML文件语法](../基础/YAML语法.md)

Compose文件包含4个部分：

1. version: Compose文件版本
2. services: 服务信息
3. networks: 网络信息
4. volumes: 数据卷信息

官方文档：https://docs.docker.com/compose/compose-file/

官方示例：

```yml
version: "3.8"
services:

  redis:
    image: redis:alpine
    ports:
      - "6379"
    networks:
      - frontend
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  db:
    image: postgres:9.4
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    deploy:
      placement:
        constraints:
          - "node.role==manager"

  vote:
    image: dockersamples/examplevotingapp_vote:before
    ports:
      - "5000:80"
    networks:
      - frontend
    depends_on:
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure

  result:
    image: dockersamples/examplevotingapp_result:before
    ports:
      - "5001:80"
    networks:
      - backend
    depends_on:
      - db
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  worker:
    image: dockersamples/examplevotingapp_worker
    networks:
      - frontend
      - backend
    deploy:
      mode: replicated
      replicas: 1
      labels: [APP=VOTING]
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints:
          - "node.role==manager"

  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    stop_grace_period: 1m30s
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints:
          - "node.role==manager"

networks:
  frontend:
  backend:

volumes:
  db-data:
```

### version配置

Docker Compose文件版本，版本的对应关系查看官方文档

```yml
version: "3.8"
```

### services配置

#### build

+ context
+ dockerfile
+ args

### networks配置

### volumes配置
