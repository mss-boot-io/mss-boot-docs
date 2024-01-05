---
title: 介绍
order: 10
nav:
  order: 0
  title: admin
description: 介绍mss-boot-admin
keywords: [admin mss-boot-admin]
---

## 简介

> 基于 `Gin` + `React` + `Atn Design v5` + `Umi v4` + `mss-boot` 的前后端分离权限管理系统，系统初始化只需要一个环境变量即可启动系统，系统支持多种配置源，迁移指令可以让初始化数据库信息更简单，服务指令可以很方便启动服务。

## 🎬 体验环境

[体验地址](https://admin-beta.mss-boot-io.top)

> 账号：`admin` 密码：`123456`
> 
> 或者直接点击github图标通过`github`登录

## ✨ 特性

- 支持国际化
- 标准 Restful API 开发规范
- 基于 Casbin 的 RBAC 权限管理
- 基于 Gorm 的数据库存储
- 基于 Gin 的中间件开发
- 基于 Gin 的 Swagger 文档生成
- 支持 oauth2.0 第三方登录
- 支持 swagger 文档生成
- 支持多种配置源(本地文件、embed、对象存储 s3 等、gorm 支持的数据库、mongodb)
- 虚拟模型支持(动态配置支持前后端功能)
- 支持数据库迁移
- 支持微服务代码生成

## 📦 内置功能

- 用户管理: 用户是系统操作者，该功能主要完成系统用户配置。
- 角色管理: 角色菜单权限分配、设置角色按机构进行数据范围权限划分。
- 菜单管理: 配置系统菜单，操作权限，按钮权限标识等。
- 选项管理: 动态配置枚举。
- 模型管理: 管理虚拟模型。
- 系统配置: 管理各种环境的配置。
- 通知公告: 用户通知消息。
- 任务管理: 管理定时任务，包括执行日志。
- 国际化管理: 管理国际化资源。
- 微服务代码生成: 根据模板生成微服务代码。

##  贡献者

<span style="margin: 0 5px;" ><a href="https://github.com/lwnmengjing" ><img src="https://images.weserv.nl/?url=avatars.githubusercontent.com/u/12806223?s=64&v=4&w=60&fit=cover&mask=circle&maxage=7d" /></a></span> <span style="margin: 0 5px;" ><a href="https://github.com/wangde7" ><img src="https://images.weserv.nl/?url=avatars.githubusercontent.com/u/56955959?s=64&v=4&w=60&fit=cover&mask=circle&maxage=7d" /></a></span>

## 建议反馈

本文档为静态站点，项目托管在`github` [mss-boot-docs](https://github.com/mss-boot-io/mss-boot-docs) 基于 [dumi](https://d.umijs.org/) 开发，
使用`github actions`将编译后的静态文件发布到`cloudflare workers`。

如果您有任何建议或者意见，欢迎提`issue`或者`pr`。

## 📨 互动

<table>
   <tr>
    <td><img src="https://mss-boot-io.github.io/.github/images/wechat.jpg" width="180px"></td>
    <td><img src="https://mss-boot-io.github.io/.github/images/wechat-mp.jpg" width="180px"></td>
    <td><img src="https://mss-boot-io.github.io/.github/images/qq-group.jpg" width="200px"></td>
    <td><a href="https://space.bilibili.com/597294782/channel/seriesdetail?sid=3881026&ctype=0">mss-boot-io</a></td>
  </tr>
  <tr>
    <td>微信</td>
    <td>公众号🔥🔥🔥</td>
    <td><a target="_blank" href="https://shang.qq.com/wpa/qunwpa?idkey=0f2bf59f5f2edec6a4550c364242c0641f870aa328e468c4ee4b7dbfb392627b"><img border="0" src="https://pub.idqqimg.com/wpa/images/group.png" alt="mss-boot技术交流群" title="mss-boot技术交流群"></a></td>
    <td>哔哩哔哩🔥🔥🔥</td>
  </tr>
</table>
