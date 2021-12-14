# OpenStack搭建



[TOC]



## 1. OpenStack介绍

OpenStack 平台是一款云计算平台，能够对行业标准硬件上的资源进行虚拟化，并将虚拟化后的资源整理到云中进行管理，让用户能够随时访问所需的内容。



## 2. OpenStack服务

| 服务                  | 项目名称  | 描述                                                         |
| :-------------------- | :-------- | :----------------------------------------------------------- |
| Dashboard             | Horizon   | 提供了一个基于web的自主服务门户，与OpenStack底层服务交互，诸如启动一个实例，分配IP地址以及配置访问控制。 |
| Computer              | Nova      | 在OpenStack环境中计算实例的生命周期管理。按需响应包括生成、调度、回收虚拟机等操作。 |
| Network               | Neutron   | 确保为其它OpenStack服务提供网络连接即服务，比如OpenStack计算。为用户提供API定义网络和使用。基于插件的架构其支持众多的网络提供商和技术。 |
| Objecet Storage       | Swift     | 通过一个 RESTful,基于HTTP的应用程序接口存储和任意检索的非结构化数据对象。它拥有高容错机制，基于数据复制和可扩展架构。它的实现并像是一个文件服务器需要挂载目录。在此种方式下，它写入对象和文件到多个硬盘中，以确保数据是在集群内跨服务器的多份复制。 |
| Block Storage         | Cinder    | 为运行实例而提供的持久性块存储。它的可插拔驱动架构的功能有助于创建和管理块存储设备。 |
| Identity Service      | Keystone  | 为其他OpenStack服务提供认证和授权服务，为所有的OpenStack服务提供一个端点目录。 |
| Image Service         | Glance    | 存储和检索虚拟机磁盘镜像，OpenStack计算会在实例部署时使用此服务。 |
| Telemetry Service     | Ceilmeter | 为OpenStack云的计费、基准、扩展性以及统计等目的提供监测和计量。 |
| Orchestration Service | Heat      | 既可以使用本地模板格式，亦可使用AWS CloudFormation模板格式，来编排多个综合的云应用，通过OpenStack本地REST API或者是CloudFormation相兼容的队列API。 |



## 3. 通过 packstack安装openstack



- 安装openstack-packstack  rpm包

```
sudo yum install -y https://www.rdoproject.org/repos/rdo-release.rpm
sudo yum update -y
sudo yum install -y openstack-packstack
```



- 在packstack命令后，使用—allinone 参数在本机上部署所有服务。


```
packstack --allinone
```

Packstack在部署完成后在终端上会输出以下信息：

```
 **** Installation completed successfully ******
```

- 登录dashboard 验证


http://192.168.100.130/dashboard



![](http://m.qpic.cn/psc?/V51qUp8Q4ORGKu4Dhczg2JyCx12uI1yY/45NBuzDIW489QBoVep5mcXIkMCQJPU0uBJl8TY9ZXIzveL8Qq3L1WIP5dVdgSII5.wVtuXV*xlK7Yl3kVV74OZQJgJHzkdX5dCrM.uNfOTM!/b&bo=9wThAgAAAAADFyI!&rf=viewer_4)

![](http://m.qpic.cn/psc?/V51qUp8Q4ORGKu4Dhczg2JyCx12uI1yY/45NBuzDIW489QBoVep5mcZ953TUyQ91aagjbIcn5EONBtUR1DfWfIgynO33cH*.cr7.HsKh8oP0iGB2qIvlM4HLrCnxp974CEdy98yw57DI!/b&bo=GgflAwAAAAADF8k!&rf=viewer_4)





## 4. 常用命令

- 查看用户


```
# openstack user list 
```

- 查看项目


```
# openstack project list 
```

- 查看服务


```
# openstack service list 
```

