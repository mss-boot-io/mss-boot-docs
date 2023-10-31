---
title: "grpc服务"
date: 2023-10-31T15:40:00+08:00
draft: false
---

## 目录结构
```shell
├── config
│   ├── config.go
│   ├── config_test.go
│   ├── application.yml
│   ├── application-local.yml
│   ├── application-dev.yml
│   ├── application-prod.yml
│   └── application-test.yml
├── proto
│   ├── xxx.proto
│   ├── xxx.pb.go
│   ├── xxx_grpc.pb.go
│   ├── go.mod
│   └── go.sum
├── handlers
│   ├── xxx.go
│   └── xxx_test.go
├── Dockerfile
├── go.mod
├── go.sum
├── main.go
```
