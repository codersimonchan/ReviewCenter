# Docker

## 基本概念

Docker是一个用于 构建build， 运行run，传送share 应用程序的平台。就可以将操作系统，运行时环境，第三方软件库和依赖包，以及环境变量，配置文件，启动命令等打包在一起，以便在任何环境中都可以正确地运行。那么只要在开发环境中运行成功了，那么在测试环境以及生产环境一定也是运行成功的。

![image-20231204203608451](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231204203608451.png)

## Docker和虚拟机的区别

### 虚拟机

Vmware，Virtalbox，Parallels desktop等虚拟机，是将一台物理服务器虚拟化称为多台虚拟化的服务器，每个逻辑服务器都有自己的操作系统，CPU，内存，硬盘，网络接口等，他们之间是完全隔离的，可以独立运行。不过缺点也很明显，每台虚拟机都需要占用大量的资源，而大部分情况下，其实我们的一台服务器上只需要运行一个主要对外提供服务的应用程序就可以了，并不需要一个完成的操作系统，所提供的所有功能，导致了资源的浪费。

### 容器和虚拟机的区别

Docker只是容器的一种实现，是一个容器化的解决方案和平台。而容器是一种虚拟化技术，和虚拟机类似，也是一个独立的环境，可以在这个环境中运行应用程序，和虚拟机不同的是，它并不需要在容器中运行一个完整的操作系统，而是使用宿主机的操作系统，所以启动速度非常快。同时因为运行需要的资源更少，所以在一台物理服务器上可以运行更多的容器。

![image-20231204204927494](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231204204927494.png)

### 基本原来和概念

镜像：是一个只读的模板，可以用来创建容器，容器是docker的运行的一个实例，它提供了一个独立的可移植的环境。可以在这个环境中运行应用程序。镜像和容器的关系，就像类和实例的关系一样，容器就是这个模板的一个实例，可以有一个，也可以有多个。

Docker 仓库： 是用来存储docker镜像的地方，最流行的仓库就是DockerHub

Docker 是使用Client-Server架构模式，Docker Client和Docker Daemon之间通过Socket或者RESTful API进行通信，Docker Daemon就是服务端的守护进程，他负责管理Docker的各种资源。Docker Client负责向Docker Daemon发送请求，Docker Daemon接收请求之后进行处理，然后将结果返回给Docker Client，这里的Docker Daemon是一个后台进程，所以我们在终端中输入的各种Docker 命令，实际上都是通过Docker客户端发送给Docker Daemon的。然后Docker Deamon再进行处理，最后再将结果返回给客户端。然后就可以在终端中看到执行结果了。

![image-20231204211214828](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231204211214828.png)

## 安装配置

docker.com 下载安装包，双击安装即可。安装完不要忘记启动docker。

## 容器化

就是将应用程序打包成容器，然后在容器中运用应用程序的过程，这个过程简单来说可以分成三个步骤

1. 创建要给Dockerfile，来告诉docker 构建应用程序镜像所需要的步骤和配置
2. 使用Dockerfile来构建镜像
3. 使用镜像创建和运行容器

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