---
title: "http服务"
date: 2023-10-31T15:40:00+08:00
description: http服务文档
keywords: [http]
---

## 目录结构
```shell
├── apis                                //api接口
│   ├── xxx.go
│   └── xxx_test.go
├── cmd                                 //命令行
│   ├── cobra.go
│   ├── server                          //服务启动
│   │   ├── server.go
│   ├── migrate                         //数据库迁移
│   │   ├── migration
│   │   │   ├── custom                  //自定义迁移
│   │   │   │   └── xxx_migrate.go
│   │   │   ├── system                  //系统迁移
│   │   │   │   ├── xxx_migrate.go
├── config
│   ├── config.go                       //配置项
│   ├── config_test.go                  //测试
│   ├── application.yml                 //全局配置
│   ├── application-local.yml           //本地配置
│   ├── application-dev.yml             //dev环境配置
│   ├── application-prod.yml            //prod环境配置
│   └── application-test.yml            //test环境配置
├── docs                                //swagger文档
│   ├── docs.go
│   ├── swagger.json
│   └── swagger.yaml
├── dto                                 //api接口参数
│   ├── xxx.go
│   └── xxx_test.go
├── middleware                          //中间件
│   ├── xxx.go
│   └── xxx_test.go
├── models                              //数据库模型
│   ├── xxx.go
│   └── xxx_test.go
├── pkg                                 //公共包
│   ├── xxx.go
│   └── xxx_test.go
├── router                              //路由包
│   ├── router.go
│   └── router_test.go
├── Dockerfile
├── generate.go                        //生成命令集成                        
├── go.mod
├── go.sum
├── main.go
└── README.md
```
