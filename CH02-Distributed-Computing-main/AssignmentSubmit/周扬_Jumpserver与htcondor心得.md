# 安装Jumpserver与htcondor心得

## 一.ubuntu安装jumpserver:

### jumpserver特点：
### 1.开源: 零门槛，线上快速获取和安装；

### 2.分布式: 轻松支持大规模并发访问；

### 3.无插件: 仅需浏览器，极致的 Web Terminal 使用体验；

### 4.多云支持: 一套系统，同时管理不同云上面的资产；

### 5.云端存储: 审计录像云端存储，永不丢失；

### 6.多租户: 一套系统，多个子公司和部门同时使用。



### 准备工作：

####  Linux系统：ubuntu18.04

####  配置好apt源

####  Linux网络通畅



###  1.准备 Python3 和 Python 虚拟环境

#### 1.1安装依赖包

#### 1.2安装 Python3.6

#### **1.3** 建立 Python 虚拟环境

#### **1.4** 自动载入 Python 虚拟环境配置

### 2.安装 Jumpserver

#### **2.1** 下载或 Clone 项目

#### **2.2** 安装依赖包

#### **2.3** 安装 Python 库依赖

#### **2.4** 安装 Redis, Jumpserver 使用 Redis 做 cache 和 celery broke

#### **2.5** 安装 MySQL

#### **2.6** 创建数据库 Jumpserver 并授权

#### **2.7** 修改 Jumpserver 配置文件

#### **2.8** 生成数据库表结构和初始化数据

#### **2.9** 运行 Jumpserver



## 二.安装htcondor：

```
cd /etc/yum.repos.d
wget http://research.cs.wisc.edu/htcondor/yum/repo.d/htcondor-stable-rhel6.repo
wget http://research.cs.wisc.edu/htcondor/yum/RPM-GPG-KEY-HTCondor
rpm --import RPM-GPG-KEY-HTCondor
yum install condor.x86_64
```







