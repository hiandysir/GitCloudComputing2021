# OpenStack部署



### 1.概述

**什么是OpenStack？**
OpenStack是开源项目的集合，旨在协同工作以形成云的基础。OpenStack既可用于私有云，也可用于公共云。

**什么是微栈？**
MicroStack提供单节点或多节点OpenStack部署，可以直接在您的工作站上运行。虽然它是为开发人员制作原型和测试而设计的，但它也适用于边缘、物联网和设备。MicroStack是一个快速的OpenStack，这意味着所有OpenStack服务和支持库都打包在一个包中，可以轻松安装，升级或删除。MicroStack包括所有关键的OpenStack组件：Keystone，Nova，Neutron，Glance和Cinder。



### 2.安装MicroStack

从测试版渠道安装 MicroStack：

```
sudo snap install microstack --devmode --beta
```

安装过程完成后，您应该会在终端上看到以下消息：

```
microstack (beta) ussuri from Canonical✓ installed
```



### 3.初始化MicroStack

MicroStack需要初始化，以便配置网络和数据库。为此，请运行：

```
sudo microstack init --auto --control
```

一旦完成（15 - 20分钟），您的OpenStack云将启动并运行！



### 4.与OpenStack交互

**网页界面**
要通过 Web UI 与云进行交互，请访问 。http://10.20.20.1/

可以通过以下方式获取用户的密码：admin

```
sudo snap get microstack config.credentials.keystone-password
```

示例输出：

```
O9bqY6i7I8jCdqUFd1njLOTt1bKDd0H2
```

![image](https://user-images.githubusercontent.com/90243359/146749126-02d79287-cc5b-49f0-8724-4a94b0ad7a37.png)

键入凭据，然后按"登录"按钮：

#### ![image](https://user-images.githubusercontent.com/90243359/146749361-762300b0-e5bd-4952-9033-f00f65874949.png)

如果一切正常，您应该会看到着陆页：

![image](https://user-images.githubusercontent.com/90243359/146749657-28621bc6-3150-4e60-a439-e1f1a8a16b93.png)
