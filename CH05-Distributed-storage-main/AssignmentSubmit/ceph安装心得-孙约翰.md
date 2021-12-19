### ceph安装心得

#### ceph组件：

OSD:

- OSD 守护进程,至少两个
- 用于存储数据、处理数据拷贝、恢复、回滚、均衡
- 通过心跳程序向monitor提供部分监控信息
- 一个ceph集群中至少需要两个OSD守护进程

MON：

- 维护集群的状态映射信息
- 包括monitor、OSD、placement Group(PG)
- 还维护了monitor、OSD和PG的状态改变历史信息

#### 安装环境：

ceph-0：eth0:192.168.0.150  

ceph-1:   eth0:192.168.0.151

ceph-2:   eth0:192.168.0.152

#### 关闭防火墙：

#systemctl stop firewalld

#### 安装epel源与ceph-deploy

#wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

#rpm -ivh https://mirrors.tuna.tsinghua.edu.cn/ceph/rpm-luminous/el7/noarch/ceph-release-1-1.el7.noarch.rpm

#### 安装ceph

##### 安装ceph-deploy

#yum install -y ceph-deploy

##### 创建 ceph-install 目录并进入，安装时产生的文件都将在这个目录

#mkdir ceph-install && cd ceph-install

##### 使用ceph-deploy安装ceph,以下步骤只在ceph-depoly管理节点执行

- ###### 创建一个ceph集群，也就是Mon,三台都充当mon

#ceph-deploy new ceph-node0 ceph-node1 ceph-node2

- ###### 在全部节点上安装ceph

#ceph-deploy install ceph-node0 ceph-node1 ceph-node2

- ###### 创建和初始化监控节点并收集所有的秘钥

#ceph-deploy mon create-initial

![image-20211217204418592](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211217204418592.png)

- ###### 创建OSD存储节点

#ceph-deploy osd create ceph-node0 --data /dev/sdc --journal /dev/sdb1

#ceph-deploy osd create ceph-node0 --data /dev/sdc --journal /dev/sdb2

#ceph-deploy osd create ceph-node1 --data /dev/sdc --journal /dev/sdb1

#ceph-deploy osd create ceph-node1 --data /dev/sdc --journal /dev/sdb2

#ceph-deploy osd create ceph-node2 --data /dev/sdc --journal /dev/sdb1

#ceph-deploy osd create ceph-node2 --data /dev/sdc --journal /dev/sdb2

- ###### 把配置文件和admin 秘钥到管理节点和ceph节点

#ceph-deploy --overwrite-conf admin ceph-node0 ceph-node1 ceph-node2

###### 输入ceph -s查看集群状态

![image-20211217204710713](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211217204710713.png)