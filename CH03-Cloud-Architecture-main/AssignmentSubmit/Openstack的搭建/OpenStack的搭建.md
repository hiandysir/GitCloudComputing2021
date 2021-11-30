# OpenStack的搭建

author:damon

OpenStack 在Ubuntu系统中搭建的教程在网络上很多：

>老师给的教程是关于microstack来搭建OpenStack,相较于DevStack安装显得极其简单，便于新手搭建。
>笔者也在私下搜了一些资料，也算是对老师的资料细节上一些补充。
>https://www.cnblogs.com/s6-b/p/14151205.htm
>由于安装过程简单，其中步骤省略，按着老师的教程走即可。

上次写的太简单了，这次把自己的代码源码粘上。

源码：

```shell
#开始的命令：
#依次运行下列命令
hadoop@ubuntu:~$ sudo apt update
[sudo] password for hadoop: 
Get:1 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]      
Hit:2 http://us.archive.ubuntu.com/ubuntu focal InRelease                      
Get:3 http://us.archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
.....
.....
.....

------------------------------------------------------------------------
#从beta 通道安装microstack
hadoop@ubuntu:~$ sudo snap install microstack --beta --devmode
确保 "microstack" 的先决条件可用                                                     确保 "microstack" 的先决条件可用                                                     确保 "microstack" 的先决条件可用                                                     确保 "microstack" 的先决条件可用  
.....
.....
.....


--------------------------------------------------------------------------
#初始化microstack
hadoop@ubuntu:~$ sudo microstack init --auto --control
2021-10-12 05:41:39,075 - microstack_init - INFO - Configuring clustering ...
2021-10-12 05:41:39,166 - microstack_init - INFO - Setting up as a control node.
2021-10-12 05:41:42,413 - microstack_init - INFO - Configuring networking ...
.....
.....
.....

----------------------------------------------------------------------------
hadoop@ubuntu:~$ sudo snap get microstack config.credentials.keystone-password
o4BgKIx9xRJgAA6D4I4OkpTzhC6gQkUF


----------------------------------------------------------------------------
hadoop@ubuntu:~$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
.....
.....
.....
--------------------------------------------------------------------------------

s





```

![image-20211020210811324](OpenStack的搭建.assets/image-20211020210811324.png)

