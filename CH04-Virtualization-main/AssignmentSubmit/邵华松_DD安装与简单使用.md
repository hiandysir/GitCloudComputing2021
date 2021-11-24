## Docker Desktop安装与简单使用

### Window下Docker Desktop安装

安装包下载地址：https://hub.docker.com/editions/community/docker-ce-desktop-windows

Docker依赖于已存在并运行的Linux内核环境，对于部分版本的Win10系统，可以选择安装Hyper-V满足环境依赖，参考：https://www.runoob.com/docker/windows-docker-install.html



安装完成后启动Docker Desktop，遇到WSL2未完全安装的错误，选择更新并配置WSL2

更新WSL2 Linux kernel：https://docs.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package

选择以管理员权限打开一个PowerShell窗口

输入并重启：dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

输入并重启：dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

输入并重启：wsl --set-default-version 2



打开Microsoft Store，选择偏好的Linux分发版，这里选择Ubuntu

获取后启动，遇到The attempted operation is not supported for the type of object referenced（参考的对象类型不支持尝试的操作）

以管理员身份运行CMD

输入并重启：netsh winsock reset

随后正常启动Docker Desktop，运行“docker run hello-world”测试

![Docker Hello World](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/dockerHellomd4.png)



### MacOS下Docker Desktop安装

MacOS下直接使用Homebrew安装，打开Terminal

输入：brew install docker

或者选择网页下载，地址：https://docs.docker.com/desktop/mac/install/

MacOS基于XNU混合内核，基本不需要配置环境依赖，经测试正常运行

![MacOS Docker Desktop](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/macOSDockermd4.png)



### Docker Desktop的简单使用（以Ubuntu为例）

拉取镜像：docker pull ubuntu:16.04（拉取Docker Hub上ubuntu仓库中tag为16.04的镜像，若无指定tag则拉取tag为latest的镜像）

启动容器：docker run -it ubuntu:16.04（-i保持标准输入打开状态，-t分配一个伪tty）

保存容器：docker commit {CONTAINER ID} ubuntu:name（保存CONTAINER ID对应的容器在ubuntu下，tag为name）

目前已经通过Docker搭建完全分布式Hadoop集群



### 有关Docker Desktop的进一步学习

Docker可以通过阅读DockerFile自动创建并配置镜像，本质上是一个包含命令的文本

有关DockerFile的编写：https://docs.docker.com/engine/reference/builder/
