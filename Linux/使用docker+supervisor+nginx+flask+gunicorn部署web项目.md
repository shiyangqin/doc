docker：非常流行的容器技术，本文是用其搭建centos虚拟环境  
supervisor: 进程管理工具  
nginx：高性能反向代理服务器  
gunicorn: 异步处理框架  
flask：微型web框架，可快速编写web应用  

## 安装docker

[请查看docker文档](https://github.com/shiyangqin/doc/blob/master/Linux/docker.md)

## 创建容器

本文档是在docker容器中进行，镜像为官方的centos:latest

获取镜像：docker pull centos

创建容器：docker run -itd --network=host --name mycentos centos:latest /bin/bash

进入容器：docker exec -it mycentos bash

更新yum: yum update

## 安装python3

[请查看python文档](https://github.com/shiyangqin/doc/blob/master/Linux/Python.md)

## 安装supervisor

[请查看supervisor文档](https://github.com/shiyangqin/doc/blob/master/Linux/supervisor.md)

## 安装nginx

[请查看nginx文档](https://github.com/shiyangqin/doc/blob/master/Linux/nginx.md)

## 安装gunicorn

[请查看gunicorn文档](https://github.com/shiyangqin/doc/blob/master/Linux/gunicorn.md)

