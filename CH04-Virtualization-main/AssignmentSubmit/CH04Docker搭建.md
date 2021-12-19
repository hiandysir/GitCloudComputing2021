## Docker搭建

作者：陈淇舒

系统环境

```
硬件环境
   内存 不小于8G
   硬盘空间 不小于50G
软件环境
 Ubuntu Bionic 18.04 (LTS)
网络环境
  需要网络可以访问Google等
```

#### 安装步骤

1.安装依赖包

```
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-releas
```

2.添加Docker官方密钥

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

3.安装Docker

```
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

 4.验证安装

```
sudo docker run hello-world
```

 <img src="https://github.com/QSue/CC-BDA/raw/4f32b1d4e12fece18d868addfb0be9813ec98a13/CH4_1.png" alt="image-20211128204303490" style="zoom:200%;" />

​                               

 

 



 