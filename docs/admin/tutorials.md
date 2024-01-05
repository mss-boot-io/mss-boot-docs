---
title: 配置操作
order: 12
nav:
  order: 1
  title: admin
description: 用户配置与操作教程
keywords: [admin config tutorial]
---

`mss-boot-admin`的系统配置已经集成在数据库中，可以通过gorm支持的数据库进行配置，
虽然mss-boot-admin的配置支持本地文件、embed、s3协议的对象存储，但是这些配置源在后台管理系统中都不是首选，
后台管理系统的配置源首选就是数据库，因为数据库的配置可以通过后台管理系统进行配置，而且可以通过配置的方式进行迁移。

### 配置逻辑
作为软件工程项目，一般都会经历开发、测试、生产等多个环境，每个环境的配置都是不一样的，所以`mss-boot-admin`的配置也是分环境的，
我们将这些不同阶段的环境成为`stage`，默认`stage`为`local`， 即为本地开发环境。
一般提交代码并合并到主分支，这时候应该将代码发布到`beta`环境。
当`beta`环境的代码测试通过后，就可以发布到`prod`环境。

所以`mss-boot-admin`的配置分为两个部分：
`application.yml`和`application-{stage}.yml`, 其中`application.yml`为公共配置，`application-{stage}.yml`为各个环境的配置，
:::warning
优先级顺序为：application-{stage}.yml > application.yml

所以 application-{stage}.yml中的值会覆盖application.yml中的值
:::

### 定制配置

   将项目启动后，打开`http://localhost:8000`, 点击`系统管理` ->`系统配置`，即可进入系统配置页面，点击`新建`按钮，即可新建配置，如下图所示：
 
![系统配置](/images/config01.jpg)

    下面给出一个oauth2配置的示例，配置如下：

![系统配置](/images/config02.jpg)

### 模板支持
:::info
通过模板支持(go template所有语法)，可以将配置文件中的一些敏感信息通过环境变量传入，这样就可以避免将敏感信息提交到代码仓库或者数据库中。
:::

> `mss-boot-admin`的配置支持模板，模板使用`{{` `}}`包裹， 目前支持环境变量作为模板变量传入，且支持go template所有语法, 如下所示：
```yaml
oauth2:
  clientID: {{ .Env.CLIENTID }}
  clientSecret: {{ .Env.CLIENTSECRET }}
```
> 在这个示例中，`{{ .Env.CLIENTID }}`和`{{ .Env.CLIENTSECRET }}`就是模板变量，这两个变量的值是从环境变量中获取的，所以在启动服务时，需要将github的clientId和clientSecret作为环境变量传入，如下所示：
```shell
export CLIENTID=xxxx
export CLIENTSECRET=xxxx
```
