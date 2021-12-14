# Ceph学习心得

## 什么是Ceph

### Ceph是一种为优秀的性能、可靠性和可扩展性而设计的统一的、分布式的存储系统。Ceph 独一无二地用统一的系统提供了**对象、块、和文件存储**功能，它可靠性高、管理简便、并且是开源软件。 Ceph 的强大足以改变贵公司的 IT 基础架构、和管理海量数据的能力。Ceph 可提供极大的伸缩性——供成千用户访问 PB 乃至 EB 级的数据。 [Ceph 节点](http://docs.ceph.org.cn/glossary/#term-13)以普通硬件和智能守护进程作为支撑点， [Ceph 存储集群](http://docs.ceph.org.cn/glossary/#term-21)组织起了大量节点，它们之间靠相互通讯来复制数据、并动态地重分布数据。

##  Ceph的核心组件

### Ceph的核心组件包括Ceph OSD、Ceph Monitor和Ceph MDS三大组件。

## Ceph系统逻辑层次结构

### 基础存储系统RADOS（Reliable, Autonomic, Distributed Object Store，即可靠的、自动化的、分布式的对象存储)

### 基础库librados,这一层的功能是对RADOS进行抽象和封装，并向上层提供API，以便直接基于RADOS（而不是整个Ceph）进行应用开发。特别要注意的是，RADOS是一个对象存储系统，因此，librados实现的API也只是针对对象存储功能的。

### 高层应用接口,这一层包括了三个部分：**RADOS GW（RADOS Gateway）、 RBD（Reliable Block Device）和Ceph FS（Ceph File System）**，其作用是在librados库的基础上提供抽象层次更高、更便于应用或客户端使用的上层接口。

## 安装

### 安装ceph-deploy

```text
wget -q -O- 'https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc' | sudo apt-key add -
```

### 将源信息加入repo，更新软件源，并按照ceph-deploy

```text
echo deb http://ceph.com/debian-hammer/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list
apt-get update
apt-get install ceph-deploy
```

### **安装NTP** 安装NTP，用于集群节点的时间同步

```text
apt-get install ntp
```

### **创建ceph部署账户** 在每个存储节点上创建ceph账户及ssh访问 每个节点上创建ceph用户

```text
sudo useradd -d /home/cephd -m cephd
sudo passwd cephd
```

### 确保具有sudo权限

```text
echo "cephd ALL = (root) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/cephd
sudo chmod 0440 /etc/sudoers.d/cephd
```

### 创建ceph集群

```text
mkdir my-cluster
cd my-cluster
```

### 修改ceph.conf配置文件

```text
[global]
fsid = c70a17e3-f677-46cb-8744-b628592d69d6
mon_initial_members = ceph-u0-l0
mon_host = 192.168.32.2
auth_cluster_required = cephx
auth_service_required = cephx
auth_client_required = cephx
filestore_xattr_use_omap = true
osd pool default size = 3
public network = 192.168.1.0/24
[osd]
osd journal size = 20000
```

### 安装软件包

#### admin节点向各节点安装ceph

```text
ceph-deploy install ceph-u0-l0 ceph-u0-m0 ceph-u0-r0 --repo-url=http://eu.Ceph.com/debian-hammer/ --gpg-url=http://eu.ceph.com/keys/release.asc
```

#### 添加osd节点

### OSD是存储数据的单元，新建完集群后需要添加OSD节点

ceph-deploy osd prepare ceph-u0-l0:sdb ceph-u0-m0:sdb ceph-u0-r0:sdb

ceph-deploy osd activate ceph-u0-l0:sdb1 ceph-u0-m0:sdb1 ceph-u0-r0:sdb1

#### 查看集群运行状态

```text
ceph -s
```

