# docker安装心得：

### 一.什么是Docker

### Docker 将应用程序与该程序的依赖，打包在一个文件里面，该文件包括了所有打包的应用程序的所有依赖，像数据库等；直接运行该文件，就可以让程序跑起来，从而不用再去考虑环境问题。



### 二.安装Docker

#### 1.下载关于Docker的依赖环境

yum -y install yum-utils device-mapper-persistent-data lvm2

#### 2.设置下载Docker的镜像源

yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

#### 3.安装Docker

yum makecache fast
yum -y install docker-ce

#### 4.启动Docker服务

systemctl start docker

#### 5.设置开机自动启动

systemctl enable docker

#### 6.测试

docker run hello-world

