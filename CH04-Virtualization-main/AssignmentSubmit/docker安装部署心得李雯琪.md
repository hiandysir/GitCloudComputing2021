# docker部署心得



[TOC]

## 1. 概述

​	Docker 是一个开源的应用容器引擎，基于 [Go 语言](https://www.runoob.com/go/go-tutorial.html) 并遵从 Apache2.0 协议开源。Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app）,更重要的是容器性能开销极低。



## 2. 安装docker

CentOS 7（使用 yum 进行安装）



- 如果安装过请先卸载

```
yum remove docker \
           docker-client \
           docker-client-latest \
           docker-common \
           docker-latest \
           docker-latest-logrotate \
           docker-logrotate \
           docker-engine
```



- step 1: 安装必要的一些系统工具

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

- Step 2: 添加软件源信息

```
sudo yum-config-manager --add-repo https:*//mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo*
```

- Step 3: 更新并安装Docker-CE

```
sudo yum makecache fast
sudo yum -y install docker-ce
```

- Step 4: 开启Docker服务

```
sudo service docker start
```

- 验证是否安装成功

```
docker version
```

- 运行测试容器

```
docker run hello-world
```





## 3. 安装 Docker Compose



[Docker Compose ](https://docs.docker.com/compose/install/) 存放在Git Hub，不太稳定。 可以通过执行下面的命令，高速安装Docker Compose。

```
curl -L https://get.daocloud.io/docker/compose/releases/download/v2.1.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```



## 4. docker基本命令



docker ps 

通过此命令可以查看 正在运行的容器



docker pull [options] NAME[:TAG]

通过此命令可以docker远程仓库拉取镜像到本地.

name是拉取镜像的名称,:TAG表示是可选的,如果不选表明时latest,如果选择表明是指定版本的. options是拉去的一些参数.

当不加请求地址的时候回去docker的官网拉取镜像.



docker images [options] [REPOSITORY[:TAG]]

options是选项,后面是指定镜像的名称.这个用的不多,可能当本地镜像非常多的时候要指定查看某一个镜像.

IMAGE ID 其实是一个64位的字符串,它可以唯一标识我们的镜像,这里只显示了16位,后面的被截掉了.



docker run [options]  IMAGE[:TAG] [COMMAND] [ARG..]

IMAGE是镜像的名字

COMMAND是运行起来的时候要执行什么命令.

ARG表示这条命令运行需要的参数.
