# 第三章 OpenStack 安装心得

## 基于 Kolla-Ansible 的容器化部署方式

### 将 Kolla-Ansible 工具和镜像包和 CentOS 系统镜像一起打包，重新构建生成一个可引导的 .iso 镜像

## 下载 .iso 镜像文件

## 创建虚机,配建两个 host-only 的网络

### 

### 命令三连：prechecks、deploy、post-deploy，命令执行完成后，会在 /etc/kolla 目录下生成 admin-openrc.sh 文件，其中包含了登录所需要的用户名和密码信息，要使用 openstack 命令，必须先要安装各模块的客户端包。而我们的宿主机系统里面只安装了 Docker 和 Ansible。Kolla 构建的 docker 镜像中，已经在 openstack-base 这个基础镜像中安装了所有的客户端包。



#### OpenStack  架构复杂，配置较多，OpenStack 是一个基于 Python 的开源项目，意味着除了从软件包安装外，还会有使用 pip install 从 pypi 安装 Python 模块，以及从 git 源码仓库下载源码安装的需求 

#### 第一次启动过程会比较耗时，网络是大问题，即便网络状况很好，安装一切顺利，如果要反复多次搭建，每次都需要下载安装也会比较耗时间。







