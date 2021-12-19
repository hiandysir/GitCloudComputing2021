---
CloudComputing2021
ch6 study notes
Author：zoxiii
---

[TOC]


# 1、安装Mininet
## （1）安装git

```c
# yum install git
```
## （2）克隆mininet源码

```c
# git clone https://github.com/mininet/mininet
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/0ddd7db2dd1147c9a66814ff52043b6d.png)
## （3）安装 mininet

```c
# mininet/util/install.sh -n3V 2.5.0
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/f0d9c02e093b43809ba18d6ad295f790.png)

## （4）测试 Mininet

```c
# mn --test pingall   ## 验证所有结点的连通性
```
`bug`：由于未安装python所以失败

