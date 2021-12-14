## Ceph的搭建和学习笔记

作者：陈淇舒

```
#创建三个用户，三台虚拟机
damon1  damon2  damon3
```

| 主机名称 | 主机IP         | 说明                                           |
| -------- | -------------- | ---------------------------------------------- |
| damon-1  | 192.168.10.100 | 容器主节点(Dashbaord、mon、mds、rgw、mgr、osd) |
| damon-2  | 192.168.10.101 | 容器子节点(mon、mds、rgw、mgr、osd)            |
| damon-3  | 192.168.10.102 | 容器子节点(mon、mds、rgw、mgr、osd)            |

配置三台CentOS7的静态ip网关（详细配置参考：https://blog.csdn.net/jingxin_123/article/details/105589679）

```
cd /etc/sysconfig/network-scripts
vi ifcfg-ens33
```

进入文件修改

```
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="static"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="yes"
IPV6_AUTOCONF="yes"
IPV6_DEFROUTE="yes"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="stable-privacy"
NAME="ens33"
UUID="7b6588f7-6142-4da9-9eaf-fe554c7264a7"
DEVICE="ens33"
ONBOOT="yes"
IPADDR=192.168.174.131
NETMASK=255.255.255.0
GATEWAY=192.168.174.2
DNS1=192.168.174.2
DNS2=8.8.8.8
```

使命令生效

```
service network restart
```

<img src="https://github.com/QSue/CC-BDA/raw/0b622276bc332fb15a1c1c1cd01a18f809fff9e3/CH5-1.png" alt="image-20211128220208210" style="zoom:200%;" />



#### 配置多台机器SSH相互通信信任

##### 3台机器执行 ssh-keygen

```
[root@damon-1 ~]# ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa): 
Created directory '/root/.ssh'.
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
The key fingerprint is:
1c:68:d2:13:01:e5:f0:36:30:bb:1a:72:09:6d:e1:45 root@sht-sgmhadoopnn-01.telenav.cn
The key's randomart image is:
+--[ RSA 2048]----+
|  ..Eo+.         |
| o o O o         |
|. + o X .        |
| o . = + .       |
|. + .   S        |
| o o             |
|  .              |
|                 |
|                 |
+-----------------+
```

##### 选取第一台,生成authorized_keys文件

```
[root@damon-1 ~]# cd ~/.ssh
[root@damon-1 .ssh]# cat /root/.ssh/id_rsa.pub>> /root/.ssh/authorized_keys
```

##### 然后将其他两台的id_rsa.pub内容,手动copy到第一台的authorized_keys文件

```
[root@damon-2 .ssh]# more id_rsa.pub
[root@damon-3 .ssh]# more id_rsa.pub

拷贝至authorized_keys文件(注意copy时,最好先放到记事本中,将回车去掉,成为一行)
[root@damon-1 .ssh]# vi  authorized_keys
```

##### 权限(每台机器)

```
chmod 700 -R ~/.ssh
chmod 600 ~/.ssh/authorized_keys 
```

##### 将第一台的authorized_keys scp 给其他两台(第一次传输,需要输入密码) 重复两次

```
[root@damon-1 .ssh]#  scp authorized_keys root@sht-sgmhadoopnn-02:/root/.ssh
root@damon-2's password: 
authorized_keys                                                            100% 2080     2.0KB/s   00:00    
```

##### 验证(每台机器上执行下面命令,只输入yes,不输入密码,则这3台互相通信了)

```
ssh root@damon-1 date
```



ssh报错

```
ERROR：ECDSA host key "ip地址" for  has changed and you have requested strict checking.
```

解决方案：

```
ssh-keygen -R "你的远程服务器ip地址"  
```

目的是清除你当前机器里关于你的远程服务器的缓存和公钥信息，注意是大写的字母“R”。

**原因分析**：云服务器重装了系统（清除了与我本地SSH连接协议相关信息），本地的SSH协议信息便失效了。SSH连接相同的ip地址时因有连接记录直接使用失效的协议信息去验证该ip服务器，所以会报错，使用上述命令便可以清除known_hosts里旧缓存文件。

**延伸：**远程服务器的ssh服务被卸载重装或ssh相关数据（协议信息）被删除也会导致这个错误。



改三台虚拟机名字

```
hostnamectl set-hostname
```

三台主机都得改配置解析

```
vim /etc/hosts
```



### 安装docker

三台主机同时以root身份依次在三台节点上创建/usr/local/ceph/{admin,data, etc,lib, logs}目录

```
[root@damon-1 ~]#  mkdir -p /usr/local/ceph/{admin,data,etc,lib,logs}
```

安装依赖包

```
[root@damon-1 ~]# yum -y install yum-utils device-mapper-persistent-data lvm2
```

此时，出现以下错误<img src="https://github.com/QSue/CC-BDA/raw/0b622276bc332fb15a1c1c1cd01a18f809fff9e3/CH5-2.png" alt="image-20211128220437031" style="zoom:200%;" />

