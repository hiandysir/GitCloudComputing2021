## Openstark安装心得

OpenStack ：是一个由NASA和Rackspace合作研发并发起的，以Apache许可证授权的自由软件和开放源代码项目。项目目标是提供实施简单、可大规模扩展、丰富、标准统一的云计算管理平台。OpenStack通过各种互补的服务提供了基础设施即服务（IaaS）的解决方案，每个服务提供API以进行集成。OpenStack是用Python编程语言编写的。

**安装方式：**

1．DevStack 在相当长一段时间内，DevStack仍将是众多开发者的首选安装工具。该方式主要是通过配置一个安装脚本，执行Shell命令来安装OpenStack的开发环境，支持CentOS、Debian等系列系统。

#### 安装环境

- 工具：VMware Workstation 16 Pro
- 操作系统：Ubuntu 20.04.2
- 虚拟机配置，内存 8G、处理器 6C、磁盘 80G、开启虚拟化引擎

开始安装

输入指令安装

```
sudo apt update
sudo snap install microstack --beta --devmode
```

初始化

```
sudo microstack init --auto --control
```

获取密匙

```
sudo snap get microstack config.credentials.keystone-password
```

登录OpenStark