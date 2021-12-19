# Mininet心得：

### 一.Mininet是什么

#### Mininet是由斯坦福大学基于[Linux](https://so.csdn.net/so/search?from=pc_blog_highlight&q=Linux) Container架构开发的一个进程虚拟化网络仿真工具，可以创建一个包含主机，交换机，控制器和链路的虚拟网络，其交换机支持OpenFlow，具备高度灵活的自定义软件定义网络。

### 二.Mininet的主要特性

#### Mininet作为一个轻量级软定义网络研发和测试平台，其主要特性包括：

#### 1.支持OpenFlow、Open vSwitch等软定义网络部件；

#### 2.方便多人协同开发；

#### 3.支持系统级的还原测试；

#### 4.支持复杂拓扑、自定义拓扑；

#### 5.提供python API；

#### 6.很好的硬件移植性（Linux兼容），结果有更好的说服力；

#### 7.高扩展性，支持超过4096台主机的网络结构。

### 三.Ubuntu 下mininet使用源码安装

### 1.若之前安装过mininet,需要卸载再重新安装
### 卸载命令：

```
sudo rm -rf /usr/local/bin/mn /usr/local/bin/mnexec \
/usr/local/lib/python*/*/*mininet* \
/usr/local/bin/ovs-* /usr/local/sbin/ovs-*
```

```
sudo apt-get remove mininet
```

### 2.更新软件

```
 apt-get update
 apt-get upgrade
```

### 3.从github上获得源码

```
git clone git://github.com/mininet/mininet
```

### 4.获取完以后，查看当前获取的Mininet版本

```
cd mininet
cat INSTALL
```

### 5.安装Mininet

```
mininet/util/install.sh -a
```

### 6. 安装完成以后，通过简单的命令测试Mininet的基本功能

```
sudo mn --test pingall
```

