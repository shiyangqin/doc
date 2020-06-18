# gunicorn

+ [直接使用pip安装](#直接使用pip安装)
+ [gunicorn的参数详解](#gunicorn的参数详解)
+ [gunicorn的配置文件](#gunicorn的配置文件)
+ [使用](#使用)

gunicorn是一个unix上被广泛使用的高性能的Python WSGI HTTP Server。和大多数的web框架兼容，并具有实现简单，轻量级，高性能等特点。

___

## 直接使用pip安装

```shell
pip3 install gunicorn
pip3 install gevent
```

使用gevent做异步时才需要安装gevent

安装后执行文件在python安装路径的bin目录下（/usr/local/python3/bin/gunicorn）

## gunicorn的参数详解

```txt
-c CONFIG    : CONFIG,配置文件的路径，通过配置文件启动；生产环境使用；

-b ADDRESS   : ADDRESS，ip加端口，绑定运行的主机；

-w INT, --workers INT：用于处理工作进程的数量，为正整数，默认为1；

-k STRTING, --worker-class STRTING：要使用的工作模式，默认为sync异步，可以下载eventlet和gevent并指定

其他还有很多，因为个人不用，所以不在详写
```

## gunicorn的配置文件

注：gunicorn的配置文件必须一个py文件（例：gunicorn_config.py）

```python
# 设置监听端口
bind = '0.0.0.0:9000'
# 并行工作进程数
workers = 8
# 工作模式协程
worker_class = 'gevent'
```

## 使用

以flask为例，假设flask启动文件地址为/opt/App/run.py

```python
# -*- coding: utf-8 -*-
from flask import Flask

app = Flask(__name__)


@app.route('/demo', methods=['GET'])
def demo():
    return "gunicorn and flask demo."
```

使用gunicorn命令参数直接启动：

```shell
/usr/local/python3/bin/gunicorn -k gevent -b 0.0.0.0:9000 -w 8 run:app 
```

使用gunicorn配置文件启动：

```shell
/usr/local/python3/bin/gunicorn -c gunicorn_config.py run:app 
```
