# centos7安装docker compose

官方文档：https://docs.docker.com/compose/install/

+ 下载

```shell
 sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

+ 修改权限

```shell
sudo chmod +x /usr/local/bin/docker-compose
```

+ 检查版本

```shell
docker-compose --version
```
