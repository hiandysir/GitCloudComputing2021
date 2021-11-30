# 安装Docker

先根据以下代码安装curl

```
sudo apt-get update
sudo apt-get install 
```

添加使用HTTPS传输的软件包以及CA证书

```
sudo apt-get update
sudo apt-get install
```

向source.list中添加Docker软件源

```
sudo add-apt-repository
(lsb_release -cs)
```

更新apt软件包缓存，并安装Docker-ce

```
sudo apt-get update
sudo apt-get install docker-ce
```

测试Docker是否安装正确

```
docker run hello-world
```

若能正常输出以下信息，则说明安装成功

```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete                                                                                  Digest: sha256:c3b4ada4687bbaa170745b3e4dd8ac3f194ca95b2d0518b417fb47e5879d9b5f
Status: Downloaded newer image for hello-world:latest
Hello from Docker!
This message shows that your installation appears to be working correctly.
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash
Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/
For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## 拉取镜像，运行Doctor容器

首先在命令中执行以下命令

```
docker pull alpine
```

利用docker image来查看本地已有的镜像文件

```
docker image
```

但是这里报错了。错误提示：尝试连接到Docker守护进程时权限被拒绝。

百度解决方法：创建泊坞组 ,将用户添加到泊坞组 ，注销并再次登录运行。



现在已经有了alpine的镜像，执行docker run命令来运行容器

```
docker run alpine ls -l
```

查看主机上的所有容器

```
docker ps -a
```

