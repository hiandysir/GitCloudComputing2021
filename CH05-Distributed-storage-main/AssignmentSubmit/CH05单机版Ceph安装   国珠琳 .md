# 单机版Ceph安装

先下载ubuntu18版本。下载地址：ubuntu-18.04.6-desktop-amd64.iso 

## 安装步骤

添加安装源

```
wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add -
echo deb https://download.ceph.com/debian-{ceph-stable-release}/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list
```

安装ceph-deploy

```
sudo apt update
sudo apt -y install ceph-deploy
```

##安装完成后会显示以下界面

![](https://raw.githubusercontent.com/kwokCL/picture/main/gzl/image-20211123204110991.png))

安装ceph软件包

```
mkdir myceph
cd myceph
ceph-deploy new ubuntu
```

使用命令ls查看myceph文件夹里有ceph.conf

```
gedit /ect/ceph.conf
```

加入以下内容

```
[global]
osd pool default size = 1
osd pool default min size = 1
```

安装ceph软件

```
ceph-deploy install --release luminous ubuntu
```

![](https://raw.githubusercontent.com/kwokCL/picture/main/gzl/image-20211123214258167.png))

初始化mon

```
ceph-deploy mon create-initial
ceph-deploy admin ubuntu
```

启动Monitor节点后Ceph集群就可以访问了，通过`ceph -s`命令可以查看集群的状态。

![](https://raw.githubusercontent.com/kwokCL/picture/main/gzl/image-20211123213951313.png))