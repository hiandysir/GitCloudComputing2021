# Ceph安装心得

### Ceph简介：

### Ceph是一个统一的分布式存储系统，设计初衷是提供较好的性能、可靠性和可扩展性。Ceph项目最早起源于Sage就读博士期间的工作（最早的成果于2004年发表），并随后贡献给开源社区。在经过了数年的发展之后，目前已得到众多云计算厂商的支持并被广泛应用。RedHat及OpenStack都可与Ceph整合以支持虚拟机镜像的后端存储。

### **Ceph特点**：

### 1.高性能

### 2.高可用性

### 3.高可扩展性

### 4.特性丰富

## 一.环境准备：

### 1.三台虚拟机

- node1 192.168.68.127
- node2 192.168.68.128
- node3 192.168.68.129

### 2.关闭防火墙 firewalld和selinux

```
systemctl stop firewalld
systemctl disable firewalld
sed -i '/SELINUX/s/enforcing/disabled/g'  /etc/sysconfig/selinux
setenforce 0
```

### 3.修改节点主机名

``` 
hostnamectl set-hostname node1
hostnamectl set-hostname node2
hostnamectl set-hostname node3
```

### 4.添加hosts解析

```
cat >/etc/hosts<<EOF
127.0.0.1 localhost localhost.localdomain
192.168.68.127 node1
192.168.68.128 node2
192.168.68.129 node3
EOF
```

### 5.同步节点时间

```
yum install ntpdate -y
ntpdate  pool.ntp.org
```

### 6.配置免秘钥

```
ssh-keygen
ssh-copy-id node1
ssh-copy-id node2
ssh-copy-id node3
```

### 7.配置Ceph EPEL 国内yum源

```
cat <<EOM > /etc/yum.repos.d/ceph.repo
[noarch]
name=ceph noarch
baseurl=https://mirrors.aliyun.com/ceph/rpm-nautilus/el7/noarch
enabled=1
gpgcheck=0
[x86_64]
name=ceph x86_64
baseurl=https://mirrors.aliyun.com/ceph/rpm-nautilus/el7/x86_64
enabled=1
gpgcheck=0
EOM

yum install epel-release
yum makecache
```

### 8.在部署机上安装ceph-deploy

### 9.更新node2,node3节点的yum源

```
for host in node{2..3};do scp -r /etc/yum.repos.d/*  $host:/etc/yum.repos.d/ ;done
```

### 10.在node1上安装相关包

```
for host in node{1..3};do ssh -l root $host yum install ceph ceph-radosgw -y ;done
ceph -v
rpm -qa |grep ceph
```

## 二.部署ceph集群

### 部署机node1上
### 1.部署ceph集群主要是通过ceph-deploy来进行。

```
mkdir -p cluster
cd cluster/
ceph-deploy new node1 node2 node3
ceph-deploy mon create-initial
ceph-deploy admin node1 node2 node3
ceph -s
```

### 2.创建mgr

### 3. 创建osd



