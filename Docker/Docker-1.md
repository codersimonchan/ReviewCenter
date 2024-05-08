# Docker

笔记：https://blog.csdn.net/qq_21197507/article/details/115071715

## 基本概念

当我们发布一个项目的时候，可不可以**带上环境一起进行打包**呢，这样的话就可以保证在任何环境中都可以运行（从windows到docker）。Docker可以将操作系统，运行时环境，第三方软件库和依赖包，以及环境变量，配置文件，启动命令等打包在一起，以便在任何环境中都可以正确地运行。那么只要在开发环境中运行成功了，那么在测试环境以及生产环境一定也是运行成功的。

传统：开发人员准备jar，运维来部署

现在：开发人员打包部署上线，一套流程做完。DevOps（开发运维）

Docker 的思想就来自于集装箱

JRE --- 多个应用（端口冲突）---说明应用之间原来都是交叉的！

现在要隔离起来：Docker的核心思想，就是打包装箱！---每个箱子都是互相隔离的。 再也不用担心应用端口冲突问题了。

Docker通过隔离机制，可以将服务器利用到极致。

![image-20231204203608451](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231204203608451.png)

## Docker的历史

2010年，几个搞IT的年轻人，在美国成立里一家dotcloud！

做一些pass的云计算服务。 有关的容器技术！

他们将自己的技术命名就是Docker！

Docker刚刚诞生的时候，没有引起行业的注意！就经营不下去！

2013年就开始开源---开放源代码！将所有的代码公布出去！

那么越来越多的人发现了docker的优点！就火了！之后几乎每个月都更新一个版本！

2014年 Docker 1.0发布！

Docker为什么这么火？相对虚拟机十分的轻巧。在容器技术出来之前，我们都是使用的虚拟机技术。

## Docker和虚拟机的区别

### 虚拟机

Vmware，Virtalbox，Parallels desktop等虚拟机，是将一台物理服务器虚拟化称为多台虚拟化的服务器，每个逻辑服务器都有自己的操作系统，CPU，内存，硬盘，网络接口等，他们之间是完全隔离的，可以独立运行。不过缺点也很明显，每台虚拟机都需要占用大量的资源，而大部分情况下，其实我们的一台服务器上只需要运行一个主要对外提供服务的应用程序就可以了，并不需要一个完成的操作系统，所提供的所有功能，导致了资源的浪费。

### 容器和虚拟机的区别

Docker只是容器的一种实现，是一个容器化的解决方案和平台。而容器是一种虚拟化技术，和虚拟机类似，也是一个独立的环境，可以在这个环境中运行应用程序，和虚拟机不同的是，它并**不需要在容器中运行一个完整的操作系统**，而是使用宿主机的操作系统，所以启动速度非常快。同时因为运行需要的资源更少，所以在一台物理服务器上可以运行更多的容器。

![image-20231204204927494](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231204204927494.png)

### 基本概念

Docker 是基于Go语言开发的，开源项目。

官网： https://www.docker.com/

文档地址：https://docs.docker.com/  Docker的文档是超详细的。

镜像image：

> 是一个只读的模板，可以用来创建容器服务，容器是docker的运行的一个实例，它提供了一个独立的可移植的环境。可以在这个环境中运行应用程序。镜像和容器的关系，就像类和实例的关系一样，容器就是这个模板的一个实例，可以有一个，也可以有多个。

仓库repository：

> Docker 仓库： 是用来存储docker镜像的地方，最流行的仓库就是DockerHub

Docker 是使用Client-Server架构模式，Docker Client和Docker Daemon之间通过Socket或者RESTful API进行通信，Docker Daemon就是服务端的守护进程，他负责管理Docker的各种资源。Docker Client负责向Docker Daemon发送请求，Docker Daemon接收请求之后进行处理，然后将结果返回给Docker Client，这里的Docker Daemon是一个后台进程，所以我们在终端中输入的各种Docker 命令，实际上都是通过Docker客户端发送给Docker Daemon的。然后Docker Deamon再进行处理，最后再将结果返回给客户端。然后就可以在终端中看到执行结果了。

![image-20231204211214828](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231204211214828.png)

## 安装配置

使用以下命令查看Linux系统的信息

```
cat /etc/os-release
//the info about the linux system.
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

## Install

在Linux系统中依据以下手册进行安装

https://docs.docker.com/engine/install/centos/

```
[root@Newaim-SMG01 ~]# systemctl start docker
[root@Newaim-SMG01 ~]# docker version

 var/lib/docker docker的默认工作路径
