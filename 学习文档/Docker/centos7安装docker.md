# centos7安装docker

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
