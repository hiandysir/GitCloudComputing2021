刘元坤

# Docker 搭建

## 环境准备

    硬件环境
        内存 不小于8G
        硬盘空间 不小于50G
    软件环境
        Ubuntu Groovy 20.10
        Ubuntu Focal 20.04 (LTS)
        Ubuntu Bionic 18.04 (LTS)
        Ubuntu Xenial 16.04 (LTS)
        VMWare Workstation、VBox 等虚拟化平台
    网络环境
        需要网络可以访问Google等

## 操作步骤

### 1、清除已安装环境(新系统可跳过)

``` shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

### 2、安装依赖包

``` shell
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

### 3、添加Docker官方密钥

``` shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

### 4、设置安装稳定版Docker

``` shell
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### 5、安装Docker

``` shell
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

### 6、验证安装

``` shell
sudo docker run hello-world
```

出现下图则为安装成功

[![c99XQS.png](https://z3.ax1x.com/2021/03/28/c99XQS.png)](https://imgtu.com/i/c99XQS)