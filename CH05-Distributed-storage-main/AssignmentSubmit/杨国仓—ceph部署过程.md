

# ceph部署心得





[TOC]

## 1. Ceph简介

Ceph是一个统一的分布式存储系统，设计初衷是提供较好的性能、可靠性和可扩展性。

Ceph项目最早起源于Sage就读博士期间的工作（最早的成果于2004年发表），并随后贡献给开源社区。在经过了数年的发展之后，目前已得到众多云计算厂商的支持并被广泛应用。RedHat及OpenStack都可与Ceph整合以支持虚拟机镜像的后端存储。



## 2. 部署环境

### 2.1  服务器信息

- 硬件环境：vmvare虚拟机,  系统:centos7.9    CPU:4C MEM:4G  系统盘:100G  数据盘:100G
- 操作系统：CentOS Linux release 7.9.2009  (Core)
- 软件版本：Mimic 13.2.8
- 部署版本：ceph-deploy  2.0.1



### 2.2 服务器配置

- 设置/etc/hosts文件


```
echo "192.168.100.101  ceph001" >>  /etc/hosts
```

- 关闭Selinux


```
setenforce 0
```

- 关闭防火墙


```
systemctl stop iptables
systemctl stop firewalld
systemctl disable iptables
systemctl disable firewalld
```

- 设置Ceph安装yum源


```
cat << EOM > /etc/yum.repos.d/ceph.repo
> [ceph-noarch]
> name=Ceph noarch packages
> baseurl=https://download.ceph.com/rpm-mimic/el7/noarch
> enabled=1
> gpgcheck=1
> type=rpm-md
> gpgkey=https://download.ceph.com/keys/release.asc
> EOM


安装EPEL 仓库
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
```



## 3. 部署ceph服务

### 3.1 安装ceph软件

- 安装 ceph, ceph-deploy软件包

```
yum install -y ceph ceph-deploy
```

- 查看ceph，ceph-deploy版本

```
ceph -v
ceph-deploy --version
```

- 安装ceph组件  

```
ceph-deploy install ceph001 
```

- 创建ceph集群

```
ceph-deploy new ceph001
```



### 3.2 安装monitor组件

- 初始化 monitor

```
ceph-deploy --overwrite-conf mon create
```

- 收集集群密钥

```
ceph-deploy gatherkeys ceph001
```

- 将配置文件分发到对应的节点

```
ceph-deploy admin  ceph001
```



### 3.3 安装manager组件

- 创建mgr

```
ceph-deploy mgr create ceph001
```

- 设置manager组件 web UI访问配置

```
ceph config set mgr mgr/dashboard/ssl false
ceph config set mgr mgr/dashboard/server_addr 0.0.0.0
```

- 启动dashboard模块

```
ceph mgr module enable dashboard
```

- 设置登录账号密码

```
ceph dashboard set-login-credentials admin admin
```

- 查看manager组件状态

```
ceph mgr services

{
    "dashboard": "http://0.0.0.0:8080/"
}

```



### 3.4 安装osd组件

- 安装osd组件

```
ceph-deploy osd create ceph001 --data /dev/sdb
```

- 创建ceph存储池

```
ceph osd pool create ceph 64 64
```

- 设置存储池副本为1

```
ceph osd pool set ceph size 1
```

- 查看存储池

```
ceph osd pool ls
```



## 4. 测试验证

### 4.1 文件上传测试

- 上传文件到存储池

```
rados -p ceph put ceph.conf /etc/ceph/ceph.conf
```

- 列出存储池中的对象

```
rados -p ceph ls
```

- 从存储池中下载对象

```
rados -p ceph get test /tmp/ceph.conf

cat /tmp/ceph.conf
```

- 删除存储池的对象

```
rados -p ceph rm test
```

