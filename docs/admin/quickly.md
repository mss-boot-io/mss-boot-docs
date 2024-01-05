---
title: 快速开始
order: 11
nav:
  order: 1
  title: admin
description: 快速启动mss-boot-admin
keywords: [admin quickly start]
---
  `mss-boot-admin`是一个前后端分离的项目，所以需要分别下载
  [前端项目mss-boot-admin-antd](https://github.com/mss-boot-io/mss-boot-admin-antd)和
  [后端项目mss-boot-admin](https://github.com/mss-boot-io/mss-boot-admin)。

:::warning
go version >= 1.21

node version >= 18.16.0

mysql version >= 8.0

port 8080(mss-boot-admin), 8000(mss-boot-admin-antd)
:::

### 1. 下载项目

```shell
# 下载后端项目
git clone https://github.com/mss-boot-io/mss-boot-admin.git
# 下载前端项目
git clone https://github.com/mss-boot-io/mss-boot-admin-antd.git
```

### 2. 迁移数据库

```shell
# 进入后端项目
cd mss-boot-admin
# 配置数据库连接信息(可根据实际情况修改)
export DB_DSN="root:123456@tcp(127.0.0.1:3306)/mss-boot-admin-local?charset=utf8mb4&parseTime=True&loc=Local"
# 迁移数据库
go run main.go migrate
```

### 3. 生成 API 接口信息

```shell
# 生成api接口信息
go run main.go server -a
```

### 4. 启动后端服务

```shell
# 启动后端服务
go run main.go server
```

### 5. 启动前端服务

```shell
# 进入前端项目
cd mss-boot-admin-antd
# 安装依赖
npm install
# 启动前端服务
npm run start
```
