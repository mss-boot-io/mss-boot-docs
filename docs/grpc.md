---
title: "grpc服务"
date: 2023-10-31T15:40:00+08:00
description: grpc服务文档
keywords: [grpc]
---

## 目录结构
```shell
├── cmd                         //命令行如果用到数据库可以将migrate模块引入
│   ├── cobra.go
│   ├── server
│   │   └── server.go
├── config
│   ├── config.go               //配置项
│   ├── config_test.go          //测试
│   ├── application.yml         //全局配置
│   ├── application-local.yml   //本地配置
│   ├── application-dev.yml     //dev环境配置
│   ├── application-prod.yml    //prod环境配置
│   └── application-test.yml    //test环境配置
├── proto                       //proto文件目录
│   ├── xxx.proto
│   ├── xxx.pb.go
│   ├── xxx_grpc.pb.go
│   ├── go.mod
│   └── go.sum
├── handlers                    //grpc服务handler代码
│   ├── xxx.go
│   └── xxx_test.go
├── Dockerfile                        
├── generate.go                 //生成命令集成                        
├── go.mod
├── go.sum
├── main.go
└── README.md
```
