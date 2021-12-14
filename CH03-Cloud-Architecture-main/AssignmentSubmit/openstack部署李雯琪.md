# openstack部署



[TOC]

## 1. Openstack 简介

​	Openstack是一个云平台管理的项目，它不是一个软件。这个项目由几个主要的组件组合起来完成一些具体的工作。Openstack是一个旨在为公共及私有云的建设与管理提供软件的开源项目。它的社区拥有超过130家企业及1350位开发者，这些机构与个人将 Openstack作为基础设施即服务资源的通用前端。Openstack项目的首要任务是简化云的部署过程并为其带来良好的可扩展性。本文希望通过提供必要的指导信息，帮助大家利用 Openstack前端来设置及管理自己的公共云或私有云。



## 2. 核心组件

Openstack 包括 6 个核心组件（Nova、Neutron、Swift、Cinder、Keystone、Glance）和 14 个可选组件,每个组件包含若干个服务



| 分类     | 组件名称   | 功能                                                         |
| -------- | ---------- | ------------------------------------------------------------ |
| 核心组件 | Nova       | 管理虚拟机的整个生命周期:创建、运行、挂起、调度、关闭、销毁等。这是真正的执行部件。接受 DashBoard 发來的命令并完成具体的动作。但是 Nova 不是虛拟机软件，所以还需要虚拟机软件（如 KVM、Xen、Hyper-v 等）配合 |
|          | Neutron    | 管理网络资源，提供/一组应用编程接口(API)，用户可以调用它们来定义网络(如 VLAN )，并把定义好的网络附加给租户。Networking 是一个插件式结构，支持当前主流的网络设备和最新网铬技术 |
|          | Swift      | 是 [NoSQL](http://c.biancheng.net/nosql/) 数据库，类似 [HBase](http://c.biancheng.net/hbase/)，为虚拟机提供非结构化数据存储，它把相同的数据存储在多台计箅机上，以确保数据不会丢失。用户可通过 RESTful 和 HTTP 类型的 API 来和它通信。这是实际的存储项目，类似 Ceph，不过在 OpcnStack 具体实施时，人们更愿意采用 Ceph。 |
|          | Cinder     | 管理块设备，为虚拟机管理 SAN 设备源。但是它本身不是块设备源， 需要一个存储后端来提供实际的块设备源（如 iSCSI、FC等）。Cinder 相当于一个管家，当虚拟机需要块设备时，询问管家去哪里获取具体的块设备。它也是插件式的，安装在具体的 SAN 设备里。Cinder 支持的存储后端品牌参见 https://wiki.openstack.org/wiki/CinderSupportMatrix，驱动参见 https://github.com/openstack/cinder/tree/master/cinder/volume/drivers。 |
|          | Keystone   | 为其他服务提供身份验证、权限管理、令牌管理及服务名册管理。要使用云计算的所有用户事先需要在 Keystone 中建立账号和密码，并定义权限（注意:这里的“用户”不是指虚拟机里的系统账户，如 Windows 7 中的 Administrator )。另外，OpenStack 服务（如 Nova、Neutron、Swift、Cinder 等）也要在里面注册，并且登记具体的 API，Keystone 本身也要注册和登记 API |
|          | Glance     | 存取虚拟机磁盘镜像文件，Compute 服务在启动虚拟机时需要从这里获取镜像文件。这个组件不同于上面的 Swift 和 Cinder，这两者提供的 存储是在虚拟机里使用的 |
| 可选组件 | Horizon    | 提供了一个网页界面，用户登录后可以做这些操作：管理虚拟机、配置权限、分配 IP 地址、创建租户和用户等。本质上就是通过图形化的 操作界面控制其他服务（如 Compute、Networking 等)。当然，如果你熟悉命令，也可以直接采用命令来完成相应的任务 |
|          | Heat       | 如果要在成千上万个虚拟机里安装和配置同一个软件，该怎么办？采用 Orchestrates 是一个不错的主意，它向每个虚拟机里注人一个名叫 heat-cfntools 的客户端工具，然后就能同时操作很多虚拟机 |
|          | Sahana     | 使用户能够在 OpenStack 平台上（利用虚拟机）一键式创建和管理 Hadoop 集群，实现类似 AWS 的 EMR（Amazon Elastic MapReduce Service）功能。用户只需要提供简单的配置参数和模板，如版本信息（CDH 版本）、集群拓扑（几个 Slave、几个 Datanode）、节点配置信息（CPU、内存）等，Sahara 服务就能够在几分钟内根据提供的模板快速 部署 Hadoop、Spark 及 Storm 集群。Sahana 是一个[大数据](http://c.biancheng.net/big_data/)分析项目 |
|          | Ironic     | 把裸金属机器（与虚拟机相对）加人到资源池中                   |
|          | Zaqar      | Zaqar 为 Web 和移动开发者提供多租户云消息和通知服务，开发人员可以通过 REST API 在其云应用的不同组件中通过不同的通信模式（如 生产者/消费者或发布者/订阅者）来传递消息 |
|          | Ceilometer | 结合 Aodh、CloudKitty 两个组件，完成计费任务，如结算、消耗的 资源统计、性能监控等。OpenStack 之所以能管理公共云，一是因为 Ceilometer 的存在，二是因为引人了租户的概念 |
|          | Barbican   | 是 OpenStack 的密钥管理组件，其他组件可以调用 Barbican 对外暴露的 REST API 来存储和访问密钥 |
|          | Manila     | 为虚拟机提供文件共享服务，不过需要存储后端的配合             |
|          |            | 其他组件：Congress（策略服务）、Designate（DNS 服务）、Freezer（备份及还原服务）、Magnum（容器支持）、Mistral（工作流服务）、Monasca（监控服务）、Searchlight（索引和搜索）、Senlin（集群服务）、Solum（APP集成开发平台）、Tacker（网络功能 虚拟化）、Trove（数据库服务） |



## 3. 通过 DevStack部署openstack

### 3.1 创建stack用户

为了系统的安全，DevStack最好不要在root用户下直接运行，因此需要创建一个专门的用户stack，该用户需要有免密码sudo权限，配置如下:

```
sudo useradd -s /bin/bash -d /opt/stack -m stack
```

配置stack用户的免密sudo权限

```
echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
sudo -u stack -i
```

### 3.2  下载devstack安装包

```
git clone https://opendev.org/openstack/devstack
cd devstack
```

### 3.2 配置DevStack

在devstack 根目录下创建`local.conf`配置文件，包含admin密码、数据库密码、RabbitMQ密码以及Service密码：

```
[[local|localrc]]
ADMIN_PASSWORD=secret
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
```



### 3.3 安装DevStack

```
./stack.sh
```



