# Redis

+ [使用docker部署Redis](#使用docker部署Redis)
+ [redis使用](#redis使用)

___

## 使用docker部署Redis

+ 下载官方镜像

```shell
docker pull redis:6.0
```

+ 创建数据卷

```shell
docker volume create redis_data
```

+ 运行容器

```shell
docker run -itd -p 6379:6379 -v redis_data:/data --name redis  redis:6.0 --requirepass "redis"
```

-p: 指定映射端口;
-v: 绑定数据卷;
--requirepass: 设置密码

+ shell

```shell
tee ./redis_deploy.sh <<-'EOF'
#!/bin/bash
docker pull redis:6.0
docker volume create redis_data
docker run -itd -p 6379:6379 -v redis_data:/data --name redis  redis:6.0 --requirepass "redis"
EOF
sh ./redis_deploy.sh
rm -rf redis_deploy.sh
```

### redis使用

+ 进入redis

```shell
redis-cli
```

其实redis命令在[redis中文官网](http://www.redis.cn/)已经写的很清楚了，个人只需要熟悉那些命令就好了
