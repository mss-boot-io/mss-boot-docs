---
title: 容器化
order: 12
nav:
  order: 1
  title: admin
description: 在容器中运行mss-boot-admin
keywords: [admin docker run]
---
## 准备环境
[本地安装docker环境](https://blog.csdn.net/m0_67393295/article/details/126327312?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522170460779016800180620265%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=170460779016800180620265&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-2-126327312-null-null.142^v99^pc_search_result_base1&utm_term=docker%20desktop%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B&spm=1018.2226.3001.4187)

## 运行mysql
```shell
docker run -d \
  --name mysql \
  --network host \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -e MYSQL_DATABASE=mss-boot-admin-local \
  -p 3306:3306 \
  mysql:8
```
## 迁移数据
```shell
docker run \
  --rm \
  --network host \
  ghcr.io/mss-boot-io/mss-boot-admin:latest \
  migrate
```
## 运行服务端
```shell
docker run -d \
  --name mss-boot-admin \
  --network host \
  -p 8080:8080 \
  ghcr.io/mss-boot-io/mss-boot-admin:latest \
  server
```
## 运行前端
```shell
docker run -d \
  --name mss-boot-admin-antd \
  --network host \
  -p 8000:80 \
  ghcr.io/mss-boot-io/mss-boot-admin-antd:latest
```
