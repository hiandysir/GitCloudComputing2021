---
CloudComputing2021
ch3 study notes
Author：zoxiii
---

[TOC]

> 以前有安装过CloudStack，它们的功能类似

# 1、部署一个单节点的OpenStack

[OpenStack官网](https://docs.openstack.org/install-guide/)

## 前置环境配置
### 0、网络可以ping通

### 1、修改主机名
```c
# hostnamectl set-hostname zoxiii
```

### 2、配置IP和主机名映射文件

```c
# vi /etc/hosts
```
添加如下内容：

```c
192.168.184.5 zoxiii
```
### 3、关闭防火墙

```c
# firewall-cmd --state        ## 查看防火墙的状态
# systemctl stop firewalld    ## 禁用防火墙
```
### 4、关闭NetworkManager服务

```c
# systemctl stop NetworkManager
# systemctl disable NetworkManager
```
### 5、安装RDO源

```c
# yum install -y https://rdoproject.org/repos/rdo-release.rpm
```
### 6、禁用SELINUX
```c
# setenforce 0
# sed -i ‘s/SELINUX=enforcing/SELINUX=disabled/g’ /etc/selinux/config
```
## 开始安装
### 1、安装Packstack Installer

```c
# yum install -y openstack-packstack
```
### 2、生成OpenStack应答文件

```c
# packstack --gen-answer-file=/root/answer.txt
```
### 3、编辑应答文件，我们选择安装OpenStack时不安装Demo project

```c
# vi answer.txt
CONFIG_PROVISION_DEMO=n # 不安装DEMO
CONFIG_KEYSTONE_ADMIN_PW=xxx # 设置管理员密码
CONFIG_HORIZON_SSL=y # 启用SSL访问（根据需求是否更改）
CONFIG_NEUTRON_OVS_BRIDGE_MAPPINGS=extnet:br-ex # OVS Bridge名称
CONFIG_NEUTRON_OVS_BRIDGE_IFACES=br-ex:ens160 # 接口名称

```
### 4、通过应答文件运行PackStack安装程序

```c
# packstack --answer-file=/root/answer.txt
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/4188aa1a56444d738523039512e4c91d.png)
### 5、安装完成
Dashboard访问地址：这里为`https://192.168.184.5/dashboard`
### 6、修改网络接口配置文件
- 在OpenStack中设置OVS Bridge接口
- 为OpenStack配置网桥接口
- 为OpenStack配置物理网络接口

```c
# cd /etc/sysconfig/network-scripts/
# ls  
# cp ifcfg-ens33 ifcfg-br-ex
# vi ifcfg-br-ex
TYPE="Ethernet"
PROXY_METHOD="none"
BROWSER_ONLY="no"
BOOTPROTO="none"
DEFROUTE="yes"
IPV4_FAILURE_FATAL="no"
IPV6INIT="no"
IPV6_AUTOCONF="no"
IPV6_DEFROUTE="no"
IPV6_FAILURE_FATAL="no"
IPV6_ADDR_GEN_MODE="no"
NAME="br-ex"
UUID="0da02f41-89f9-43fd-b2a9-7de6d89165e3"
DEVICE="br-ex"
ONBOOT="yes"
IPADDR="192.168.2.188"
PREFIX="24"
GATEWAY="192.168.2.1"
DNS1="8.8.8.8"
IPV6_PRIVACY="no"

```

```c
# vi ifcfg-ens33
TPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no
IPV6_AUTOCONF=no
IPV6_DEFROUTE=no
IPV6_FAILURE_FATAL=no
NAME=ens33
DEVICE=ens33
ONBOOT=yes
TYPE=OVSPort
DEVICETYPE=ovs
OVS_BRIDGE=br-ex
IPV6_PRIVACY=no

```
### 7、重启网络

```c
# systemctl restart network.service
# ifconfig

```
### 8、访问网页
![在这里插入图片描述](https://img-blog.csdnimg.cn/b469a50084dd4876a91fb0ff101c0c7f.png)






# 2、通过DevStack工具来安装OpenStack

[官方文档](https://kairen.gitbooks.io/openstack-centos/content/deployments/index.html )

## 安装DevStack
### 1、下载 DevStack 
```c
# git clone https://opendev.org/openstack/devstack
# cd devstack
```
### 2、创建一个 local.conf

```c
# touch local.conf
# vi local.conf
[[local|localrc]]
ADMIN_PASSWORD=secret
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
```
### 3、开始安装

```c
# ./stack.sh
```

