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
# 是rmi 是remove image的缩写
# -f 是force的意思，强制删除镜像
```

#### 容器命令

**说明**：我们有了镜像才可以创建容器，我们下载一个linux发行版centos镜像测试学习。

```
docker pull centos #下载centos
docker images #查看
```

**新建容器并启动**

```
docker run -it centos /bin/bash
docker run [可选参数] images
```

>### 常用参数说明
>
>**-d, --detach**
>
>分离的意思，在后台运行容器并返回容器ID。
>
>**-it**
>
>- **-i, --interactive**：保持标准输入打开（用于交互式会话）。
>- **-t, --tty**：分配一个伪终端。
>
>**--name**
>
>- **作用**：为容器指定一个名称。
>
>**-p, --publish**
>
>- **作用**：将容器的端口映射到主机的端口。
>
>**-v, --volume**
>
>- **作用**：绑定挂载一个卷。
>
>**--rm**
>
>- **作用**：在容器停止后自动删除容器。
>
>**-e, --env**
>
>- **作用**：设置环境变量。
>
>**--network**
>
>- **作用**：指定容器连接的网络。

```
docker run -d -p 8080:80 --name my-nginx -v /my/local/content:/usr/share/nginx/html -e NGINX_HOST=myhost -e NGINX_PORT=80 nginx
```

解释：

- `-d`：在后台运行容器。在 Docker 中，后台运行容器指的是以守护进程（daemon）模式运行容器，使其在后台运行，不会阻塞终端。这样你可以继续在终端执行其他命令，而容器在后台继续运行。
- 
- `-p 8080:80`：将主机的 8080 端口映射到容器的 80 端口。
- `--name my-nginx`：为容器指定名称 `my-nginx`。
- `-v /my/local/content:/usr/share/nginx/html`：将主机的目录 `/my/local/content` 挂载到容器的 `/usr/share/nginx/html` 目录。
- `-e NGINX_HOST=myhost` 和 `-e NGINX_PORT=80`：设置环境变量 `NGINX_HOST` 和 `NGINX_PORT`。
- `nginx`：使用 `nginx` 镜像。

通过这些参数，你可以灵活地控制容器的行为和配置。

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

**可视化**

- portainer可视化面板安装

  什么是portainer，是docker图形化界面管理工具，提供一个后台面板供我们操作。

  ```
  docker run -d -p 8088:9000 \
  --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
  ```

  访问测试：外网： 8088

  可视化面板我们平时不会使用，大家自己测试玩玩即可。

# Docker 镜像讲解

镜像是一种轻量级，可执行的独立软件包，用来打包润建运行环境和基于运行环境开发的软件，它包含运行某个软件所需要的所有内容，包括代码、运行时，库，环境变量和配置文件。

所有的应哟个，直接打包成docker镜像，就可以直接跑起来。

如何得到镜像：

从远程仓库下载

朋友copy给你

自己制作一个镜像DockerFile

## 镜像原理之联合文件系统

- Docker 镜像采用了分层的设计，每一层都包含了文件系统的一部分，而这些层通过 UnionFS 进行联合。当你创建一个新的镜像时，Docker 会在现有的镜像基础上创建一个新的容器层，该容器层与之前的层通过 UnionFS 进行联合，形成一个新的镜像。

- 通过这种分层的设计，Docker 镜像可以实现高效的复用和共享。如果两个镜像都使用了相同的基础镜像，那么它们会共享相同的基础层，而不需要重复存储相同的文件。这样可以节省存储空间，并且加快镜像的下载和部署速度。

- 比如我们分别拉取了镜像Centos，MySql以及Mongodb，假如Mysql和Mongdb都是运行在Centos上面，那么我们只需要一个Centos就可以了。

  ![](Docker-1/images.png)

  ![](Docker-1/newimage.png)

## commit 镜像

如何自己提交一个镜像

```
docker commit 提交容器称为一个新的副本镜像

#命令和git原理类似
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名：[TAG]
```

实战测试

```
1.启动一个默认的tomcat
2.发现这个默认的tomcat是没有webapps应用，官方的镜像默认webapps下面是没有文件的。
3.我自己拷贝进了进本文件
4.然后将修改过的容器提交为一个新的镜像，这样我们就可以直接使用我们修改过的镜像即可。
docker commit -m='' -a='' 7e119b82cff6 tomcat02:1.0
这样repository里面就有一个名为tomcat02，TAG为1.0的镜像。
```

## 容器数据卷

数据如果也存在容器当中，那么容器删除，数据就会丢失了，那么我们需求数据可以持久化。

比如MySQL，容器删了，删库跑路，需求：MySQL数据可以存储在本地！

容器之间可以有一个数据共享的技术，Docker容器中产生的数据，同步到本地。

这就是卷技术，目录的挂载，将我们容器内的目录，挂载到Linux上面。

总结：容器的持久化和同步操作！荣期间也是可以数据共享的。

### 使用数据卷

```
docker run -it -v 主机目录：容器内目录 images /bin/bash

docker run -it -v /home/ceshi:/home centos /bin/bash
ceshi 是linux上文件夹， /home是docker容器中文件夹，二者文件夹内容的是同步的。

