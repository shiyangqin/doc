# Dockerfile语法

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
例：ENTRYPOINT ["supervisord", "-n", "-c", "/etc/supervisord.conf"]
```

注：ENTRYPOINT和CMD的区别是ENTRYPOINT启动容器时可以设置额外参数

+ HEALTHCHECK 健康检查

+ ONBUILD 为他人做嫁衣裳
