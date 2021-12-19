# 安装OpenStack

作者：陈淇舒

### 第一种安装：ubuntu 上使用 devstack 安装 Openstack

安装虚拟机先配置好硬件部分

```
内存 不小于8G
硬盘空间 不小于50G （不然空间不够！！！）
```

按照官网教程（https://docs.openstack.org/devstack/latest/#create-a-local-conf）安装

1.开始时要先进行apt和pip下载源的配置：

```
$ sudo apt-get update
$ sudo apt-get upgrade
```

  更新git

```
$sudo apt install git`
```



2.配置网络（第二章心得有提及这里不重复）

 

3.创建一个新的stack用户，devstack的安装与运行都在该用户下进行

```
$ sudo useradd -s /bin/bash -d /opt/stack -m stack

$ sudo passwd stack #编辑stack用户的登录密码

$ echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack #为用户分配sudo权限
```



4.下载最新版的devstack并设置权限

```
$ cd /opt/stack/  #进入此目录下载
$ git clone https://git.openstack.org/openstack-dev/devstack
```

设置用户权限

```
$ chown -R stack:stack /opt/stack/devstack
```

不然会出现以下错误： 

```
cannot create directory'文件':Permission denied
```



5.切stack用户并进入/opt/stack/devstack

```
$ cd devstack
```



6.修改文件配置，使用`vi local.conf`命令将其内容修改为

```
[[local|localrc]]
ADMIN_PASSWORD=123456
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
HOST_IP=192.168.174.104
```



7.运行 devstack 安装 openstack

```
FORCE=yes ./stack.sh
```

#### 运行结果失败

猜测会出现失败的理由是：

1.网络代理导致连接不到下载的服务器地址

2.可能版本冲突

 

### 【旧版成功】第二种安装：ubuntu 上使用 miss安装 Openstack

安装虚拟机先配置好硬件部分同第一种安装

本人安装是最老的版本才成功

```
sudo snap install microstack --classic --channel=rocky/edge
```

查看是否安装成功

```
snap list microstack
```

<img src="https://github.com/QSue/CC-BDA/raw/4bd874ed9a23f29d88c6952518c06aa9e84421ce/CH3_1.png" alt="image-20211128175529374" style="zoom:200%;" />

 

```
 sudo microstack.init --auto 
```



登录openstack，浏览器输入ip地址登录

<img src="https://github.com/QSue/CC-BDA/raw/4bd874ed9a23f29d88c6952518c06aa9e84421ce/CH3_2.png" alt="image-20211128180122154" style="zoom:200%;" />

旧版本只需要输入

用户名：admin

密码：keystone 就可以浏览

<img src="https://github.com/QSue/CC-BDA/raw/4bd874ed9a23f29d88c6952518c06aa9e84421ce/CH3_3.png" alt="image-20211128180440292" style="zoom:200%;" />

