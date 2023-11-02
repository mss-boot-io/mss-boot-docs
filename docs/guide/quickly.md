---
title: 快速开始
order: 11
nav:
  title: 指南
  order: 1
keywords: [quickstart]
---
# 安装环境

## 安装golang 1.21+
参考[golang官网](https://go.dev/dl/)

## mysql8+安装(docker)
```shell
#安装mysql8
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:8
#进入容器
docker exec -it mysql bash
#创建数据库
mysql -h 127.0.0.1 -p123456 -e 'create database service-http default charset utf8'
```

## protoc
### mac安装：
```shell
brew install protobuf
```
### linux安装：
```shell
sudo apt-get install protobuf-compiler  # For Debian/Ubuntu
sudo yum install protobuf-compiler      # For CentOS/Red Hat
```

## Go Plugins
```shell
go get -u google.golang.org/grpc
go get -u github.com/golang/protobuf/protoc-gen-go
go get -u google.golang.org/grpc/cmd/protoc-gen-go-grpc
export PATH="$PATH:$(go env GOPATH)/bin"
```

## Generate pb.go
```shell
protoc --go_out=. --go_opt=paths=source_relative helloworld.proto
```

## golangci-lint
```shell
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
```

## 快速开发
#### 1. 创建新项目
![看这里](/images/use-template.png)
- http项目使用模板[`service-http`](https://github.com/mss-boot-io/service-http)，直接use this template
- grpc项目使用模板[`service-grpc`](https://github.com/mss-boot-io/service-grpc)，直接use this template

#### 2. 本地开发
- 根据自己的需求，修改项目对应名称
- 根据自己的需求，修改项目对应的proto文件
- 根据自己的需求，修改项目对应的config文件
- 运行`make migrate`，生成数据库表
- 运行`make run`，启动项目

## 开发自己服务
### http服务
  请参考 [service-http](/http)
### grpc服务
  请参考 [service-grpc](/grpc)
