# OpenStack心得：

### 一.简介：

### OpenStack是一个由NASA（美国国家航空航天局）和Rackspace合作研发并发起的，以Apache许可证授权的自由软件和开放源代码项目。OpenStack是一个开源的云计算管理平台项目，由几个主要的组件组合起来完成具体工作。OpenStack支持几乎所有类型的云环境，项目目标是提供实施简单、可大规模扩展、丰富、标准统一的云计算管理平台。OpenStack通过各种互补的服务提供了基础设施即服务（IaaS）的解决方案，每个服务提供API以进行集成。OpenStack是一个旨在为公共及私有云的建设与管理提供软件的开源项目。它的社区拥有超过130家企业及1350位开发者，这些机构与个人都将OpenStack作为基础设施即服务（IaaS）资源的通用前端。

### 二.安装OpenStack：

### 0. 环境准备：

### 8G内存，60G硬盘。Ubuntu 16.04系统，server image即可

###  1. 配置Ubuntu apt source源

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo vim /etc/apt/sources.list
```

###  2. 配置pip源

```
mkdir ~/.pip
vim ~/.pip/pip.conf
sudo mkdir /root/.pip
sudo vim /root/.pip/pip.conf
```

###  3. 下载devstack

```
git clone https://git.openstack.org/openstack-dev/devstack
cd devstack
git checkout remotes/origin/stable/queens
git checkout -b queens
```

###  4. 配置devstack local.conf

```
cp samples/local.conf ./
vim local.conf
cp ~/devstack/samples/local.sh ~/devstack/
```

### 5.安装

在devstack目录，执行：

```
./stack.sh
```

### 6.验证

```
source ~/devstack/openrc admin admin
nova boot --image cirros-0.3.5-x86_64-disk --flavor 1 --nic net-name=private vm1
```











