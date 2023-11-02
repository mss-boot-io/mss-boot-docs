---
title: "指南"
date: 2021-12-30T15:40:00+08:00
description: 文档指南
keywords: [quickstart]
---
## 介绍
[mss-boot](https://github.com/mss-boot-io/mss-boot)是一个企业级微服务框架，基于（gin, grpc, Ant Design）实现。
[mss-boot](https://github.com/mss-boot-io/mss-boot)目前已经开源在github上，欢迎大家star和fork。
mss-boot分为四个项目
[mss-boot](https://github.com/mss-boot-io/mss-boot)、
[mss-boot-monorepo](https://github.com/mss-boot-io/mss-boot-monorepo)、
[mss-boot-frontend](https://github.com/mss-boot-io/mss-boot-frontend)和
[mss-boot-template](https://github.com/mss-boot-io/mss-boot-template),
其中mss-boot为核心框架，mss-boot-monorepo中的服务为所有后端微服务，mss-boot-frontend为后台前端服务，
mss-boot-template为代码生成提供模板支持。mss-boot主要提供了微服务的基础开发框架，并依托istio进行微服务治理。

## 前置安装

### protoc
- mac安装：
```shell
brew install protobuf
```
- linux安装：
```shell
sudo apt-get install protobuf-compiler  # For Debian/Ubuntu
sudo yum install protobuf-compiler      # For CentOS/Red Hat
```

### Go Plugins
```shell
go get -u google.golang.org/grpc
go get -u github.com/golang/protobuf/protoc-gen-go
go get -u google.golang.org/grpc/cmd/protoc-gen-go-grpc
export PATH="$PATH:$(go env GOPATH)/bin"
```

### Generate pb.go
```shell
protoc --go_out=. --go_opt=paths=source_relative helloworld.proto
```

### golangci-lint
```shell
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
```

## 特性
- devops全流程支持
- 依托istio平台的微服务治理
- rest服务基于gin框架
- grpc服务基于grpc框架
- 支持Swagger文档(基于swaggo)

## 快速开发
### 1. 创建新项目
- http项目使用模板[`service-http`](https://github.com/mss-boot-io/service-http)，直接use this template
- grpc项目使用模板[`service-grpc`](https://github.com/mss-boot-io/service-grpc)，直接use this template

### 2. 本地开发
- 根据自己的需求，修改项目对应名称
- 根据自己的需求，修改项目对应的proto文件
- 根据自己的需求，修改项目对应的config文件
- 运行`make migrate`，生成数据库表
- 运行`make run`，启动项目

## 体验环境
1. [alpha](http://alpha.mssboot.io): 提交pr后经审核后自动部署
2. [beta](http://beta.mssboot.io): 代码合并到main分支后自动部署
3. [prod](http://www.mssboot.io): 发布版本后自动部署

## 贡献者
<span style="margin: 0 5px;" ><a href="https://github.com/lwnmengjing" ><img src="https://images.weserv.nl/?url=avatars.githubusercontent.com/u/12806223?s=64&v=4&w=60&fit=cover&mask=circle&maxage=7d" /></a></span>
