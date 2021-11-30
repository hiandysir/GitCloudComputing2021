# Mininet网络拓扑实验

![](https://img2020.cnblogs.com/blog/2457032/202107/2457032-20210710115128472-1303898580.png)

[SDN开发环境搭建以及Mininet编程 - Jcpeng_std - 博客园 (cnblogs.com)](https://www.cnblogs.com/JCpeng/p/14993506.html)

## 1.安装pip

```
sudo apt install python-pip
pip install ryu
```

### 出现错误

解决方法：

执行sudo apt-get install python3-pip
3.1 若报 No module named ‘distutils.util’,执行 sudo apt-get install python3-distutils
3.2 报E: Package python3-distutils has no installation candidate,执行: sudo apt update
3.3 重新执行:sudo apt-get install python3-distutils
3.4 执行:sudo apt-get install pyton3-pip
3.5 pip3 -V 成功!!

```
sudo apt update
执行:sudo apt-get install pyton3-pip
```

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222107358.png)

###  2.pip3 -V 成功!

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222120660.png)

### 3.pip install ryu成功

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222129128.png)

### 4.Open vSwitch源代码的规范位置是它的Git存储库，您可以将其克隆到名为“ovs”的目录中：

```
sudo git clone https://github.com/openvswitch/ovs.git
```

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222131929.png)

## 2.搭建网络环境

### 1.启动ryu控制器

```asp
ryu-manager --verbose ryu.app.simple_switch_13
```

### 出现错误

解决方法：

```
sudo apt install python3-ryu
```

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222141162.png)

### 启动成功

启动成功后卡住，需要关闭控制台再打开

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222142519.png)

### 2.启动mininet搭建网络拓扑

#### 打开terminal后进入root用户

#### 搭建网络topo，由于所要求的网络拓扑结构为比较简单的线性拓扑，可以直接使用--topo linear，x构建，选用Open vSwitch交换机：--switch ovsk。启动mininet：

```
mn --topo linear,2 --mac --switch ovsk --controller remote
```

#### 出现错误

解决办法：

回过头来看所作的两个操作，我们需要注意到，创建拓扑时，指向的remote控制器在尝试连接了127.0.0.1：6653和127.0.0.1：6633都未连接上控制器（controller还未启动）后，最终还是指向了前者（即端口号为6653）；但是，我们的pox控制器默认是工作在localhost（即127.0.0.1：6633）；于是两个端口号不一致，自然无法将交换机和控制器连接上。

发现问题所在后，在启动mininet时，可以指定remote控制器工作在localhost，
$ sudo mn --topo single,3 --mac --switch ovsk --controller remote,ip=127.0.0.1,port=6633
————————————————
版权声明：本文为CSDN博主「xiaobpeter」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/xiaobpeter/article/details/51030085



#### 但是我并没有解决这个问题，百度了几个办法也还是没有解决

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222214354.png)

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222215797.png)