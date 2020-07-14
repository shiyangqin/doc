# docker-compose语法

docker compose是用于定义和运行多容器Docker应用程序的工具。通过Compose，您可以使用YAML文件来配置应用程序的服务。然后，使用一个命令，就可以从配置中创建并启动所有服务。

一般情况下，Compose文件主要包含4个部分：

1. version: Compose文件版本
2. services: 服务信息
3. networks: 网络信息
4. volumes: 数据卷信息

官方文档：https://docs.docker.com/compose/compose-file/

## version配置

Docker Compose文件版本，版本的对应关系查看官方文档

```yml
version: "3.8"
```

## services配置

+ build：在build时的配置选项
  + context：包含Dockerfile的目录的路径，此目录也是Docker构建时的上下文路径
  + dockerfile：指定dockerfile文件
  + args：构建参数，用于dockerfile文件中
  + cache_from
  + labels
  + network
  + shm_size
  + target

示例1：

```yml
version: "3.8"
services:
  my_service:
    build: ./dir
```

my_service：自定义服务名称  
build: ./dir：指定构建目录，使用该目录下的Dockerfile进行构建

示例2：

```yml
version: "3.8"
services:
  webapp:
    build:
      context: ./dir
      dockerfile: Dockerfile-alternate
      args:
        buildno: 1
        gitcommithash: cdc3b19
```

context: ./dir：指定构建目录  
dockerfile：指定dockerfile文件  
args：设置buildno和gitcommithash参数，在dockerfile中使用

args参数使用方法：

```Dockerfile
ARG buildno
ARG gitcommithash

RUN echo "Build number: $buildno"
RUN echo "Based on commit: $gitcommithash"
```

+ image：指定启动容器的映像，如果同时指定了build和image，那么 Compose 会构建镜像并且把镜像命名为 image 后面的那个名字

```yml
image: ubuntu:18.04
```

```yml
build: ./dir
image: webapp:tag
```

+ cap_add, cap_drop：添加或删除容器功能

```yml
cap_add:
  - ALL

cap_drop:
  - NET_ADMIN
  - SYS_ADMIN
```

+ cgroup_parent：为容器指定一个可选的父cgroup

```yml
cgroup_parent: m-executor-abcd
```

+ command：重写默认启动命令

```yml
command: ["bundle", "exec", "thin", "-p", "3000"]
```

+ configs：配置，略

+ container_name：指定容器名

```yml
container_name: my-container
```

+ credential_spec：略

+ depends_on：服务间的依赖关系，例如服务依赖redis，所以服务启动之前，需要等redis先启动

```yml
version: "3.8"
services:
  web:
    build: .
    depends_on:
      - db
      - redis
  redis:
    image: redis
  db:
    image: postgres
```

+ deploy：略
  + endpoint_mode
  + labels
  + mode
  + placement
  + replicas
  + max_replicas_per_node
  + resources
  + restart_policy
  + rollback_config
  + update_config

+ devices：略

+ DNS：自定义DNS服务器

```yml
dns: 8.8.8.8
```

```yml
dns:
  - 8.8.8.8
  - 9.9.9.9
```

+ dns_search：略

+ entrypoint：重写entrypoint

```yml
entrypoint: ["php", "-d", "memory_limit=-1", "vendor/bin/phpunit"]
```

+ env_file：从文件添加环境变量，略

+ environment：环境变量

```yml
environment:
  RACK_ENV: development
  SHOW: 'true'
  SESSION_SECRET:
```

```yml
environment:
  - RACK_ENV=development
  - SHOW=true
  - SESSION_SECRET
```

+ expose：暴露端口

```
expose:
  - "3000"
  - "8000"
```

+ external_links：不推荐使用，略

+ extra_hosts：略

+ healthcheck：略

+ init：略

+ isolation：略

+ labels：略

+ links：不推荐使用，略

+ logging：略

+ network_mode：略

+ networks：网络设置
  + aliases：别名
  + ipv4_address：ipv4地址
  + ipv6_address：ipv6地址

```yml
version: "3.8"

services:
  web:
    image: "nginx:alpine"
    networks:
      - new

  worker:
    image: "my-worker-image:latest"
    networks:
      - legacy

  db:
    image: mysql
    networks:
      new:
        aliases:
          - database
      legacy:
        aliases:
          - mysql
        ipv4_address: 172.16.238.10
        ipv6_address: 2001:3984:3989::10

networks:
  new:
  legacy:
    ipam:
        driver: default
        config:
          - subnet: "172.16.238.0/24"
          - subnet: "2001:3984:3989::/64"
```

+ pid：略

+ ports：端口映射

```yml
ports:
  - "3000"
  - "3000-3005"
  - "8000:8000"
  - "9090-9091:8080-8081"
  - "49100:22"
  - "127.0.0.1:8001:8001"
  - "127.0.0.1:5000-5010:5000-5010"
  - "6060:6060/udp"
  - "12400-12500:1240"
```

```yml
ports:
  - target: 80
    published: 8080
    protocol: tcp
    mode: host
```

+ restart：重启策略

```yml
restart: "no"
restart: always
restart: on-failure
restart: unless-stopped
```

+ secrets：略

+ security_opt：略

+ stop_grace_period：略

+ stop_signal：略

+ sysctls：略

+ tmpfs：略

+ ulimits：略

+ userns_mode：略

+ volumes：指定挂载文件或数据卷

```yml
volumes:
  # Just specify a path and let the Engine create a volume
  - /var/lib/mysql

  # Specify an absolute path mapping
  - /opt/data:/var/lib/mysql

  # Path on the host, relative to the Compose file
  - ./cache:/tmp/cache

  # User-relative path
  - ~/configs:/etc/configs/:ro

  # Named volume
  - datavolume:/var/lib/mysql
```

```yml
version: "3.8"
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - type: volume
        source: mydata
        target: /data
        volume:
          nocopy: true
      - type: bind
        source: ./static
        target: /opt/app/static

volumes:
  mydata:
```

## volumes配置

+ driver：略
+ driver_opts：略
+ external：如果为true，则docker-compose不会创建此数据卷，不存在则引发错误
+ labels：略
+ name：指定数据卷名称

```yml
volumes:
  my-app-data:
    name: my-app-data
```

## networks配置

+ driver：略
+ driver_opts：略
+ attachable：略
+ ipam：自定义IPAM配置
+ internal：略
+ labels：略
+ external：如果为true，则docker-compose不会创建此网络，不存在则引发错误
+ name：指定网络名称

### 测试

docker-compose.yml：

```yml
version: "3.8"
services:
  web:
    image: nginx:alpine
    container_name: web
    ports:
      - "80:80"
    volumes:
      - web-data:/data
    networks:
      web-net:
        aliases:
          - web

networks:
  web-net:
    name: web-net
volumes:
  web-data:
    name: web-data
```

启动：

```shell
docker-compose up -d
```

## 官方示例

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
