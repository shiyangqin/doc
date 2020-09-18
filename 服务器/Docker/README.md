# Docker

+ [centos7安装docker](centos7安装docker.md)

+ [centos7安装docker-compose](centos7安装docker-compose.md)

+ [Docker命令](Docker命令.md)

+ [Dockerfile语法](Dockerfile语法.md)

+ [docker-compose语法](docker-compose语法.md)

+ [docker-compose命令](docker-compose命令.md)

## Docker是什么

docker是为了解决代码环境运行不一致问题

docker镜像简单来说就是一个只读的文件系统

docker容器就是在docker镜像上添加了一个读写层的进程

## 容器和虚拟机的区别

虚拟机：虚拟机提供整个虚拟化硬件层，每个虚拟机都有自己的完整操作系统。传统虚拟机会占用大量磁盘空间：除了虚拟机托管的任何应用程序外，它们还包含完整的操作系统和相关工具。成本很高

容器：容器是应用程序级构造，共享操作系统环境（内核），因此它们比完整虚拟机使用更少的资源，并减轻主机内存的压力，操作更灵活
