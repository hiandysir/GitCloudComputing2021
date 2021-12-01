# JUMPSERVER＆HTCONDOR安装心得

## 1.JUMPSERVER安装

​	[JumpServer - 开源堡垒机 - 官网](https://www.jumpserver.org/)

​	为了保证服务器安全，加个堡垒机，所有ssh连接都通过堡垒机来完成，堡垒机也需要有身份认证，授权，访问控制，审计等功能。

​	JumpServer 是一款由python编写开源的跳板机（堡垒机）系统，实现了跳板机应有的功能。基于ssh协议来管理，客户端无需安装agent。

### 	JUMPSERVER的一键部署

​		1.默认会安装到 /opt/jumpserver-installer-v2.15.0 目录。

   		curl -sSL https://github.com/jumpserver/jumpserver/releases/download/v2.15.0/quick_start.sh | bash cd /opt/jumpserver-installer-v2.15.

​		2.在这里会出现一个错误，Ubuntu20.0.3的系统里没有curl这个命令，因此我们还需要安装curl命令。
​		可以先更新一下apt：sudo apt-get update
​		然后再下载curl命令：sudo apt install curl
​		3.安装完成后配置文件 /opt/jumpserver/config/config.txt
​		cd /opt/jumpserver-installer-v2.15.0
​		启动：./jmsctl.sh start
​		停止：./jmsctl.sh stop
​		卸载：./jmsctl.sh uninstall
​		帮助：./jmsctl.sh -h
​		

## 2.HTOCONDOR的安装
​	[1.HTCondor官网](https://research.cs.wisc.edu/htcondor/)

​	[2.如何在ubuntu安装HTCondor](https://research.cs.wisc.edu/htcondor/instructions/ubuntu/20/stable/)

​	由于是先安装的堡垒机，堡垒机里面包含了许多我们要用到的程序或指令，这对我们安装HTCondor提供了很大的便捷。只需要按照教程2里面的链接一步步进行部署，就能顺利完成安装。
