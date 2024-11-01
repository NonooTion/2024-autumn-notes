# Docker入门

## Docker 基础命令

实验：启动一个nginx，将它的页面改为自己的页面并发布

步骤：下载镜像->启动容器->修改页面->保存镜像->分享社区

### 下载镜像

**检索:docker search**

使用docker search命令 搜索nginx镜像

![](image/Docker%E5%85%A5%E9%97%A8/docker%20search.png)

搜索结果中，NAME表示镜像名，DESCRIPTION是镜像的描述,OFFICIAL表示镜像是官方的

**下载：docker pull**

使用docker pull命令 可以下载对应镜像

![](image/Docker%E5%85%A5%E9%97%A8/docker%20pull.png)

> docker pull nginx 其实等价于 docker pull nginx:latest 下载最新版本
>
> 如果像下载特殊版本的镜像，推荐不使用search命令，而是到docker hub 官网搜索镜像名 查看对应版本号

**列表: docker images**

使用docker images命令，可以列出已经下载的镜像列表

![](image/Docker%E5%85%A5%E9%97%A8/docker%20images.png)

REPOSITROTY：镜像名

TAG：镜像版本

IMAGE ID：镜像ID

CREATED：镜像创建时间

SIZE：镜像的大小

**删除：docker rmi**

使用docker rmi命令可以删除镜像

比如

```
docker rmi nginx:latest
```



### 启动容器

**运行：docker run**

```
Usage: docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

[]表示可选内容，一般COMMAND 和ARGS...可以忽略，除非想要修改镜像的初始启动方法

[OPTIONS] 填写启动镜像的一些参数设置

![](image/Docker%E5%85%A5%E9%97%A8/docker%20run.png)

这个命令会阻塞控制台



**查看：docker ps**

![](image/Docker%E5%85%A5%E9%97%A8/docker%20ps.png)

docker ps命令可以查看运行中的应用

CONTAINER ID: 运行中镜像的唯一ID

IMAGE:镜像名

COMMAND: 启动命令

CREATED: 多久之前启动的

STATUS：Up代表上线成功

PORTS: 代表应用所占的端口为80端口

NAMES:应用容器的名字



使用docker ps -a可以查看所有容器，包括停止的容器

![](image/Docker%E5%85%A5%E9%97%A8/docker%20ps%20-a.png)

**停止:docker stop**

docker stop可以停止应用运行

![](image/Docker%E5%85%A5%E9%97%A8/docker%20stop.png)

后面的参数可以是CONTAINER ID也可以是NAMES

**启动:docker start**

使用docker start命令，可以启动停止的容器

![](image/Docker%E5%85%A5%E9%97%A8/docker%20start.png)

后面的参数可以是CONTAINER ID也可以是NAMES

**重启:docker restart**

docker restart命令可以重启应用

**状态:docker stats**

docker stats可以展示应用所占的资源信息

![](image/Docker%E5%85%A5%E9%97%A8/docker%20stats.png)

**日志:docker logs**

docker logs可以查看某个应用运行中产生的日志



**删除:docker rm**

使用docker rm 可以删除停止运行的应用

![](image/Docker%E5%85%A5%E9%97%A8/docker%20rm.png)





**docker run 的细节**

![](image/Docker%E5%85%A5%E9%97%A8/docker%20run%20-d%20--name.png)

参数解析：

-d 后台启动容器

--name 给容器名字

![](image/Docker%E5%85%A5%E9%97%A8/docker%20run%20-p.png)

-p 端口映射 

此时可以通过主机浏览器访问虚拟机88端口来访问容器

![](image/Docker%E5%85%A5%E9%97%A8/%E8%AE%BF%E9%97%AE%E5%AE%B9%E5%99%A8.png)

**进入:docker exec**

![](image/Docker%E5%85%A5%E9%97%A8/docker%20exec.png)

-it 交互模式

/bin/bash 使用控制台模式交互



![](image/Docker%E5%85%A5%E9%97%A8/niginx-%E4%BF%AE%E6%94%B9html.png)

我们进入容器后，cd到指定文件目录(可以到官网查)，修改nginx的index.html

由于容器及其轻量级，以至于没有vi命令

我们使用echo 插入一个h1标签到index.html中

修改后效果

![](image/Docker%E5%85%A5%E9%97%A8/index.html%E4%BF%AE%E6%94%B9%E5%90%8E.png)

exit可以退出该容器

### 保存镜像

**提交: docker commit**

可以从容器的改变里创建一个新的镜像

```
Usage:  docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
```

Options:

-a 作者

-c 有哪些改变的列表

-m 本次改变的消息

-p 提交期间暂停容器运行

![](image/Docker%E5%85%A5%E9%97%A8/docker%20commit.png)

提交后，使用docker images命令，可以看到已经有一个mynginx

**保存:docker save**

保存一个或多个镜像为tar文件

```
Usage:  docker save [OPTIONS] IMAGE [IMAGE...]
```

Options:

-o 文件名称

![](image/Docker%E5%85%A5%E9%97%A8/docker%20save.png)

**加载:docker load**

从一个tar包或者STDIN中加载一个镜像

```
Usage:  docker load [OPTIONS]
```

Options:

-i 指定压缩包

-q 

![](image/Docker%E5%85%A5%E9%97%A8/docker%20laod.png)

load后使用docker run 命令即可运行容器

![](image/Docker%E5%85%A5%E9%97%A8/docker%20load%20%E5%90%8E%E8%BF%90%E8%A1%8C.png)



### 分享社区

**登录：docker login**

注册一个[docker hub](https://app.docker.com/)账号，并在命令行使用docker login 命令登录

![](image/Docker%E5%85%A5%E9%97%A8/docker%20login.png)

**命名：docker tag**

从源镜像创建一个新的目标镜像

```
Usage:  docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

