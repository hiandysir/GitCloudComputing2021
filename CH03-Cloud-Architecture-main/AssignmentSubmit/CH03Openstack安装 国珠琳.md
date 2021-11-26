# Openstack安装

### 环境准备

```
硬件环境
    确保CPU的VX-T已经打开，且Hyper-v已关闭
    内存 不小于8G
    硬盘空间 不小于50G
软件环境
    VMWare Workstation、VBox 等虚拟化平台
网络环境
    需要网络可以访问Google等
```



### 详细步骤

运行以下命令

```
sudo apt update
sudo snap install microstack --beta --devmode
```

运行成功后显示：microstack (beta) ussuri from Canonical✓ installed

初始化

```
sudo microstack init --auto --control
```

获取管理界面密码，进入Web GUI界面

```
sudo snap get microstack config.credentials.keystone-password
```





打开浏览器，输入虚拟机IP地址时出错了。（其实前面就已经报错了，没有注意到，到这里才发现错误）

![](https://raw.githubusercontent.com/kwokCL/picture/main/gzl/image-20211122204533277.png))

经检查发现安装时给虚拟机的内存太小了，只有4G。重新设置给虚拟机8G后，成功运行。

![](https://raw.githubusercontent.com/kwokCL/picture/main/gzl/image-20211122234631199.png))

ip:192.168.227.131

用户名：amdin

密钥：kiDZ39Wc04ViiBahfhrqL6acBiyTELp5







![](https://raw.githubusercontent.com/kwokCL/picture/main/gzl/image-20211122234226579.png))



登陆成功后：

![](https://raw.githubusercontent.com/kwokCL/picture/main/gzl/image-20211122234435192.png))