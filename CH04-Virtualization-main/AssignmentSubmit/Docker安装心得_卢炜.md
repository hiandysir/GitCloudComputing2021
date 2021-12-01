# Docker安装心得

## 环境准备

内存不小于8G，硬盘空间不小于50G，软件环境为Ubuntu虚拟机，网络环境需要可以访问Google。

由于我的虚拟机是新开的，所以不用清除旧的安装环境。

## 安装过程

### 1.安装依赖包

```
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

### 2.添加Docker官方钥匙

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

### 3.设置安装稳定版Docker

```
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 4.安装Docker

```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

### 5.安装完成

```
sudo docker run hello-world
```

## 总结

容器是一个允许我们在资源隔离的过程中，运行应用程序和其依赖项的，轻量的容器技术。而且每套容器都拥有自己的隔离化用户空间，从而使多套容器能够运行在同一主机系统上。

安装Docker的过程并不是很复杂，但是在后续的使用过程中，如何使用容器来实现项目才是最重要的。