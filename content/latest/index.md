---
title: "Get Start"
date: 2021-12-30T15:40:00+08:00
draft: false
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
mac安装：
```shell
brew install protobuf
```

### Go Plugins
```shell
go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.28
go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.2
export PATH="$PATH:$(go env GOPATH)/bin"
```

### Generate pb.go
```shell
protoc --go_out=. --go_opt=paths=source_relative helloworld.proto
```

## 特性
- devops全流程支持
- 依托istio平台的微服务治理
- rest服务基于gin框架
- grpc服务基于grpc框架
- 支持Swagger文档(基于swaggo)
- dex集成
- 支持多租户
- 代码生成服务

## 体验环境
1. [alpha](http://alpha.mssboot.io): 提交pr后经审核后自动部署
2. [beta](http://beta.mssboot.io): 代码合并到main分支后自动部署
3. [prod](http://www.mssboot.io): 发布版本后自动部署

## 贡献者
<span style="margin: 0 5px;" ><a href="https://github.com/lwnmengjing" ><img src="https://images.weserv.nl/?url=avatars.githubusercontent.com/u/12806223?s=64&v=4&w=60&fit=cover&mask=circle&maxage=7d" /></a></span>
