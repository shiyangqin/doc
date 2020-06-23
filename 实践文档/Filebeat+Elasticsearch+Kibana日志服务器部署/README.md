# Filebeat+Elasticsearch+Kibana日志服务器部署

Filebeat：日志收集器

Elasticsearch：一个开源的分布式、RESTful 风格的搜索和数据分析引擎

Kibana：为 Elasticsearch 设计的开源分析和可视化平台

+ docker-compose.yml：Elasticsearch和Kibana部署文件
  + filebeat
    + filebeat.yml：filebeat配置文件
    + docker-compose.yml：filebeat部署文件

## Filebeat配置文件

```yml
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /opt/logs/*.log  # 配置文件路径（容器内，在yml配置文件里做文件映射）

output.elasticsearch:
  hosts: ["10.255.175.224:9200"]  # es地址
```

## 部署

需先安装docker和docker compose

+ 部署Kibana和es

将docker-compose.yml上传至服务器，在文件所在目录下执行docker compose命令

```shell
docker-compose up -d
```

+ 部署Filebeat

将filebeat文件夹上传至日志文件所在服务器，进入filebeat文件夹，修改好配置文件，执行docker compose命令

```shell
docker-compose up -d
```