```

## 回顾Run流程和Docker原理？？？

```
docker run hello-world
```

![img](https://img-blog.csdnimg.cn/20200411132109992.png)

Docker Engine 是一个客户端-服务器应用程序，具有以下主要组件：

- 一个服务器，他是一种长期运行的程序，称为守护进程。运行在主机上，通过Socket从客户端访问
- DockerServer 接收到Docker Client的指令，就会执行这个命令

### Docker常用命令

#### 帮助命令

```
docker version
docker info
docker 命令 --help
```

帮助文档: https://docs.docker.com/engine/reference/commandline

#### 镜像命令

**docker images** 查看所有本地的主机上的镜像

```
[root@Newaim-SMG01 ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
return        latest    e28edeb325b9   46 hours ago    60MB
hello-world   latest    d2c94e258dcb   12 months ago   13.3kB

```

**docker search** 搜索镜像

```
docker search mysql[:tag]

通过收藏过滤,收藏大于3000
--filer=START=3000
```

**docker pull** 下载镜像

```
docker pull mysql
如果不写tag，默认就是最新版的，指定版本命令如下
docker pull mysql:5.7
```

**docker remove** 删除镜像

```
docker rmi -f e73346bdf465（镜像id） #删除指定的镜像
```

#### 容器命令

**说明**：我们有了镜像才可以创建容器，我们下载一个linux发行版centos镜像测试学习。

```
docker pull centos 下载centos
docker images 查看
```

**新建容器并启动**

```
docker run -it centos /bin/bash
docker run [可选参数] images
```

![image-20240506195220835](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20240506195220835.png)

**列出所有的运行的容器**

![image-20240506195516299](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20240506195516299.png)

**退出容器**

```
exit 直接容器停止并退出
ctrl+P+Q 容器不停止退出
```

**删除容器**

```
docker rm 容器id ，不能删除正在运行的容器
docker rm -f 删除所有的容器
```

启动和停止容器的操作

```
docker start 容器id
docker restart 容器id
docker stop 容器id
docker kill 容器id  #强制停止当前容器
```

**后台启动容器**

![image-20240506202428630](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20240506202428630.png)

进入当前正在运行的容器

```
docker exec -it 容器id /bin/bash  （进入容器后开启一个新的终端，可以在里面操作，常用）
docker attach 容器id   （进入容器正在执行的终端，不会启动新的进程）
```

**从容器内拷贝到主机上**

```
docker cp 容器id：容器内路径 目的地主机路径
将文件copy到主机上
docker cp b8453025116：home/test.jave /home
```



**部署Nginx**

```
# pull the images
docker pull ngin
# 将容器内80端口，映射给宿主机3344端口，这意味着你可以通过访问主机的 3344 端口来访问容器中运行的 Nginx 服务
docker run -d --name nginx01 -p 3344:80 nginx (将容器内80端口，映射给服务器3344端口)
# check docker contianer
docker ps
# 访问服务器3344端口，本机自测
curl localhost:3344

进入容器
查看nginx内部配置文件
```

思考问题：我们每次改动nginx配置文件，都需要进入容器内部，十分麻烦，我要是可以在容器外部提供一个映射路径，使容器内部就可以自动修改? -v 数据卷。

**部署Tomcat**

```
# docker run也会自动帮我们下载
docker run -it --rm tomcatL9.0
# 我们之前的启动都是后台，停止了容器之后，容器还是可以查到， 以上一般用来测试，用完就删除 docker run -it --rm，不建议这么用

docker pull tomcat
docker pull tomcat:9.0
# start tomcat
docker run -d -p 3355:8080 --name tomcat01 tomcat
# 进入容器，发现 1，Linux命令少了，2 没有webapps。 因为镜像原因，默认为最小的镜像，所有不必要的都剔除掉，保证最小化可用。
docker exec -it tomcat01 /bin/bash

```

思考问题：如果以后要部署项目，如果每次都进入容器是不是十分麻烦，我要是可以在容器外部提供一个映射路径，webapps，我们在外部防止项目，就自动同步到内部就好了。

## 容器化

就是将应用程序打包成容器，然后在容器中运用应用程序的过程，这个过程简单来说可以分成三个步骤

1. 创建要给Dockerfile，来告诉docker 构建应用程序镜像所需要的步骤和配置
2. 使用Dockerfile来构建镜像
3. 使用镜像创建和运行容器
4. **容器是一种对进程进行隔离的运行环境。**

Dockerfile是一个根目录下的创建文件，里面包含了一条条的指令，用来告诉Docker 如何来构建镜像，这个镜像文件中包括了我们应用程序的所有指令，也就是我们刚刚提到的，各种依赖，环境配置和运行应用程序所需要的所有内容。Docker就会根据这个Dockerfile文件，来构建一个镜像。有了镜像之后，我们就可以使用这个镜像来创建容器。然后在容器中运行应用程序。

![image-20231204214806273](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231204214806273.png)

## 构建镜像

```
# Make base image change if needed to use the required node version
FROM node:18.16.0-slim as builder
WORKDIR /app
COPY . .
RUN yarn install && yarn build

FROM nginx:1.23-alpine
COPY --from=builder /app/dependencies/nginx/defaultk8s.conf /etc/nginx/conf.d/default.conf
COPY --from=builder /app/build /usr/share/nginx/html


COPY source(相对于Dockerfile文件的路径) dest（相对于镜像的路径）
```

## Docker Compose & Kubernetes

Docker Compose 和 Kubernetes 是用于管理容器化应用程序的两种不同的工具/平台。

- Docker Compose 主要用于本地开发和测试环境，是一个简单的工具，适用于单机环境下少量容器的管理。
- Kubernetes 更适用于大规模容器集群的管理，具有高可用性、弹性伸缩、自动化部署等强大功能，适合于生产环境中的复杂应用。

在某些情况下，可以将 Docker Compose 结合 Kubernetes 使用，例如可以在开发阶段使用 Docker Compose 进行本地开发和测试，然后在部署到生产环境时使用 Kubernetes 进行容器编排和管理。