docker inspect 容器id
查看容器挂载的详细信息
```

### 实战：安装MySQL

思考：MySQL的数据持久化的问题

![](C:\Users\simon.cheng\Documents\ReviewCenter\Docker\Docker-1\mysqlexample.png)

### 具名和匿名挂载

将挂载的卷命名就是具名挂载，否则就是匿名挂载，他们不需要宿主机路径，会有一个默认路径。

![](C:\Users\simon.cheng\Documents\ReviewCenter\Docker\Docker-1\volume.png)

查看卷volume的具体位置

![](C:\Users\simon.cheng\Documents\ReviewCenter\Docker\Docker-1\mount.png)

 如何确定是具名挂载还是匿名挂载，还是指定路径挂载呢？

```
-v 容器内路径   #匿名挂载
-v 卷名:容器内路径  #具名挂载
-v /宿主机路径::容器内路径  #指定挂载路径
通过 -v 容器内路径:ro rw 改变读写权限
ro readonly #只读
rw readwrite #可读可写

docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx/:ro nginx
docker run -d -P --name nginx02 -v juming-nginx:etc/nginx:rw nginx
只要看到ro 就说明路径只能通过宿主机改变，容器内是无法操作的。
```

# 初识Dockerfile

Dockerfile 就是用来构建docker镜像的构建文件，是一个命令脚本，先体验一下，通过这个脚本可以生成镜像。

镜像是一层一层的，脚本是一个个命令，每个命令是一层

```
# 创建一个dockerfile文件，名字可以随机，建议Dockerfile
# 文件中的内容， 指令(大写) 参数
FROM centos

VOLUME ["volume01","volume02"] //匿名挂载，默认挂载到宿主机

CMD echo "----end----"

CMD /bin/bash

# 这里的每个命令，就是镜像的一层
```

 这种方式我们未来使用的十分多，因为我们通常会构建自己的镜像！

## 数据卷容器

![](C:\Users\simon.cheng\Documents\ReviewCenter\Docker\Docker-1\share.png)

容器之间实现数据同步。

```
--volumes-from
相当于 子继承父，那么父镜像中挂载的数据卷，在儿子中都是同步和共享的。
测试：可以删除docker01，查看docker02和docker03，看看是否还是可以访问呢
是可以访问的，相当于一种备份机制。

