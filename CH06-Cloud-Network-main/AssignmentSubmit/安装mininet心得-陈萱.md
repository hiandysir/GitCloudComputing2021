### 安装mininet心得

#### 什么是mininet？

​		Mininet是由一些虚拟的终端节点（end-hosts）、交换机、路由器连接而成的一个网络仿真器，它采用轻量级的虚拟化技术使得系统可以和真实网络相媲美。

​		Mininet可以很方便地创建一个支持SDN的网络：host就像真实的电脑一样工作，可以使用ssh登录，启动应用程序，程序可以向以太网端口发送数 据包，数据包会被交换机、路由器接收并处理。有了这个网络，就可以灵活地为网络添加新的功能并进行相关测试，然后轻松部署到真实的硬件环境中。

####  安装方式

##### 一．VM镜像安装

\1. 官网下载VM镜像

windows用户下载[Mininet 2.2.1 on Ubuntu 14.04 - 32 bit] (http://downloads.mininet.org/mininet-2.2.1-150420-ubuntu-14.04-server-i386.zip)镜像，并利用VirtualBox进行导入安装。

\2. 导入镜像

创建虚拟机后，导入ovf文件。

 

#####  二.从git clone 下载mininet

\1. 运行命令git clone git://github.com/mininet/mininet.git

\2. 可以选择mininet的版本

首先进入mininet目录下，运行

$git tag

$git checkout -b 2.2.1 2.2.1

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wps9703.tmp.jpg) 

 

\3. 运行$mininet/util/install.sh -n3V 2.5.0

出现以下报错：

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wps9704.tmp.jpg) 

根据查找解决办法，在/mininet/util/install.sh的文件下，把iproute换成iproute2，即可解决，但我并没有在此文件下查找到iproute词条。

又运行apt-get install iproute，更新环境之后，再输入$mininet/util/install.sh -n3V 2.5.0还是会报错。



使用命令install.sh -h查看一下其他的options

![image-20211216153644051](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211216153644051.png)

我选择使用![image-20211216153853540](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211216153853540.png)

安装成功后，通过简单的命令测试mininet的基本功能。

![image-20211216153955288](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211216153955288.png)

启动OVS：

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wps9705.tmp.jpg) 

