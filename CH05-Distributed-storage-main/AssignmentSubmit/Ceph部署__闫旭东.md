# Ceph部署



#### 1)部署环境

三台虚拟机（centos7)

镜像链接：http://mirror.hostlink.com.hk/centos/7.9.2009/isos/x86_64/

#### ![1640078834(1)](https://user-images.githubusercontent.com/90243359/146905540-85675178-44d1-477f-a9c0-876e4140e308.png)

修改3台主机的主机名（分别在三台主机执行）

```
hostnamectl  set-hostname ceph0
hostnamectl  set-hostname ceph1
hostnamectl  set-hostname ceph2
```

使用ifconfig命令查看主机ip（三台主机均查看）

```
ifconfig
```

###### <img src="https://user-images.githubusercontent.com/90243359/146906538-36c3f2dd-1cae-4bb0-92e3-6167263917f3.png" alt="1640079227(1)" style="zoom:50%;" />

配置域名解析（三台主机都执行）

```
echo "192.168.128.128  ceph0" >> /etc/hosts
echo "192.168.128.129  ceph1" >> /etc/hosts
echo "192.168.128.130  ceph2" >> /etc/hosts
```

生成密钥文件（ceph10主机执行）

```
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```

将生成密钥文件传送给其他节点（ceph10主机执行）

```
scp -r ~/.ssh ceph0:~/
scp -r ~/.ssh ceph1:~/
scp -r ~/.ssh ceph2:~/
```

###### <img src="https://user-images.githubusercontent.com/90243359/146907106-10932fb5-c24f-483c-9261-4a0a2fa0bcfe.png" alt="c2457379f80ef79ba0b6b529bf57487" style="zoom:67%;" />

使用命令测试ssh

```
ssh ceph1  #hostname
exit
```

###### <img src="https://user-images.githubusercontent.com/90243359/146908278-fc4cce1b-b44e-4e33-83a2-a38f04ec9a14.png" alt="1640079901" style="zoom: 67%;" />



#### 2)安装Ceph部署工具

ceph1执行

```
sudo yum install -y yum-utils && sudo yum-config-manager --add-repo https://dl.fedoraproject.org/pub/epel/7/x86_64/ && sudo yum install --nogpgcheck -y epel-release && sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7 && sudo rm /etc/yum.repos.d/dl.fedoraproject.org*

```

更新软件库并安装 ceph-deploy ：

```
sudo yum update && sudo yum install ceph-deploy
```

安装NTP服务

###### 建议在所有 Ceph 节点上安装 NTP 服务（特别是 Ceph Monitor 节点），以免因时钟漂移导致故障。

```
sudo yum install ntp ntpdate ntp-doc
```

SELIINUX

###### 在 CentOS 和 RHEL 上， SELinux 默认为 Enforcing 开启状态。为简化安装，我们建议把 SELinux 设置为 Permissive 或者完全禁用，也就是在加固系统配置前先确保集群的安装、配置没问题。用下列命令把 SELinux 设置为 Permissive ：

```
sudo setenforce 0
```

关闭防火墙

```
systemctl  stop firewalld
systemctl  disable firewalld
```



### 3）部署mon

创建集群（ceph1执行）

```
ceph-deploy new ceph0 ceph1 ceph2
```

安装ceph（ceph1执行）

```
ceph-deploy install ceph0 ceph1 ceph2
```

###### <img src="https://user-images.githubusercontent.com/90243359/146910517-a64b6ac6-89ef-4722-9ad8-c24eb024d373.png" alt="95d5ca61a72305cf7225bec1e6b0e0e" style="zoom:80%;" />

###### <img src="https://user-images.githubusercontent.com/90243359/146910534-8487e130-6792-4ad6-83b0-4824c60d3b34.png" alt="059fb03fad44754f9243bdcf3eb45fd" style="zoom:80%;" />

###### <img src="https://user-images.githubusercontent.com/90243359/146910551-ec1cdaa1-6913-4b64-9bf8-4810629232a9.png" alt="d0faf5da6942d10bcfe4b6e38928e2b" style="zoom:80%;" />

配置初始 monitor(s)、并收集所有密钥

```
ceph-deploy mon create-initial
```

用 ceph-deploy 把配置文件和 admin 密钥拷贝到管理节点和 Ceph 节点，这样你每次执行 Ceph 命令行时就无需指定 monitor 地址和 ceph.client.admin.keyring 了。

```
ceph-deploy admin  ceph001 ceph002 ceph003
```



### 4）部署osd

增加挂载磁盘虚拟机ceph1、ceph2操作

###### <img src="https://user-images.githubusercontent.com/90243359/146910901-e7427253-2ef4-4f7a-9ced-1a3621e385fb.png" alt="1640080931(1)" style="zoom: 50%;" />

添加osd(sdb分别为ceph1、ceph2挂载的磁盘)

```
ceph-deploy osd create --data  /dev/sdb ceph1
ceph-deploy osd create --data  /dev/sdb ceph1
```

###### <img src="https://user-images.githubusercontent.com/90243359/146911988-e74505b3-be11-49ad-a7c2-d49cbb776a4d.png" alt="b8cf2f7a1d3a7f148b5365ecb7f3d43" style="zoom:80%;" />

###### <img src="https://user-images.githubusercontent.com/90243359/146912013-dfefa6c4-b245-4580-b6aa-e25c682f10c4.png" alt="7450f9bdfa785e68c86c2d09cc00fc7" style="zoom:80%;" />

查看osd

```
ceph osd status
```

###### <img src="https://user-images.githubusercontent.com/90243359/146912222-ee31f958-a5ed-4550-ba79-326efc1a1ad5.png" alt="1640081484(1)" style="zoom:80%;" />



### 5)部署mgr

添加mgr

```
ceph-deploy mgr create ceph0 ceph1 ceph2
```

设置manager组件 web UI访问配置

```
ceph config set mgr mgr/dashboard/ssl false
ceph config set mgr mgr/dashboard/server_addr 0.0.0.0
```

启动dashboard模块

```
ceph mgr module enable dashboard
```

设置账户密码

```
ceph dashboard set-login-credentials admin admin
```

查看mgr组件

```
ceph mgr services
```

###### <img src="https://user-images.githubusercontent.com/90243359/146912932-33edd628-2af5-4ea1-9d54-6c795670edd8.png" alt="1640081773(1)" style="zoom:80%;" />

浏览器输入上述链接查看

###### <img src="https://user-images.githubusercontent.com/90243359/146913047-430c1ea0-569c-4719-b428-156cea0fdefd.png" alt="712672ac25772e3dc76fdf033493bde" style="zoom:50%;" />

###### <img src="https://user-images.githubusercontent.com/90243359/146913168-ab2ed613-161e-4858-9193-97472707054f.png" alt="62da322733f97a47db6d75629e4ea6d" style="zoom:50%;" />

###### <img src="https://user-images.githubusercontent.com/90243359/146913264-3e202706-8d22-4a8d-acf7-1a28891e540c.png" alt="6b4fa2e61129cdf5d156e7d817d1d8c" style="zoom: 80%;" />
