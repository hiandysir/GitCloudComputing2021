# HTCONDER 安装心得

* HTCONDER的安装方便，当前使用vmware的centos7系统来进行安装，由于系统已经安装完毕curl命令，所以直接下载htconder即可。

* 在执行curl -fsSL https://get.htcondor.org | sudo /bin/bash -s -- --no-dry-run 命令时，由于不是直接复制命令，而是手打，导致命令输入错误，出现host未响应的错误，之后安装了vmware tools，直接将命令复制运行，可以成功下载。

* 在安装完成htconder后，输入condor_status以及condor_q可以查看当前的状态等等，成功安装。



# JUMPSERVER 安装心得

##  搭建环境

* 首先在为centos7的系统更新命令中，出现命令卡死的情况，因为长时间的未响应，就将命令ctrl+c强制中断，后检查了dns与虚拟机的网络情况后，再次运行update命令，将源换成国内（如aliyun等）能够加快下载速度。

* 安装jumpserver使用的是一键部署，所以mysql、redis等等都不需要提前准备，直接运行一键部署命令即可 。

## 安装过程

* 执行安装命令时，出现curl的（35）错误，即git网站没有回应。在查询相关资料后，https容易出现无法连接的情况，将其更改为git@后，安装jumpserver命令可以正常执行。

* 安装jumpserver成功后，可以看到访问jumpserver的web地址以及默认用户与密码，SSH/SFTP访问等等信息。

* 配置jumpserver文件时，执行命令/opt/jumpserver/config/config.txt会提示权限不够，进入该文件进行chmod 777 +文件名可修改权限，解决问题。

## 启动过程

* 使用命令./jmsctl.sh start启动jumpserver时，需要启动jms_core、web、lion等等。此时出现过容器不健康的错误，在重新启动docker后，能够正确创建，错误消失。



## 总结

在解决完以上问题后，可以成功进入jumpserver控制web页面，能够管理用户的各种信息数据等等，从管理页面可以看出，jumpserver是个强大的开源堡垒机。



