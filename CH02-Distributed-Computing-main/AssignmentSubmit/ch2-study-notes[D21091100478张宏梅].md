---
CloudComputing2021
ch2 study notes
Author：zoxiii
---

[TOC]

> 未发布blog，主要是因为这个东西虽然做了，但总觉得还是没有很了解，只是安装，但并没有应用过它。

# 1、高通量计算HTCondor

## 1.1 下载

（1）[HTCondor官网](https://research.cs.wisc.edu/htcondor/)

![](https://img-blog.csdnimg.cn/2b8f28eef7dd4a5bb339bab391c167c1.png)

（2）[下载地址](https://research.cs.wisc.edu/htcondor/tarball/stable/)

![](https://img-blog.csdnimg.cn/44a72b5c18d540618607cab7f62d9cb0.png)

## 1.2 安装

（1）使用XShell将压缩包传输到虚拟机中

![](https://img-blog.csdnimg.cn/38c48e5c514a410bafa1b0d17bf0eff1.png)

（2）解压文件

![](https://img-blog.csdnimg.cn/665c6c6d3e6b4a02b4aabdd35e01df6f.png)

（3）打开终端，安装Condor

```bash
./condor_install --prefix=/home/Condor/prefix --local-dir=/home/Condor/local --type=manager,execute --owner=zoxiii
```

![](https://img-blog.csdnimg.cn/b9fb90e8c81d4587ab68fae80c0ba775.png)

（4）启动Condor Web服务

```bash
vi /home/Condor/prefix/etc/condor_config
```

```bash
RELEASE_DIR = /home/Condor/prefix
WEB_ROOT_DIR = $(RELEASE_DIR)/web
ENABLE_SOAP = TRUE
ENABLE_WEB_SERVER = TRUE
```

![](https://img-blog.csdnimg.cn/0ba542848869499e85f9a1228cd28ebe.png)



（5）安装成功

```bash
## 安装epel资料库
yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
 
## 安装HTCondor资料库
yum install -y https://research.cs.wisc.edu/htcondor/repo/8.8/el7/release/htcondor-release-8.8-1.el7.noarch.rpm
 
## 安装HTCondeor(minicondor还是condor看是在集群上还是单机器上）
yum install -y minicondor # or condor
 
## 运行
systemctl start condor # 通过系统启动condor
systemctl enable condor # 启动默认运行
```

![](https://img-blog.csdnimg.cn/22cdd97e38f3434b9e53472bba5698f8.png)

（6）查看状态

```bash
## 检查是否运行成功
condor_q #列出当前池中任务
condor_status #列出当前池中节点
systemctl #检查condor的系统状态
```

![](https://img-blog.csdnimg.cn/60c050a886b64c24a0197d6c5804f754.png)

## 1.3 测试

（1）编写文件【sleep.sh】

```bash
#!/bin/bash
# file name: sleep.sh

TIMETOWAIT="6"
echo "sleeping for $TIMETOWAIT seconds"
/bin/sleep $TIMETOWAIT
```

（2）编写文件【sleep.sub】

```bash
# sleep.sub -- simple sleep job

executable              = sleep.sh
log                     = sleep.log
output                  = outfile.txt
error                   = errors.txt
should_transfer_files   = Yes
when_to_transfer_output = ON_EXIT
queue
```

（3）提交作业

```c
condor_submit sleep.sub
```

![](https://img-blog.csdnimg.cn/7cd070a0c1d044a3a209c051452ee69e.png)

（4）得到结果文件

![](https://img-blog.csdnimg.cn/7a2fa7176e414d55a16f37dd11beb006.png)



（5）查看任务状态

```bash
condor_q       #列出当前池中任务
```

![](https://img-blog.csdnimg.cn/3c3bb684817c428b8ff4f1aaef592d0c.png)

（6）删除该集群中的所有作业

```bash
condor_rm 1
```

![](https://img-blog.csdnimg.cn/f35574d6237d45d1b436d4b2d887789d.png)

（7）查看历史任务

```bash
condor_history
```

![](https://img-blog.csdnimg.cn/d1e37a1c65474510ba2d5cb16f3109a4.png)


# 2、堡垒机JumpServer
## 2.1 学习文档

[官方文档](https://docs.jumpserver.org/zh/master/)

## 2.2 安装

（1）部署NFS服务

```c
yum -y install epel-release
```

（2）手动部署

```c
cd /opt
```

```c
wget https://github.com/jumpserver/installer/releases/download/v2.14.0/jumpserver-installer-v2.14.0.tar.gz
```

```c
tar -xf jumpserver-installer-v2.14.0.tar.gz
```

（3）进入文件夹下
```c
cd jumpserver-installer-v2.14.0
```

（4）安装jumpserver
```c
./jmsctl.sh install
```

![](https://img-blog.csdnimg.cn/bc0b5938ef6f4c979397f37e0bf03ee0.png)

![](https://img-blog.csdnimg.cn/d0a958b44c0a400a819cea9f2f6c4390.png)

![](https://img-blog.csdnimg.cn/264c6080d1fc442c85328e6ca7c4593e.png)

![](https://img-blog.csdnimg.cn/bb987b654e9c4d519244c12357731bea.png)

![](https://img-blog.csdnimg.cn/8728552bf6c248d6845af13fb2e366f0.png)

（5）开启jumpserver

```c
./jmsctl.sh start
```

![](https://img-blog.csdnimg.cn/4afca9b8b3474eb3a954e3787bf962d6.png)

（6）停机jumpserver

```c
./jmsctl.sh down
```

![](https://img-blog.csdnimg.cn/bf1ecb3856694eae89a15ec1af4606cb.png)

## 2.3 在多台机器上安装

![](https://img-blog.csdnimg.cn/7d1a8c741b2e439482f347a9c6a31b44.png)