这也就是说，多个mysql实现数据共享
```

结论：

容器之间配置信息的传递，数据卷生命周期一直持续到没有容器使用位置。但是一旦你持久化到了本地，这个时候本地的数据是不会删除的。

## 构建步骤

```
编写一个dockerfile 文件
docker build 构建成镜像
docker run 运行镜像
docker push 发布镜像（DockerHub）
```

## 构建镜像

### 基础知识

```
1.每个保留关键字都是必须大写字母
2.#表注释
3.从上到下顺序执行
4.每一个指令都会创建一个新的镜像层，并提交
```

dockerfile是面向开发，我们以后要发布项目，做镜像，就需要编写dockerfile文件，这个文件十分简单。docker镜像逐渐称为企业交付的标准，必须要掌握。

## 指令说明

```
FROM        # 基础镜像 一切从这里开始构建 centos
MAINTAINER  # 镜像是谁写的，姓名+邮箱
RUN         # 镜像构建时需要执行的一些命令
ADD         # 比如说我想搭建里面有tomcat的容器，比如在centos上面添加tomcat
WORKDIR     # 镜像的当前工作目录
VOLUME      # 挂载的目录
EXPOST      # 保留端口的位置
CMD         # 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT  # 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILID    # 刚构建一个被继承的DockerFile 这个时候就会运行ONBUILD 的指令，触发指令
COPY        # 类似ADD，将我们文件拷贝到镜像中
ENV         # 构建的时候设置环境变量
```

### 实战测试

Docker Hub 中99%的镜像都是从scratch开始的，然后配置需要的软件和配置进行进一步的构建。

### 发布自己的镜像

```
#DockerHub
```

确定账号可以登录，在我们服务器行提交自己的镜像。

### Docker所有流程小结 

![img](https://img-blog.csdnimg.cn/20200813194116511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbmppYW5oYWk=,size_16,color_FFFFFF,t_70#pic_center)

![img](https://img-blog.csdnimg.cn/2020081319424581.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbmppYW5oYWk=,size_16,color_FFFFFF,t_70#pic_center)



# Dokcer网络

>我们发现这个容器带来的网卡，都是一对对的。
>
>veth-pair 就是一堆的虚拟设备接口，他们都是成对出现的，一端连着协议，一段彼此相连。
>
>正因为有这个特性，veth-pair充当一个桥梁，连接各种虚拟网络设备
>
>Docker容器之间的连接，OVS，OpenStac的连接都是使用veth-pair技术

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200814101617604.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbmppYW5oYWk=,size_16,color_FFFFFF,t_70#pic_center)



![img](https://img-blog.csdnimg.cn/20200814102155735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZhbmppYW5oYWk=,size_16,color_FFFFFF,t_70#pic_center)

结论：容器之间通过ip地址是可以相互ping通的。tomcat01和tomcat02是共用的一个路由器，docker0

所有容器不指定网络的情况下，都是docker0路由的，docker会给我们的容器分配一个默认可用的IP。

只要容器删除，对应的网桥一对就没有了。

## Link

>思考一个场景，我们编写另一个微服务，database url=ip；如果项目不重启，数据库ip换掉了，我们希望可以处理这个问题，可以按名字来进行访问容器。
>
>本质探究：--link就是我们在hosts配置中增加了一个id配置。不过我们现在玩Docker已经不建议使用--link了！
>
>我们需要使用的是自定义网络，而不是使用Docker0, 因为它不支持容器名链接访问。

## 自定义网络

Docker 容器互联是指在 Docker 环境中，多个容器之间通过网络进行通信和数据交换的能力。它允许不同的容器相互连接和交互，使得分布式应用和微服务架构更加容易实现。

```
docker network ls
# 查看所有的docker网络
```

网络模式：

- bridge 桥接模式，桥接网络是 Docker 的默认网络模式，也就是上面的docker0，当 Docker 在宿主机上安装时，它会自动创建一个名为 `docker0` 的虚拟网桥（virtual bridge），它**为容器之间的通信提供了一个虚拟网络环境**。通过这个桥接网络，**Docker 容器可以相互通信**并与宿主机进行数据交换。每次启动一个容器，Docker 都会在这个桥接网络上分配一个 IP 地址给容器。不同容器可以通过这个网络相互通信。
- none  就是不配置网络，一般不会用。
- host 和宿主机共享网络：在主机网络模式下，容器共享宿主机的网络命名空间，直接使用宿主机的 IP 地址和端口。这种模式适用于需要高性能网络通信的场景，但会带来端口冲突问题。
- container 容器内网络联通，用的少，局限很大。

```
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
```

### 命令解释

- `docker network create`: 创建一个新的 Docker 网络。
- `--driver bridge`: 指定网络驱动类型为桥接（默认类型）。
- `--subnet 192.168.0.0/16`: 指定新的docker网络的子网范围。
- `--gateway 192.168.0.1`: 指定网络的网关。
- `mynet`: 自定义网络的名称

假设我们有一个子网 `192.168.0.0/16`，在这个子网内，子网是一个 IP 地址范围，表示在这个网络中可用的 IP 地址集合：

- **网络地址（192.168.0.0）**：标识整个子网，相当于小区的名字“绿色花园小区”。
- **广播地址（192.168.255.255）**：用于向子网内所有设备发送广播消息，相当于小区的公告板或喇叭广播。
- **网关地址（192.168.0.1）**：子网内设备与外部通信的出入口，相当于小区的大门。
- **普通IP地址（192.168.0.2）**：分配给网络中的某个设备，相当于小区中的2号房，住着某一户家庭。

### 验证网络配置

创建网络后，可以使用以下命令查看新创建的网络配置：

```
docker network inspect mynet
```

个命令将显示有关 `mynet` 网络的详细信息，包括其子网、网关、连接的容器等。

### 使用自定义网络启动容器

你可以使用这个自定义网络启动容器：

```
bash
docker run -d --name container1 --network mynet busybox top
docker run -d --name container2 --network mynet busybox top
```

启动后，这些容器将连接到 `mynet` 网络，并根据网络配置获取 IP 地址。

| 特性         | `docker0` 默认网络         | 自定义网络                         |
| ------------ | -------------------------- | ---------------------------------- |
| **创建**     | Docker安装时自动创建       | 用户手动创建                       |
| **子网范围** | 通常是 `172.17.0.0/16`     | 用户可自定义，如 `192.168.0.0/16`  |
| **网关地址** | 自动设置                   | 用户可自定义，如 `192.168.0.1`     |
| **隔离性**   | 所有容器共享同一个默认网络 | 不同自定义网络间隔离               |
| **灵活性**   | 配置固定                   | 可指定子网、网关、网络驱动等参数   |
| **DNS解析**  | 支持，通过容器名访问       | 支持，通过容器名访问               |
| **适用场景** | 简单应用和默认配置         | 复杂应用、分布式系统、网络隔离需求 |

## 网络联通

在使用 Docker 时，如果你需要让一个容器能够与其他网络上的容器进行通信，可以使用 `docker network connect` 命令来将容器连接到一个或多个网络。

![](C:\Users\simon.cheng\Documents\ReviewCenter\Docker\Docker-1\containernet.png)

# Docker Compose



# Docker Swarm



# CI/CD 之Jenkins



## Docker Compose & Kubernetes

Docker Compose 和 Kubernetes 是用于管理容器化应用程序的两种不同的工具/平台。

- Docker Compose 主要用于本地开发和测试环境，是一个简单的工具，适用于单机环境下少量容器的管理。
- Kubernetes 更适用于大规模容器集群的管理，具有高可用性、弹性伸缩、自动化部署等强大功能，适合于生产环境中的复杂应用。

在某些情况下，可以将 Docker Compose 结合 Kubernetes 使用，例如可以在开发阶段使用 Docker Compose 进行本地开发和测试，然后在部署到生产环境时使用 Kubernetes 进行容器编排和管理。