#### Ceph分布式存储架构搭建心得

##### 前期环境准备

\1.环境准备

分别将Monitor节点定义为node1，两台OSD节点定义为node2、node3，RGW节点定义为node4.

![image-20211216160720860](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211216160720860.png)

\2.在所有节点上安装SSH server服务.运行#apt-get install openssh-server

因为我们搭建的Ceph直接使用root用户，所以需要修改SSH配置文件/etc/ssh/sshd_config,将PermitRootLogin的参数改为yes.之后执行命令#service ssh restart.

\3.使用SSH免密登录

生成SSH keys，拷贝这个key到所有节点。

#ssh-keygen

#ssh-copy-id node1



##### 部署Ceph存储

在monitor节点node1上安装ceph-deploy,通过ceph-deploy在node1上部署monitor，在node2和node3节点上部署OSD,最后，在node4上部署Ceph网关rgw.

在node1上创建一个目录，用来维护ceph-deploy生成的配置信息。ceph-deploy命令会在当前目录生成输出文件，确保执行命令#mkdir my-cluster, #cd my-cluster,时位于对应的目录。

##### 安装ceph-deploy

更新镜像仓库，并安装ceph-deploy。

#apt-get update && sudo apt-get install ceph-deploy

##### 重新开始部署Ceph

在安装过程中如果遇到了问题，像重新开始安装，执行以下命令来清空配置。

#ceph-deploy purgedate {ceph-node} [{ceph-node}]

#ceph-deploy forgetkeys

如果执行第一个命令提示Ceph还安装在该节点上，拒绝执行清空操作。可以先执行ceph-deploy purge，在执行以上两条命令。

##### 部署Ceph

######  创建集群

#ceph-deploy new node1

在当前目录下使用ls和cat命令检查ceph-deploy输出结果，可以看到一个ceph配置文件，一个密钥环以及为新集群创建的日志文件。

###### 修改OSD参数

因为环境中有两个OSD,而Ceph模式的副本个数为3，因此需要修改配置文件Ceph.conf，在[global]部分增加一下配置：

osd pool default size=2

###### 配置Ceph网络参数

如果环境中有多种网络，需要在ceph.conf的[global]部分增加一下配置。如果只有一种网络，就不需要此操作。

public network={ip-address}/{netmask}

###### 安装Ceph

#ceph-deploy install node1 node2 node3 node4

这个命令是在每个节点上都安装Ceph。

###### 安装monitor

安装并初始化Monitor，收集Keys：

#ceph-deploy mon create-initial

执行完命令后，当前目录会生成如下keyring：

![image-20211216165433721](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211216165433721.png)

###### 创建OSD数据目录

OSD的数据目录可以使用单独的分区，也可以只使用已有分区的目录。此实验直接使用目录的方式。添加两个OSD：

#ssh node2

#sudo mkdir /var/local/osd0

#chown ceph:ceph /var/local/osd0

#exit



#ssh node2

#sudo mkdir /var/local/osd0

#chown ceph:ceph /var/local/osd0

#exit



###### 准备OSD

#ceph-deploy osd prepare node2:/var/local/osd0 node3:/var/local/osd1

###### 激活OSD

#ceph-deploy osd prepare node2:/var/local/osd0 node3:/var/local/osd1

###### 拷贝配置文件和管理key

ceph-deploy admin node1 node2 node3

###### 检查集群状态

#ceph -s

集群应该返回health HEALTH_OK,并且所有package都是active+clean的状态。

###### 部署rgw网关

#ceph-deploy rgw create node4

###### 验证Ceph

创建一个普通文本文件testfile.txt,并向其写入数据。

创建一个pool。格式为：

#rados mkpool data

将文件写入pool：

#rados put test-object-1 testfile.txt --pool=data

查看文件是否存在于pool中：

#rados -p data ls

确定文件的位置：

#ceph osd map data test-object-1

从pool中读取文件：

#rados get test-object-1 --pool=data myfile