这个错误的原因是因为： /var/run/yum.pid 已被锁定，PID 为 2931 的另一个程序正在运行，另外一个程序锁定了 yum。

```
[root@damon-1 ~]#rm -f /var/run/yum.pid 2931
```

然后重新输入安装依赖包命令

配置YUM源

```
[root@damon-1 ~]# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

安装Docker服务

```
[root@damon-1 ~]# yum -y install docker-ce-18.03.1.ce
```

启动服务设置开机启动和查看是否安装成功

```
#启动服务
[root@damon-1 ~]# systemctl start docker

#设置开机启动
[root@damon-1 ~]# systemctl enable docker

#查看docker是否成功
[root@damon-1 ~]# docker -v
Docker version 18.03.1-ce, build 9ee9f40
[root@damon-2 ~]# docker -v
Docker version 18.03.1-ce, build 9ee9f40
[root@damon-3 ~]# docker -v
Docker version 18.03.1-ce, build 9ee9f40
```

### 安装ceph

安装完docker,开始在docker下载ceph镜像--nautilus版本

```
#下载镜像
[root@damon-1 ~]# docker pull ceph/daemon:master-7ef46af-nautilus-centos-7-x86_64
```

修改标签

```
[root@damon-1 ~]# docker images
REPOSITORY          TAG                                       IMAGE ID            CREATED             SIZE
ceph/daemon         master-7ef46af-nautilus-centos-7-x86_64   6f68932df7dd        23 months ago       946MB
[root@damon-1 ~]# docker tag 6f68932df7dd ceph/daemon:latest
[root@damon-1 ~]# docker images
REPOSITORY          TAG                                       IMAGE ID            CREATED             SIZE
ceph/daemon         latest                                    6f68932df7dd        23 months ago       946MB
ceph/daemon         master-7ef46af-nautilus-centos-7-x86_64   6f68932df7dd        23 months ago       946MB
```

### 启动mon服务

在主节点的/usr/local/ceph/admin目录下创建start_mon.sh脚本

```
[root@damon-1 ~]# cd /usr/local/ceph/admin
[root@damon-1 admin]# ls
[root@damon-1 admin]# vim start_mon.sh 
```

在start_mon.sh文件里面修改：

```
docker run -d --net=host \
    --name=mon \
    -v /etc/localtime:/etc/localtime \
    -v /usr/local/ceph/etc:/etc/ceph \
    -v /usr/local/ceph/lib:/var/lib/ceph \
    -v /usr/local/ceph/logs:/var/log/ceph \
    -e MON_IP=192.168.174.130 \
    -e CEPH_PUBLIC_NETWORK=192.168.174.0/24 \
    ceph/daemon:latest mon
```

修改权限并运行mon服务

```
[root@damon-1 admin]# chmod 777 -R  /usr/local/ceph/admin/start_mon.sh    #修改权限
[root@damon-1 admin]#  /usr/local/ceph/admin/start_mon.sh
```

创建ceph配置文件

```
[root@damon-1 admin]# vi /usr/local/ceph/etc/ceph.conf
[global]
fsid = a80efb9b-00ca-447d-9307-1add78a63b53
mon initial members = localhost
mon host = 192.168.174.130
public network = 192.168.174.0/24
cluster network = 192.168.174.0/24
osd journal size = 100
osd pool default size = 2
mon clock drift allowed = 10
mon clock drift warn backoff = 30
mon_allow_pool_delete = true
[mgr]
mgr modules = dashboard
[client.rgw.CENTOS7-1]
rgw_frontends = "civetweb port=20003"
```

检查mon服务状态

```
#出现HEALTH_OK代表服务启动成功：
[root@damon-1 admin]#  docker exec -it mon ceph -s 
  cluster:
    id:     c29075e1-6a98-4ae1-b4c2-cfc3008a07d4
    health: HEALTH_OK
 
  services:
    mon: 1 daemons, quorum damon-1 (age 10s)
    mgr: no daemons active
    osd: 0 osds: 0 up, 0 in
```

将主节点配置复制到其他两个节点， 覆盖/usr/local/ceph/目录

```
[root@damon-1 admin]# scp -r /usr/local/ceph/ root@172.26.183.116:/usr/local/
[root@damon-1 admin]# scp -r /usr/local/ceph/ root@172.26.183.114:/usr/local/
```

damon-2，damon-3修改start_mon.sh和ceph.conf，ip地址改为自己本机并启动

```
[root@damon-2 local]# /usr/local/ceph/admin/start_mon.sh
[root@damon-3 ~]# /usr/local/ceph/admin/start_mon.sh
```

但由于本人电脑硬件，没办法启动集群，第三个出现

<img src="https://github.com/QSue/CC-BDA/raw/0b622276bc332fb15a1c1c1cd01a18f809fff9e3/CH5-3.png" alt="image-20211128222531168" style="zoom:200%;" />

