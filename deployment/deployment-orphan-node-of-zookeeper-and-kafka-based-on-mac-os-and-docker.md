---
title: 在macOS下基于Docker部署单节点的Kafka以及ZooKeeper
description: 在macOS下基于Docker部署开发环境使用的Kafka和ZooKeeper手记
published: true
date: 2021-06-29T09:41:24.493Z
tags: orphan, docker, zookeeper, kafka, docker-compose
editor: markdown
dateCreated: 2021-06-29T09:36:32.740Z
---

# 在macOS下基于Docker部署单节点的Kafka以及ZooKeeper

> 在macOS在一般为了方便本地开发，都会将某些开源中间件甚至于数据库基于Docker环境部署于本地环境。

## Requirements

1. [Docker](https://docs.docker.com/get-docker/)
2. [Docker Compose](https://docs.docker.com/compose/)

## 使用Docker Compose部署单节点ZooKeeper

```yaml
version: '3'

services:
    zookeeper-orphan:
        restart: always
        hostname: zookeeper-orphan
        container_name: zookeeper-orphan
        image: zookeeper:3.5
        ports:
            - 2181:2181
        environment:
            ZOO_MY_ID: 1
            ZOO_SERVERS: server.1=zookeeper-orphan:2888:3888;2181

networks:
    default:
        name: zookeeper-orphan
```

运行`docker compose up -d`即可自动化的拉取镜像并创建对应容器。

## 验证ZooKeeper是否可用

一般我会使用[ZooNavigator](https://zoonavigator.elkozmon.com/en/docs-pre-v1/quickstart.html)来管理和对ZooKeeper上的数据进行变更，同样的，可以使用Docker Compose来部署ZooNavigator。  

```yaml
version: '3'

services:
    zoonavigator:
        image: elkozmon/zoonavigator:latest
        container_name: zoonavigator
        restart: unless-stopped
        ports:
            - 3873:9000
        environment:
            HTTP_PORT: 9000
```

部署完成之后访问[http://localhost:3872](http://localhost:3872)即可看到连接界面：

![login-for-zoonavigator.png](/login-for-zoonavigator.png)

在macOS下我们可以使用`host.docker.internal:2181`来借助于Docker for Desktop提供的内部DNS机制来访问到我们的ZooKeeper服务。

![zoonavigator-main.png](/zoonavigator-main.png)

## 使用Docker Compose部署单节点Kafka

## 验证Kafka是否可用
