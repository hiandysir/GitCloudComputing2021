

#                               Mininet 部署过程：

* 参考资料 https://www.cnblogs.com/JCpeng/p/14993506.html
* 参考资料中有少许错误，已经在本文中修改过来
* 部署环境：ubuntu-20.x

## 一、网络拓扑

搭建如下网络

![img](https://img2020.cnblogs.com/blog/2457032/202107/2457032-20210710115128472-1303898580.png)



## 二、准备阶段

## 2.1、git

使用以下指令安装git

```shell
sudo apt-get update
sudo apt install git
```

## 2.2、github获取Mininet源代码并安装

更新apt索引并安装镜像（内地的小伙伴可以在虚拟机中使用astrill先翻墙，再下载）

```shell
git clone http://github.com/mininet/mininet.git
```

安装Mininet（安装耗时十五分钟左右）

```bash
sudo ./mininet/util/install.sh -a
```

最后出现‘’Enjoy Mininet!‘’即为安装成功

安装完成后，测试基本的Mininet功能：(不安装net-tools会提示找不到所需要的可执行的ifconfig)

```
sudo apt install net-tools
sudo mn --test pingall
```

会出现如下图所示（因为本地图片会丢失路径，所以此处借用网络图片，但不影响展示效果）

![img](https://img2020.cnblogs.com/blog/2457032/202107/2457032-20210710113453060-1476223005.png)

## 2.3安装ryu

使用pip命令安装：

```
sudo apt install python3-pip
pip install ryu
```

![img](https://img2020.cnblogs.com/blog/2457032/202107/2457032-20210710113527559-504064287.png)

## 2.4 获取Open vSwitch源代码

Open vSwitch源代码的规范位置是它的Git存储库，您可以将其克隆到名为“ovs”的目录中：

```
sudo git clone https://github.com/openvswitch/ovs
```

## 三、搭建网络环境

## 3.1 启动ryu控制器

```
sudo apt install python3-ryu
ryu-manager --verbose ryu.app.simple_switch_13
```

## 3.2 启动mininet搭建网络拓扑

搭建网络topo，由于所要求的网络拓扑结构为比较简单的线性拓扑，可以直接使用--topo linear，x构建，选用Open vSwitch交换机：--switch ovsk。启动mininet。

在新建终端中输入如下指令：

```
sudo mn --topo linear,2 --mac --switch ovsk --controller remote
```

![img](https://img2020.cnblogs.com/blog/2457032/202107/2457032-20210710113716860-1707775693.png)

这时候就可以看到mininet已经连接到ryu控制器了。

![img](https://img2020.cnblogs.com/blog/2457032/202107/2457032-20210710113741126-517273242.png)

至此，所要求的网络环境已经搭建完成，对其进行简单的测试，在mininet的控制命令行里执行pingall：

![img](https://img2020.cnblogs.com/blog/2457032/202107/2457032-20210710113801064-1611059253.png)
