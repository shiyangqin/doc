+ [centos7安装docker](#centos7安装docker)
+ [docker命令](#docker命令)
    + [info|version](#info|version)
    + 镜像
        + [镜像仓库](#镜像仓库)
        + [本地镜像管理](#本地镜像管理)
    + 容器
        + [容器生命周期管理](#容器生命周期管理)
        + [容器操作](#容器操作)
+ [Dockerfile](#Dockerfile)
+ [Docker数据卷](#Docker数据卷)
+ [Docker容器互联](#Docker容器互联)
    + [Docker网络](#Docker网络)
    + [使用Docker网络进行容器互联](#使用Docker网络进行容器互联)


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
yum install -y docker-ce docker-ce-cli containerd.io
```

+ 启动docker
```
systemctl start docker
systemctl enable docker
```

+ 查看是否安装成功
```
docker version
```

+ 修改镜像源为阿里源

进入地址https://cr.console.aliyun.com/

点击左侧镜像加速器，复制下方代码，直接粘贴复制到centos执行即可
```
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

使用命令行无法查询镜像的版本，需要查询时，只能上官网查询：https://hub.docker.com/
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
-f: 	强制删除

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

#### 容器生命周期管理

+ run
```
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
```
docker start :启动一个或多个已经被停止的容器
docker stop :停止一个运行中的容器
docker restart :重启容器

例：
docker start mycentos
docker stop mycentos
docker restart mycentos
```

+ kill
```
docker kill: 杀死一个或多个正在运行的容器

例：
docker kill mycentos
```

+ rm
```
docker rm 删除容器。
-f: 强制删除正在运行的容器
-l: 移除容器之间的链接
-v: 删除与容器关联的卷
例：

docker rm -f mycentos
```

+ pause/unpause
```
docker pause :暂停容器中所有的进程。
docker unpause :恢复容器中所有的进程。

例：
docker pause mycentos
docker unpause mycentos
```

+ create
```
docker create: 创建一个新的容器但不启动它
语法同run
```

+ exec
```
docker exec:在正在运行的容器中运行命令
-i:即使未连接STDIN也保持打开状态
-t:分配伪TTY

例：
docker exec -it mycentos bash
```

#### 容器操作

+ ps
```
docker ps:列出容器
-a:显示所有容器，包括未运行的


例：
docker ps -a
```

+ inspect
```
docker inspect: 获取容器/镜像的信息

例：
docker inspect mycentos
```

+ top
```
docker top: :查看容器中运行的进程信息

例：
docker top mycentos
```

+ attach
```
docker attach: 连接到正在运行中的容器。

有缺陷，一般不用，一般使用docker exec -it mycentos bash进入容器，使用CTRL + D退出容器
```

+ events
```
docker events : 从服务器获取实时事件

具体用法看官方文档：https://docs.docker.com/engine/reference/commandline/events/
```

+ log
```
docker logs : 获取容器的日志

例：
docker logs mycentos
```

+ wait
```
docker wait : 阻塞运行直到容器停止，然后打印出它的退出代码。

例：
运行docker wait，它将阻塞直到容器退出。
docker wait mycentos

在另一个终端，停止容器。docker wait上面的命令返回退出代码。
docker stop mycentos

docker wait会输出0
```

+ export/import
```
docker export:将容器的文件系统导出为tar存档
docker import:从tar存档文件中创建镜像。

例：
docker export centos > centos_export.tar
docker import http://example.com/exampleimage.tgz
```

+ port
```
docker port: 列出端口映射或容器的特定映射

例：
docker port mycentos
```

+ commit
```
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
```
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
```
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

+ from 指定基础镜像
```
FROM centos:centos7
```

+ run 执行命令

其格式有两种：

shell 格式：RUN <命令>，就像直接在命令行中输入的命令一样。
```
RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
```

exec 格式：RUN ["可执行文件", "参数1", "参数2"]
```
错误示范：
FROM centos:centos7

RUN yum install -y gcc openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel libffi-devel tk-devel wget curl-devel make
RUN wget -P /opt https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tar.xz
RUN mkdir /usr/local/python3
RUN tar -xvJf /opt/Python-3.7.2.tar.xz -C /opt
RUN /opt/Python-3.7.2/configure --prefix=/usr/local/python3
RUN make
RUN make install
```

Dockerfile 中每一个指令都会建立一层，RUN 也不例外。每一个 RUN 都会新建立一层，而上面的这种写法，创建了7层镜像，这是完全没有意义的，正确的写法是:
```
FROM centos:centos7

RUN yum install -y gcc openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel libffi-devel tk-devel wget curl-devel make \
    && wget -P /opt https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tar.xz \
    && tar -xvJf /opt/Python-3.7.2.tar.xz -C /opt \
    && mkdir /usr/local/python3 \
    && /opt/Python-3.7.2/configure --prefix=/usr/local/python3 \
    && make \
    && make install \
    && ln -s /usr/local/python3/bin/python3 /usr/local/bin/python3 \
    && ln -s /usr/local/python3/bin/pip3 /usr/local/bin/pip3 \
    && rm -rf /opt/Python*
```

一定要确保每一层只添加真正需要添加的东西，任何无关的东西都应该清理掉。

运行：  
```
docker build -t dockerfile:test .
```

+ COPY 复制文件

格式：COPY <源路径>... <目标路径>

```
COPY package.json /usr/src/app/
```

<源路径> 可以是多个，甚至可以是通配符，其通配符规则要满足 Go 的 filepath.Match 规则，如：
```
COPY hom* /mydir/
COPY hom?.txt /mydir/
```

注意在运行命令最后的“.”，“.”表示上下文路径为当前目录

这就引入了上下文的概念。当构建的时候，用户会指定构建镜像上下文的路径，docker build 命令得知这个路径后，会将路径下的所有内容打包，然后上传给 Docker 引擎。这样 Docker 引擎收到这个上下文包后，展开就会获得构建镜像所需的一切文件。

如果在 Dockerfile 中这么写：
```
COPY ./package.json /app/
```

这并不是要复制执行 docker build 命令所在的目录下的 package.json，也不是复制 Dockerfile 所在目录下的 package.json，而是复制 上下文（context） 目录下的 package.json。

+ ADD 更高级的复制文件

ADD 指令和 COPY 的格式和性质基本一致。但是在 COPY 基础上增加了一些功能。

<源路径> 可以是一个 URL，这种情况下，Docker 引擎会试图去下载这个链接的文件放到 <目标路径> 去。

如果 <源路径> 为一个 tar 压缩文件的话，压缩格式为 gzip, bzip2 以及 xz 的情况下，ADD 指令将会自动解压缩这个压缩文件到 <目标路径> 去。
```
ADD files* /mydir/
```

+ CMD 容器启动命令

CMD 指令的格式和 RUN 相似，也是两种格式：

shell 格式：CMD <命令>

exec 格式：CMD ["可执行文件", "参数1", "参数2"...]

+ ENTRYPOINT 入口点

ENTRYPOINT 的格式和 RUN 指令格式一样，分为 exec 格式和 shell 格式。

ENTRYPOINT 的目的和 CMD 一样，都是在指定容器启动程序及参数。ENTRYPOINT 在运行时也可以替代，不过比 CMD 要略显繁琐，需要通过 docker run 的参数 --entrypoint 来指定。

+ ENV 设置环境变量

格式有两种：

ENV <key> <value>

ENV <key1>=<value1> <key2>=<value2>...

这个指令很简单，就是设置环境变量而已，无论是后面的其它指令，如 RUN，还是运行时的应用，都可以直接使用这里定义的环境变量

+ ARG 构建参数

格式：ARG <参数名>[=<默认值>]

构建参数和 ENV 的效果一样，都是设置环境变量。所不同的是，ARG 所设置的构建环境的环境变量，在将来容器运行时是不会存在这些环境变量的。但是不要因此就使用 ARG 保存密码之类的信息，因为 docker history 还是可以看到所有值的。

+ VOLUME 定义匿名卷

格式为：

VOLUME ["<路径1>", "<路径2>"...]

VOLUME <路径>

之前我们说过，容器运行时应该尽量保持容器存储层不发生写操作，对于数据库类需要保存动态数据的应用，其数据库文件应该保存于卷(volume)中

+ EXPOSE 声明端口

格式为 EXPOSE <端口1> [<端口2>...]。

EXPOSE 指令是声明运行时容器提供服务端口，这只是一个声明，在运行时并不会因为这个声明应用就会开启这个端口的服务。

+ WORKDIR 指定工作目录

格式为 WORKDIR <工作目录路径>。

使用 WORKDIR 指令可以来指定工作目录（或者称为当前目录），以后各层的当前目录就被改为指定的目录，如该目录不存在，WORKDIR 会帮你建立目录。

+ USER 指定当前用户

格式：USER <用户名>[:<用户组>]

USER 指令和 WORKDIR 相似，都是改变环境状态并影响以后的层。WORKDIR 是改变工作目录，USER 则是改变之后层的执行 RUN, CMD 以及 ENTRYPOINT 这类命令的身份。

+ HEALTHCHECK 健康检查

格式：

HEALTHCHECK [选项] CMD <命令>：设置检查容器健康状况的命令

HEALTHCHECK NONE：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令

HEALTHCHECK 指令是告诉 Docker 应该如何进行判断容器的状态是否正常，这是 Docker 1.12 引入的新指令。

+ ONBUILD 为他人做嫁衣裳

格式：ONBUILD <其它指令>。

ONBUILD 是一个特殊的指令，它后面跟的是其它指令，比如 RUN, COPY 等，而这些指令，在当前镜像构建时并不会被执行。只有当以当前镜像为基础镜像，去构建下一级镜像的时候才会被执行。

## Docker数据卷

暂未学习

## Docker容器互联

#### Docker网络

客户端和守护程序API都必须至少为 1.21， 才能使用此命令。docker version在客户端上使用命令检查客户端和守护程序API版本。

+ docker network connect
```
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
```
创建一个网络
-d: 驱动程序来管理网络,有2个值供选择：bridge：桥接网络（不给-d参数时，默认是桥接网络）；overlay：覆盖网络

例：
docker network create -d bridge my-net
```

+ docker network disconnect
```
断开容器与网络的连接
-f: 强制断开容器与网络的连接

例：
docker network disconnect -f multi-host-network container1
```

+ docker network inspect
```
在一个或多个网络上显示详细信息
-f: 使用给定的Go模板格式化输出
-v: 详细输出以进行诊断

例：
docker network inspect host
```

+ docker network ls
```
列出网络

例：
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
b362ee7d74e8        bridge              bridge              local
24a43bf8e2e7        host                host                local
ab5585fc37ff        none                null                local
```

+ docker network prune
```
删除所有未使用的网络
--filter： 提供过滤器值（例如'until ='）
-f: 不提示确认

例：
docker network prune
```

+ docker network rm
```
删除一个或多个网络

例：
docker network rm my-net
```

#### 使用Docker网络进行容器互联

生成容器的镜像请查看[使用docker+supervisor+nginx+flask+gunicorn部署web项目文档](使用docker+supervisor+nginx+flask+gunicorn部署web项目.md)

+ 创建网络
```
docker network create my-net
```

+ 将容器连接到创建的网络
```
将运行中的容器添加到网络：
docker network connect --alias mc1 my-net mc1

生成容器时设置网络：
docker run -itd --network my-net --network-alias mc2 --name mc2 mc:test
```

+ 验证：
```
在mc1中测试是否能够连接到mc2：
ping mc2

在mc2中访问mc1中的服务(返回“Hello World[”表示成功)：
curl mc1
```
