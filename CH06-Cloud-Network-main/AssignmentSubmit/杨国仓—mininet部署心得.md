# Mininet安装部署心得





## 1. 什么是Mininet

​    Mininet是由一些虚拟的终端节点（end-hosts）、交换机、路由器连接而成的一个网络仿真器，它采用轻量级的虚拟化技术使得系统可以和真实网络相媲美。

​    Mininet可以很方便地创建一个支持SDN的网络：host就像真实的电脑一样工作，可以使用ssh登录，启动应用程序，程序可以向以太网端口发送数 据包，数据包会被交换机、路由器接收并处理。有了这个网络，就可以灵活地为网络添加新的功能并进行相关测试，然后轻松部署到真实的硬件环境中。



## 2. Mininet的特性

- ​    可以简单、迅速地创建一个支持用户自定义的网络拓扑，缩短开发测试周期
- ​    可以运行真实的程序，在Linux上运行的程序基本上可以都可以在Mininet上运行，如Wireshark
- ​    Mininet支持Openflow，在Mininet上运行的代码可以轻松移植到支持OpenFlow的硬件设备上
- ​    Mininet可以在自己的电脑，或服务器，或虚拟机，或者云（例如Amazon EC2）上运行
- ​    Mininet提供python API，简单易用



## 3. 安装mininet

### 3.1 从Github上获取mininet源码

```
root@ubuntu:~# git clone git://github.com/mininet/mininet
Cloning into 'mininet'...
remote: Enumerating objects: 10182, done.
remote: Counting objects: 100% (28/28), done.
remote: Compressing objects: 100% (19/19), done.
remote: Total 10182 (delta 9), reused 22 (delta 8), pack-reused 10154
Receiving objects: 100% (10182/10182), 3.22 MiB | 83.00 KiB/s, done.
Resolving deltas: 100% (6791/6791), done.

```



### 3.2 安装mininet

```
cd minine/util
sudo ./install.sh -a

.........    # 安装过程中会打印很多日志 此处略  安装完成后会显示  Enjoy Mininet!
Making install in doc
make[1]: Entering directory '/root/oflops/doc'
make[1]: Nothing to be done for 'install'.
make[1]: Leaving directory '/root/oflops/doc'
Enjoy Mininet!

```



### 3.3 测试安装结果

```
root@ubuntu:~/mininet/util# sudo mn --test pingall     #无异常报错则可以认为安装成功
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
*** Waiting for switches to connect
s1 
*** Ping: testing ping reachability
h1 -> h2 
h2 -> h1 
*** Results: 0% dropped (2/2 received)
*** Stopping 1 controllers
c0 
*** Stopping 2 links
..
*** Stopping 1 switches
s1 
*** Stopping 2 hosts
h1 h2 
*** Done
completed in 0.530 seconds
```

