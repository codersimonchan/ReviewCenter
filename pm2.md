---
layout: post
title: PM2
tags: [Server]
---
[TOC]

# PM2

## 介绍

pm2 是node进程管理工具，可以用来简化很多应用管理的繁琐任务，比如性能监控，自动重启，负载均衡等，因为在工作遇到服务器重启后，需要一个一个去重新启动每个进程（process），这样不仅繁琐，而且效率低，而且容易遗忘开启一些服务。PM2还可以与容器编排工具（如Docker和Kubernetes）集成，以便在容器中管理Node.js应用程序。

Nodemon：虽然Nodemon不是严格意义上的进程管理器，但它是一个非常有用的开发工具。Nodemon监视文件的变化，并在代码更改时自动重新启动Node.js应用程序，这对于开发过程中的实时调试和测试非常方便。在生产环境中，PM2是最常见和推荐的选择，因为它提供了丰富的功能和广泛的社区支持。

## 常用命令

运行以下命令进行全局安装

```
npm install -g pm2
```

查看版本号

```
pm2 -v
```

查看所有进程

```
pm2 list
```

启动进程并命名进程为 app_name， app.js is your service script.

```
pm2 start app.js --name app_name
```

停止进程

```
#停止特定进程
pm2 stop app_name | app_id
#停止所有进程
$ pm2 stop all
```

重启命令

```
pm2 restart app_name | app_id
```

删除命令

```
pm2 delete app_name | app_id
```

监听变化，一旦目录文件有变化，自动重启

```
pm2 start app.js --watch
```

开启多个进程, each with its own process and managed by PM2

```
pm2 start my_app.js --name my_app -i 4
```

