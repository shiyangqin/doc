docker：非常流行的容器技术，本文是用其搭建centos虚拟环境  
supervisor: 进程管理工具  
nginx：高性能反向代理服务器  
gunicorn: 异步处理框架  
flask：微型web框架，可快速编写web应用  

## 安装docker

[请查看docker文档](docker.md)

## 创建容器

本文档是在docker容器中进行，镜像为官方的centos:centos7

获取镜像：docker pull centos:centos7

创建容器：docker run -itd -p 80:80 -p 443:443 --name mc centos:centos7 /bin/bash

进入容器：docker exec -it mc bash

如果需要的话可以更新yum: yum update -y

安装vim: yum install -y vim

## 安装python3

[请查看python文档](Python.md)

## 安装gunicorn

[请查看gunicorn文档](gunicorn.md)

## 安装supervisor

[请查看supervisor文档](supervisor.md)

## 安装nginx

[请查看nginx文档](nginx.md)

## 创建flask服务
```
mkdir /opt/App
vim /opt/App/run.py
-------------------------------------------------
# -*- coding: utf-8 -*-
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
   return 'Hello World'

if __name__ == '__main__':
   app.run(host='0.0.0.0', port=9000, debug=True)

-------------------------------------------------
```

## 编辑supervisor和nginx配置文件

nginx配置文件：
```
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
        listen       443;
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

supervisor配置文件：
```
[program:nginx]
directory=/
command=nginx -c /etc/nginx/nginx.conf
autostart=true
stopwaitsecs=3600

[program:run]
directory=/opt/App
command=/usr/local/python3/bin/gunicorn -k gevent -b 0.0.0.0:9000 -w 1 run:app
autostart=true
stopwaitsecs=3600

```

## 启动服务
```
supervisord -c /etc/supervisord.conf
```

查看服务状态
```
supervisorctl
```

验证：打开浏览器，输入服务器IP或IP:443，返回Hello World即部署成功
