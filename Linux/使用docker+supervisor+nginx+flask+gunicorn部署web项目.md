# 使用docker+supervisor+nginx+flask+gunicorn部署web项目

+ [安装docker](#安装docker)
+ [编写run文件与其他配置文件](#编写run文件与其他配置文件)
+ [编写Dockerfile构建镜像](#编写Dockerfile构建镜像)
+ [启动容器](#启动容器)

docker：非常流行的容器技术，本文是用其搭建centos虚拟环境  
supervisor: 进程管理工具  
nginx：高性能反向代理服务器  
gunicorn: 异步处理框架  
flask：微型web框架，可快速编写web应用  

## 安装docker

[请查看docker文档](docker.md)

## 编写run文件与其他配置文件

/opt/dockerfile/run.py

```python
# -*- coding: utf-8 -*-
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
   return 'Hello World'
```

/opt/dockerfile/sap.ini

```ini
[program:nginx]
directory=/opt
command=nginx -c /etc/nginx/nginx.conf -g 'daemon off;'
autostart=true
stopwaitsecs=3600

[program:run]
directory=/opt/App
command=/usr/local/python3/bin/gunicorn -k gevent -b 0.0.0.0:9000 -w 1 run:app
autostart=true
stopwaitsecs=3600
```

/opt/dockerfile/nginx.conf

```conf
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
#error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
#    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
#                      '$status $body_bytes_sent "$http_referer" '
#                      '"$http_user_agent" "$http_x_forwarded_for"';

#    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
#    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    # Load modular configuration files from the /etc/nginx/conf.d directory.
    # See http://nginx.org/en/docs/ngx_core_module.html#include
    # for more information.
    include /etc/nginx/conf.d/*.conf;

    server {
        listen       80;
        server_name  my_app;
        root         /opt/App;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://localhost:9000/;
        }
        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }

# Settings for a TLS enabled server.
#
#    server {
#        listen       443 ssl http2 default_server;
#        listen       [::]:443 ssl http2 default_server;
#        server_name  _;
#        root         /usr/share/nginx/html;
#
#        ssl_certificate "/etc/pki/nginx/server.crt";
#        ssl_certificate_key "/etc/pki/nginx/private/server.key";
#        ssl_session_cache shared:SSL:1m;
#        ssl_session_timeout  10m;
#        ssl_ciphers HIGH:!aNULL:!MD5;
#        ssl_prefer_server_ciphers on;
#
#        # Load configuration files for the default server block.
#        include /etc/nginx/default.d/*.conf;
#
#        location / {
#        }
#
#        error_page 404 /404.html;
#            location = /40x.html {
#        }
#
#        error_page 500 502 503 504 /50x.html;
#            location = /50x.html {
#        }
#    }

}
```

## 编写Dockerfile构建镜像

结合其他文档中的服务安装方法，编写Dockerfile：

```Dockerfile
FROM centos:centos7

RUN yum install -y gcc openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel libffi-devel tk-devel wget curl-devel make \
    && wget -P /opt https://www.python.org/ftp/python/3.8.2/Python-3.8.2.tar.xz \
    && tar -xvJf /opt/Python-3.8.2.tar.xz -C /opt \
    && mkdir /usr/local/python3 \
    && /opt/Python-3.8.2/configure --prefix=/usr/local/python3 \
    && make \
    && make install \
    && ln -s /usr/local/python3/bin/python3 /usr/local/bin/python3 \
    && ln -s /usr/local/python3/bin/pip3 /usr/local/bin/pip3 \
    && rm -rf /opt/Python* \
    && pip3 install flask \
    && pip3 install gunicorn \
    && pip3 install gevent \
    && yum install -y epel-release \
    && yum install -y supervisor \
    && yum install -y nginx

ADD dockerfile/run.py /opt/App/run.py
ADD dockerfile/sap.ini /etc/supervisord.d/sap.ini
ADD dockerfile/nginx.conf /etc/nginx/nginx.conf

CMD ["supervisord", "-n", "-c", "/etc/supervisord.conf"]

EXPOSE 80
```

在/opt下执行命令

```shell
docker build -t mc:test .
```

## 启动容器

启动容器：

```shell
docker run -itd -p 80:80 --name mc mc:test
```

验证服务：

打开浏览器，输入服务器IP地址，返回“Hello World”表示成功
