感謝：刘元坤

官方安裝文檔： https://docs.openstack.org/install-guide/

官方 DevStack 安裝 OpenStack https://kairen.gitbooks.io/openstack-centos/content/deployments/index.html 

其他參考：https://kairen.gitbooks.io/openstack-centos/content/deployments/index.html


# OpenStack 搭建

## 环境准备

    硬件环境
        确保CPU的VX-T已经打开，且Hyper-v已关闭
        内存 不小于8G
        硬盘空间 不小于50G
    软件环境
        VMWare Workstation、VBox 等虚拟化平台
    网络环境
        需要网络可以访问Google等

## 详细步骤

- 下载ubuntu镜像，推荐使用Ubuntu20.04LTS。下载地址为 [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)
- 打开虚拟机软件，安装Ubuntu。内存分配不小于8G，硬盘不小于50G，最小化安装。
- 依次运行下列命令

```shell
sudo apt update
sudo snap install microstack --beta --devmode
```

运行成功后显示：microstack (beta) ussuri from Canonical✓ installed

安装完成，开始配置：

- 初始化

```shell
sudo microstack init --auto --control
```

初始化完成

[![657qsO.png](https://z3.ax1x.com/2021/03/21/657qsO.png)](https://imgtu.com/i/657qsO)

- 获取管理界面密码，进入Web GUI界面

```shell
sudo snap get microstack config.credentials.keystone-password
```

如下图所示：

[![65buAH.png](https://z3.ax1x.com/2021/03/21/65buAH.png)](https://imgtu.com/i/65buAH)

打开浏览器，输入虚拟机IP地址即可进入

[![65btHg.png](https://z3.ax1x.com/2021/03/21/65btHg.png)](https://imgtu.com/i/65btHg)
用户名：admin
密码：刚刚获取的密码

登陆成功后

[![65b481.png](https://z3.ax1x.com/2021/03/21/65b481.png)](https://imgtu.com/i/65b481)

- test

```shell
microstack launch cirros --name test
```

运行成功后会提示进入test设备的方法，如下图所示：

[![65qurT.png](https://z3.ax1x.com/2021/03/21/65qurT.png)](https://imgtu.com/i/65qurT)
此时管理界面为：

[![65qqyV.png](https://z3.ax1x.com/2021/03/21/65qqyV.png)](https://imgtu.com/i/65qqyV)

## 安装完成 :)
