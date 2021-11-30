# Ceph安装心得

由于在书本上看到安装Ceph需要四个虚拟机的时候，我就觉得我的电脑应该带不动，所以一直没安装，直到这段时间同学说ceph安装单机版也可以，于是我也开始了尝试。

## 安装教程

单机版Ceph环境部署，Linux平台

[单机版Ceph环境部署，Linux平台 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/67832892)

## 更换ubuntu版本

```
由于ubuntu20版本与要求不一致，所以我下载了ubuntu18进行ceph的搭建。
```

## 安装步骤

### 添加安装源

对于Ubuntu系统**需要添加安装源**，如果使用系统默认的安装源ceph-deploy版本太低，后续操作会有问题。

```
wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add -
echo deb https://download.ceph.com/debian-{ceph-stable-release}/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list
```

安装L版本则需要替换成字符串`luminous`

```
echo deb https://download.ceph.com/debian-luminous/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list
```

### 安装ceph-deploy

```
sudo apt update
sudo apt -y install ceph-deploy
```

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/image-20211121225810494.png)

### 出现错误

由于贪图方便克隆了一个以前的虚拟机，但是由于是ubuntu20版本的所以出错了。

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/image-20211121230226846.png)

### 改正错误

改为ubuntu18后安装就成功了。

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/image-20211121233644592.png)

## 安装ceph软件包

```
mkdir myceph
cd myceph
ceph-deploy new ubuntu
```

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/image-20211121234109888.png)

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/image-20211122000451652.png)

```
原因
　　Ubuntu有一个让人头痛的特性，就是在/etc/hosts配置文件中，让hostname使用了它的回环loopback地址。这个特性使得很多服务无法检测到真正的地址，这里，ceph-deploy中，ceph_deploy.util.get_nonlocal_ip获取到的是127网段的地址，然后就报错不能解析hostname了。

解决方法
　　在/etc/hosts中，把回环地址对应的hostname给删除掉。再添加一行真正的ip地址和hostname的对应关系，即可。
```



![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/image-20211122003843262.png)

然后使用命令ls查看myceph文件夹里有ceph.conf

```
gedit /ect/ceph.conf
```

加入以下内容

```
[global]
osd pool default size = 1
osd pool default min size = 1
```

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/image-20211122002649967.png)

### 安装 ceph 软件

```
ceph-deploy install --release luminous ubuntu
```

### 初始化mon

```
ceph-deploy mon create-initial
ceph-deploy admin ubuntu
```

启动Monitor节点后Ceph集群就可以访问了，通过`ceph -s`命令可以查看集群的状态。