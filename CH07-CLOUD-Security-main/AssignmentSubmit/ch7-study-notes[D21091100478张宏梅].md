---
CloudComputing2021
ch7 study notes
Author：zoxiii
---

[TOC]


# 1、练习1 Editing Mac address table overflow attack
## （1）安装git

```c
# yum install git
```
## （2）克隆源码

```c
# git clone https://huangty@bitbucket.org/huangty/cs144_security.git
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6b16e09b60334826bdbdfc772487dfaa.png)

```c
# cd cs144_security  ## 进入该文件夹
# ln -s ~/pox/    ## 将POX链接到目录中
# bash ./config.sh   ## 环境配置
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/78e1cfac6e764d238621ac276c63592d.png)

## （3）Attack Demonstration
```c
# ./run_pox.sh   ## 启动POX网络控制器 
# ./run.sh   ## 启动Mininet仿真 
```