![](image/Docker%E5%85%A5%E9%97%A8/docker%20tag.png)

虽然镜像名和标签不同，但是镜像ID是相同的

**推送：docker push**

使用docker push命令 即可将镜像推送至docker hub 之后便可以使用docker pull命令拉取

![](image/Docker%E5%85%A5%E9%97%A8/docker%20push.png)

![](image/Docker%E5%85%A5%E9%97%A8/docker%20hub.png)



## Docker 存储

熟悉目录挂载、数据卷，让容器数据不再丢失

问题：直接进入docker容器中修改数据不容易，数据也容易丢失



### 目录挂载

![](image/Docker%E5%85%A5%E9%97%A8/%E7%9B%AE%E5%BD%95%E6%8C%82%E8%BD%BD.png)

使用-v命令进行目录挂载

```
docker run -d -p 88:80 -v /app/nghtml:/usr/share/nginx/html --name app01 nginx
//将容器目录挂载到本地的/app/nghtml
```

![](image/Docker%E5%85%A5%E9%97%A8/%E7%9B%AE%E5%BD%95%E6%8C%82%E8%BD%BD-1.png)

创建index.html文件，就可以直接在主机目录下进行修改,也不用担心数据丢失



### 卷映射

使用目录挂载，我们开始时缺少初始文件，这时我们就需要用到卷映射

![](image/Docker%E5%85%A5%E9%97%A8/%E5%8D%B7%E6%98%A0%E5%B0%84-1.png)

```
docker run -d -p 88:80 -v /app/nghtml:/usr/share/nginx/html -v ngconf:/etc/nginx --name app03 nginx
```

docker 将卷统一放在了 /var/lib/docker/volumes/<volume-name>目录下



## Docker 网络

掌握网络机制，构建集群

### 自定义网络

docker多个容器间的互相访问，可以通过映射端口号来进行

![](image/Docker%E5%85%A5%E9%97%A8/docker%20%E7%BD%91%E7%BB%9C%20%E9%80%9A%E8%BF%87%E7%AB%AF%E5%8F%A3%E5%8F%B7%E8%AE%BF%E9%97%AE.png)

这显然不够方便快捷

docker有一个docker0网络地址，我们可以通过ip addr 命令查看

![](image/Docker%E5%85%A5%E9%97%A8/docker%200.png)

docker 为每个容器都分配了不同的ip

![](image/Docker%E5%85%A5%E9%97%A8/docker%20%E5%AD%90%E7%BD%91.png)

我们可以通过这个ip地址来访问

![](image/Docker%E5%85%A5%E9%97%A8/docker%20%E7%BD%91%E7%BB%9C%20%E9%80%9A%E8%BF%87ip%E5%9C%B0%E5%9D%80%E8%AE%BF%E9%97%AE.png)

可以使用任意容器ip+容器端口相互访问

但是我们又有新的问题: ip地址改变怎么办

解决的方法：自定义网络

自定义网络中，容器名就是稳定域名

可以通过域名来访问容器



**docker network**

![](image/Docker%E5%85%A5%E9%97%A8/docker%20network.png)

通过docker network指令，可以创建网络(create)，查看网络(ls),查看网络细节(inspect)等等

```
docker network create mynet
```

创建一个名为mynet的自定义网络

我们在创建容器时，使用--network参数即可使容器加入自定义网络

```
docker run -d -p 81:80 --name app1 --network mynet nginx
```

我们创建两个容器 app1 app2，并将两个容器都加入自定义网络

同时进入第二个容器访问第一个容器

![](image/Docker%E5%85%A5%E9%97%A8/app2%E8%AE%BF%E9%97%AEapp1.png)

也可以通过app2:80来访问

![](image/Docker%E5%85%A5%E9%97%A8/app1%E8%AE%BF%E9%97%AEapp2%20%E5%9F%9F%E5%90%8D.png)

## Docker Compose

批量管理容器

### Docker Compose语法

**顶级元素**

name：名字

services：服务

networks：网络

volumes：卷

configs：配置

secrets：密钥



根据官方文档填写 yaml文件

使用 docker compose -f compose.yaml up -d可以后台批量启动容器



## Dockerfile

构建自定义镜像

