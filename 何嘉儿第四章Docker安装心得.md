# 第四章Docker安装心得

## 搭建过程

### 卸载旧版本—下载安装包—设置仓库-下载最新版本

```csharp
[root@bogon ~]# yum remove docker \
        docker-client \
        docker-client-latest \
        docker-common \
        docker-latest \
        docker-latest-logrotate \
        docker-logrotate \
        docker-engine
```

```csharp
[root@bogon ~]# yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

```csharp
[root@bogon ~]# yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

```csharp
[root@bogon ~]# yum install docker-ce docker-ce-cli containerd.io
```

### 安装完后启动

```csharp
[root@bogon ~]# systemctl start docker
```

### 验证

```csharp
[root@bogon ~]# docker run hello-world
```

## 容器的使用

### 启动一个容器

```csharp
[root@bogon ~]# docker run -it ubuntu /bin/bash
```

#### 如果需要后台运行可加上-d参数

### 查看所有容器

```ruby
[root@bogon ~]# docker ps -a
```

### 进入和退出在后台运行的容器

```csharp
-- 进入容器
[root@bogon ~]# docker attach <容器ID>   
-- 退出容器且不关闭
[root@bogon ~]# docker exec 
```

##### 可以通过 -p 参数来配置端口；查看web应用程序容器的进程；使用docker inspect来检查docker底层信息，它会返回一个json文件记录着容器的配置和状态信息



## 镜像的使用

### 列出所有本地主机上的镜像

```csharp
[root@bogon ~]# docker images
```

### 获取新镜像

```css
[root@bogon ~]# docker pull REPOSITORY:TAG
如：docker pull ubuntu:13.10
```

### 查找镜像

#### 在docker hub网站上找:[https://hub.docker.com/](https://links.jianshu.com/go?to=https%3A%2F%2Fhub.docker.com%2F)

### 拖取镜像

```csharp
-- 拖取httpd镜像
[root@bogon ~]# docker pull httpd
```

### 删除镜像

```csharp
-- 删除httpd镜像
[root@bogon ~]# docker rmi httpd
```

### 构建镜像

```csharp
-- 创建docker使用的目录来规划存放dockerfile文件的目录（此步可忽略）
[root@bogon ~]# mkdir /home/docker
[root@bogon ~]# cd /home/docker
-- 创建文件需要注意的是：D需要大写，当我们构建镜像的时候docker默认选取当前目录下的Dockerfile文件
[root@bogon ~]# vim DockerFile
```

### 创建镜像

#### 当docker镜像库中的镜像不能满足我们的需求时，我们可以通过两种方式对镜像进行修改：
1、从已经创建的容器中更新镜像，并提交这个镜像
2、使用dockerfile指令来创建一个新的镜像

### docker容器互联

```csharp
-- 创建新的docker网络，其中-d参数指定docker网络类型，可以是bridge、overlay
[root@bogon docker]# docker network create -d bridge test-net
-- 查看当前docker网络
[root@bogon docker]# docker network ls
```

### 仓库管理

#### docker仓库存放docker镜像的仓库，Docker Hub



