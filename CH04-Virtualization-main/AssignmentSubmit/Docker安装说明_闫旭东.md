# Docker安装说明



**安装环境：**Windows 11 家庭版(版本:21H2)

###### 在Windows 10 2004上，微软引入了适用于Linux 2的Windows子系统（WSL 2），这是一个新的架构版本，该架构允许在Windows 10上运行Linux （使用轻量级虚拟机）。



**1.下载docker安装包**

https://www.docker.com/get-started

# <img src="http://xe0n.top/markdown/docker.png" alt="img" style="zoom: 30%;" />

**2.安装完成后可能会出现以下错误提示**

# <img src="http://xe0n.top/markdown/error1.png" alt="img" style="zoom: 30%;" />

https://docs.microsoft.com/zh-cn/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package

进入此链接进入官方文档，按照官方文档给出步骤进行操作

<font color='red'>Tips:windows商店安装Ubuntu后需要重启，否则无法设定linux账户的账号和密码</font>

**3.安装完成**

安装完成，推入自己所需的镜像。

###### ps:docker pull centos

# <img src="http://xe0n.top/markdown/d-finish.png" alt="img" style="zoom: 50%;" />