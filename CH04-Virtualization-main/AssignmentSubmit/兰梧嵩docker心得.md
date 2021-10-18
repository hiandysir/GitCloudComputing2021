   本次进行的Docker的安装配置实验。根据在网上面查询的资料，我了解到想要在Win10上部署Docker必须先安装Linux的虚拟机，
实际上和在Ubnatu上安装Docker是一回事。我就跳过这一步直接进行Ubantu上的安装实验。而且根据我查阅资料发现安装Docker-desktop需要的Hyper V会与Vmware<font color=red size=3>不兼容</font>，为了接下来VMware的使用只能选择跳过Docker-desktop的安装。我在互联网上发现了一个更为便利的部署方式，只需要一行命令就能完成部署，点击[链接](https://www.runoob.com/docker/ubuntu-docker-install.html)
查看  
```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

```
若是没有安装curl需要先安装curl
```
sudo snap install curl  # version 7.79.1
```
这个脚本可以完成一键部署，只要运行脚本等待一段时间后便可完成安装。安装完成后运行个Hello World测试一下
```
docker run ubuntu:15.10 /bin/echo "Hello world"
```
在这里**ubuntu：15.10**指的是ubantu的镜像版本，若是主机上没有会自动帮你连接网络下载，我自己用的是ubantu20.04版本，但在这里使用15.10的镜像也能运行。当我试图使用20.04的镜像是也得连接服务器下载。
运行完成后会输出一行朴实无华的Hello World，代表你安装的Docker可以正常运行。

然后我们来通过建立容器来熟息Docker的使用。首先
```
docker pull alpine
```
来拉取一个容器。
```
docker images
```
可以查看本地的容器，记得images要加s。然后使用
```
docker run alpine ls -l
```
命令就可以让Docker run起来了。默认情况下完成运行容器就会自动退出了。但我这里需要手动
```
exit
```
退出，不然会跳提示。