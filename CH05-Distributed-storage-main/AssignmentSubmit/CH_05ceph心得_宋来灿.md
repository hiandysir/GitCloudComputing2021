# Ceph安装心得



## 1. Ceph 简介

​	Ceph是一种为优秀的性能、可靠性和可扩展性而设计的统一的、分布式文件系统。ceph 的统一体现在可以提供文件系统、块存储和对象存储，分布式体现在可以动态扩展。在国内一些公司的云环境中，通常会采用 ceph 作为openstack 的唯一后端存储来提高数据转发效率。

#### 	核心组件

- **Ceph OSDs**: [*Ceph OSD 守护进程*](http://docs.ceph.org.cn/glossary/#term-56)（ Ceph OSD ）的功能是存储数据，处理数据的复制、恢复、回填、再均衡，并通过检查其他OSD 守护进程的心跳来向 Ceph Monitors 提供一些监控信息。当 Ceph 存储集群设定为有2个副本时，至少需要2个 OSD 守护进程，集群才能达到 `active+clean` 状态（ Ceph 默认有3个副本，但你可以调整副本数）。
- **Monitors**: [*Ceph Monitor*](http://docs.ceph.org.cn/glossary/#term-ceph-monitor)维护着展示集群状态的各种图表，包括监视器图、 OSD 图、归置组（ PG ）图、和 CRUSH 图。 Ceph 保存着发生在Monitors 、 OSD 和 PG上的每一次状态变更的历史信息（称为 epoch ）。
- **MDSs**: [*Ceph 元数据服务器*](http://docs.ceph.org.cn/glossary/#term-63)（ MDS ）为 [*Ceph 文件系统*](http://docs.ceph.org.cn/glossary/#term-45)存储元数据（也就是说，Ceph 块设备和 Ceph 对象存储不使用MDS ）。元数据服务器使得 POSIX 文件系统的用户们，可以在不对 Ceph 存储集群造成负担的前提下，执行诸如 `ls`、`find` 等基本命令。



## 2. Ceph特点

**高性能：**

1. 摒弃了传统的集中式存储元数据寻址的方案，采用[CRUSH算法](https://www.zhihu.com/search?q=CRUSH算法&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A161358190})，数据分布均衡，并行度高。
2. 考虑了容灾域的隔离，能够实现各类负载的副本放置规则，例如跨机房、机架、感知等。
3. 能够支持上千个存储节点的规模，支持TB到PB级的数据。

**高可用性：**

1. 副本数可以灵活控制。（就是说让副本保存份数可以多份，在正常的生产环境是保存3副本）
2. 支持故障域分隔，数据强一致性。
3. 多种故障场景自动进行修复自愈。
4. 没有单点故障，自动管理。（假如说我这个文件设置的是3副本，如果后端服务器坏掉，副本数不够3，它会自动补充至3副本）

**高可扩展性：**

1. 去中心化。
2. 扩展灵活。
3. 随着节点增加而线性增长。



## 3. Ceph应用场景

Ceph可以提供对象存储、块设备存储和文件系统服务，其对象存储可以对接网盘（owncloud）应用业务等；其块设备存储可以对接（IaaS），当前主流的IaaS运平台软件，如：OpenStack、CloudStack、Zstack、Eucalyptus等以及kvm等。

- **对象存储（RADOSGW）**：提供RESTful接口，也提供多种编程语言绑定。兼容S3（是AWS里的对象存储）、Swift（是openstack里的对象存储）；
- **块存储（RDB）**：由RBD提供，可以直接作为磁盘挂载，内置了容灾机制；
- **文件系统（CephFS）**：提供POSIX兼容的网络文件系统CephFS，专注于高性能、大容量存储；



## 4. Ceph 部署

### 4.1 部署环境

| IP地址          | 主机名称 | 组件                |
| --------------- | -------- | ------------------- |
| 192.168.100.101 | ceph001  | mon osd  admin-node |
| 192.168.100.102 | ceph002  | mon osd             |
| 192.168.100.103 | ceph003  | mon osd             |



### 4.2 前期准备



- #### 修改主机名


```
hostnamectl  set-hostname ceph001     # 在 ceph001 节点执行
hostnamectl  set-hostname ceph002     # 在 ceph002 节点执行
hostnamectl  set-hostname ceph003     # 在 ceph003 节点执行
```

- #### 配置本地域名解析 (每个节点都要执行)

```
echo "192.168.100.101  ceph001" >> /etc/hosts
echo "192.168.100.102  ceph002" >> /etc/hosts
echo "192.168.100.103  ceph003" >> /etc/hosts
```

- #### 生成密钥文件  （在 ceph001 节点 root用户下执行）

```
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```

- #### 将生成的密钥传输到其他节点 （在 ceph001 节点 root用户下执行）

```
scp -r ~/.ssh ceph001:~/
scp -r ~/.ssh ceph002:~/
scp -r ~/.ssh ceph003:~/
```

- #### SSH连接测试

```
ssh ceph001 'echo $HOSTNAME'
ssh ceph002 'echo $HOSTNAME'
ssh ceph003 'echo $HOSTNAME'
```



### 4.3 安装ceph发行包    （在 ceph001 节点 root用户下执行）

```
rpm -ivh https://download.ceph.com/rpm-mimic/el7/noarch/ceph-release-1-1.el7.noarch.rpm
```

- #### 安装 ceph, ceph-deploy

```
yum install -y ceph ceph-deploy
```

- #### 查看ceph，ceph-deploy版本

```
ceph -v
ceph-deploy --version
```

- #### 为其他节点安装ceph组件   （在 ceph001 节点 root用户下执行）

```
ceph-deploy install ceph001 ceph002 ceph003
```

### 4.4 创建ceph集群

```
ceph-deploy new ceph001 ceph002 ceph003
```



### 4.5 部署monitor组件

- #### 初始化 monitor

```
ceph-deploy --overwrite-conf mon create ceph001 ceph002 ceph003
```

- #### 收集集群密钥

```
ceph-deploy gatherkeys ceph001 ceph002 ceph003
```

- #### 将配置文件分发到对应的节点

```
ceph-deploy admin  ceph001 ceph002 ceph003
```



### 4.6 部署manager组件

- #### 部署manager组件

```
ceph-deploy mgr create ceph001 ceph002 ceph003
```

- #### 设置manager组件 web UI访问配置

```
ceph config set mgr mgr/dashboard/ssl false
ceph config set mgr mgr/dashboard/server_addr 0.0.0.0
```

- #### 启动dashboard模块

```
ceph mgr module enable dashboard
```

- #### 设置登录账号密码

```
ceph dashboard set-login-credentials admin admin
```

- #### 查看manager组件状态

```
ceph mgr services

{
    "dashboard": "http://0.0.0.0:8080/"
}

```

- #### 验证manager组件是否访问成功

```
http://192.168.100.101:8080/
```

![](http://m.qpic.cn/psc?/V51qUp8Q4ORGKu4Dhczg2JyCx12uI1yY/45NBuzDIW489QBoVep5mcWWrf0IwxS.UDeVhwPYtFdFNmkjajVmbqm0NXfAyIKFzhHr5yedd3gzOZVNAbQWKn9c4I39eOr3830dyVxt6j7g!/b&bo=0ASSAgAAAAADF3Y!&rf=viewer_4)



![](http://m.qpic.cn/psc?/V51qUp8Q4ORGKu4Dhczg2JyCx12uI1yY/45NBuzDIW489QBoVep5mceraofX1d7kOHjFOdL3mF1Kk5dDMoyNaasWmxE3T1NtPoVDHYqcZiKyVFxf4Ur.FFcLVWjfoJPCY875oQrTHLaQ!/b&bo=hgbqAwAAAAADF1s!&rf=viewer_4)



### 4.7 部署osd组件

- #### 部署osd

```
ceph-deploy osd create ceph001 --data /dev/sdb
ceph-deploy osd create ceph002 --data /dev/sdb
ceph-deploy osd create ceph003 --data /dev/sdb
```

- #### 查看 osd组件状态

```
ceph osd status

+----+---------+-------+-------+--------+---------+--------+---------+-----------+
| id |   host  |  used | avail | wr ops | wr data | rd ops | rd data |   state   |
+----+---------+-------+-------+--------+---------+--------+---------+-----------+
| 0  | ceph001 | 1025M | 1022G |    0   |     0   |    0   |     0   | exists,up |
| 1  | ceph002 | 1025M | 1022G |    0   |     0   |    0   |     0   | exists,up |
| 2  | ceph003 | 1025M | 1022G |    0   |     0   |    0   |     0   | exists,up |
+----+---------+-------+-------+--------+---------+--------+---------+-----------+
```

- #### 创建ceph存储池

```
ceph osd pool create ceph 64 64
```

- #### 查看存储池

```
ceph osd pool ls
```

- #### 上传文件到存储池

```
rados -p ceph put test /etc/ceph/ceph.conf
```

- #### 列出存储池中的对象

```
rados -p ceph ls
```

- #### 从存储池中下载对象

```
rados -p ceph get test /tmp/ceph.conf
```

- #### 删除存储池的对象

```
rados -p ceph rm test
```









