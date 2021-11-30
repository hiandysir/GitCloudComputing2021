# Mininet的搭建和学习笔记

author:Damon

参考资料：**Mininet**安裝教學

### 什么是Mininet

 Mininet是由一些虚拟的终端节点（end-hosts）、交换机、路由器连接而成的一个**网络仿真器**，它采用轻量级的虚拟化技术使得系统可以和真实网络相媲美。

什么是SDN,openflow

SDN（Software Defined Network）即软件定义网络，是一种网络设计理念，或者一种推倒重来的设计思想。只要网络硬件可以集中式软件管理，可编程化，控制转发层面分开，则可以认为这个网络是一个SDN网络。所以说，SDN并不是一个具体的技术，不是一个具体的协议，而是一个思想、一个框架。狭义的SDN是指的“软件定义网络”，广义的SDN的概念还延伸出了：软件定义安全、软件定义存储等等。可以说，SDN是一个浪潮，席卷整个IT产业。

*OpenFlow*，一种网络通信协议，属于数据链路层，能够控制网上交换器或路由器的转发平面（forwarding plane），借此改变网络数据包所走的网络路径。

### 基本概念

SDN Controller 控制器(c0)，

交换机 (s1和s2)，

主机 (h1和h2和h3)。

![image-20211111215657231](Mininet的搭建和学习笔记.assets/image-20211111215657231.png)

**本文是基于部署和一些实战的过程，目的是进一步的了解网络通信的过程。**

**安装过程也是十分顺利，重点是后面的两个实验，比如说，同一个交换机下，主机怎么连接，如何修改自身的网段等等**

### 下列是安装源码，仅供大家参考：

```shell
#基础环境————ubuntu 20.04.1
#安装git
damonpkl@damonpkl-VirtualBox:~$ sudo apt install git
[sudo] password for damonpkl: 
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following additional pa
...
#安装mininet
damonpkl@damonpkl-VirtualBox:~$ git clone git://github.com/mininet/mininet.git 
Cloning into 'mininet'...
remote: Enumerating objects: 10182, done.
remote: Counting objects: 100% (28/28), done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 10182 (delta 9), reused 16 (delta 6), pack-reused 10154
Receiving objects: 100% (10182/10182), 3.22 MiB | 2.42 MiB/s, done.
Resolving deltas: 100% (6791/6791), done.
#查看该文件的路径，目标进入到/mininet/util找到install.sh
damonpkl@damonpkl-VirtualBox:~$ cd mininet
damonpkl@damonpkl-VirtualBox:~/mininet$ ls 
bin           custom  doc       INSTALL  Makefile  mnexec.c   setup.py
CONTRIBUTORS  debian  examples  LICENSE  mininet   README.md  util
damonpkl@damonpkl-VirtualBox:~/mininet$ cd util
damonpkl@damonpkl-VirtualBox:~/mininet/util$ ls
build-ovs-packages.sh  install.sh   openflow-patches  versioncheck.py
clustersetup.sh        kbuild       sch_htb-ofbuf     vm
colorfilters           m            sysctl_addon
doxify.py              nox-patches  unpep8
damonpkl@damonpkl-VirtualBox:~/mininet/util$ sudo  ./install.sh

...
make[2]: Nothing to be done for 'install-data-am'.
make[2]: Leaving directory '/home/damonpkl/oflops/cbench'
make[1]: Leaving directory '/home/damonpkl/oflops/cbench'
Making install in doc
make[1]: Entering directory '/home/damonpkl/oflops/doc'
make[1]: Nothing to be done for 'install'.
make[1]: Leaving directory '/home/damonpkl/oflops/doc'
Enjoy Mininet!

damonpkl@damonpkl-VirtualBox:~/mininet/util$ sudo ./install.sh -a^C
damonpkl@damonpkl-VirtualBox:~/mininet/util$ sudo mn
[sudo] password for damonpkl: 
*** Creating network
*** Adding controller
*** Adding hosts:
h1 h2 
*** Adding switches:
s1 
*** Adding links:
(h1, s1) (h2, s1) 
*** Configuring hosts
h1 h2 
*** Starting controller
c0 
*** Starting 1 switches
s1 ...
*** Starting CLI:
mininet> 

#安装成功
```

