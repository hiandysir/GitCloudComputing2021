# JumpServer安装使用心得
[TOC]
## 1. 概要
以下是JumpServer官网对于该软件的[总体介绍][官方文档]
>JumpServer 是全球首款开源的堡垒机，使用 GNU GPL v2.0 开源协议，是符合 4A 规范的运维安全审计系统。
>JumpServer 使用 Python / Django 为主进行开发，遵循 Web 2.0 规范，配备了业界领先的 Web Terminal 方案，交互界面美观、用户体验好。
>JumpServer 采纳分布式架构，支持多机房跨区域部署，支持横向扩展，无资产数量及并发限制。

由上述介绍可知JumpServer为开源堡垒机且采用了分布式架构，下面[百度百科中对于堡垒机的解释][百度百科堡垒机]

>堡垒机，即在一个特定的网络环境下，为了保障网络和数据不受来自外部和内部用户的入侵和破坏，而运用各种技术手段监控和记录运维人员对网络内的服务器、网络设备、安全设备、数据库等设备的操作行为，以便集中报警、及时处理及审计定责。

总的来说，堡垒机就是用来控制哪些人可以登录哪些资产（事先防范和事中控制），以及录像记录登录资产后做了什么事情（事后溯源）。

它具有核心功能 4A：

- 身份验证 Authentication
- 账号管理 Account
- 授权控制 Authorization
- 安全审计 Audit

## 2. 安装
### (1) 安装准备
这里安装步骤是根据JumpServer官方文档所写进行，详情可打开[官方文档][]查看。这里我将采用一键部署的方式进行安装。
安装环境：CentOS-7-x86_64-Everything-2009
### (2) 开始安装

打开终端输入以下指令。

```
curl -sSL https://github.com/jumpserver/jumpserver/releases/download/v2.15.3/quick_start.sh | bash
```

这里遇到了网络问题，尝试多次连接即可。

另外还遇到了权限问题，这里使用下面指令转换为管理员（root）。

```
su -
```

东西有点多，可能需要下载一定时间，最后效果如下：

![CloudComputeJumpServerInstall-1.png](https://raw.githubusercontent.com/Alvin-Gongwang/notebook-img/main/CityuCourse/CloudComputeJumpServerInstall-1.png)

![CloudComputeJumpServerInstall-2.png](https://raw.githubusercontent.com/Alvin-Gongwang/notebook-img/main/CityuCourse/CloudComputeJumpServerInstall-2.png)
这里要注意的是系统默认会安装到 /opt/jumpserver-installer-v2.15.3 目录

### (3) 简单测试
转到安装好的目录
```
cd /opt/jumpserver-installer-v2.15.3
```
跳转后直接启动服务，这里会有点慢，而且可能会出现Unhealty的问题，这里我的问题应该是网络问题导致的，多试几次即可，另外当start失败后，成功的容器会留着系统中，若要停止一定要使用down进行去除。启动成功后图如下：
![CloudComputeJumpServerInstall-3.png](https://raw.githubusercontent.com/Alvin-Gongwang/notebook-img/main/CityuCourse/CloudComputeJumpServerInstall-3.png)
根据文档其余操作如下：
```
# 启动
./jmsctl.sh start
# 停止
./jmsctl.sh down
# 卸载
./jmsctl.sh uninstall
# 帮助
./jmsctl.sh -h
```
注意：这里我并没有配置相关配置文件
随后打开浏览器输入localhost.com（默认）即可，会出现如下图情形说明安装时成功的。
![CloudComputeJumpServerInstall-4.png](https://raw.githubusercontent.com/Alvin-Gongwang/notebook-img/main/CityuCourse/CloudComputeJumpServerInstall-4.png)

至此简单测试成功。
## 3. 简单使用
简单思路下配置资产并分配用户相关权限即可正常使用。
###  (1) 创建账户
点击用户列表选择创建，填入相关信息，保存提交即可，如下图：
![CloudComputeJumpServerInstall-5.png](https://raw.githubusercontent.com/Alvin-Gongwang/notebook-img/main/CityuCourse/CloudComputeJumpServerInstall-5.png)
这里需要解释的两个选项
1. 密码策略：这里有两个可选，如果选择邮箱，密码相关会通过邮件发送，第二个选项择是直接设置。
2. 系统角色：管理员不在解释，用户就是普通权限账户，审计员是类似查看日志维护的职位。
###  (2) 添加资产
资产添加同账户差不多，大致流程如下图，另资产树节点可设置默认。
![CloudComputeJumpServerInstall-7.png](https://raw.githubusercontent.com/Alvin-Gongwang/notebook-img/main/CityuCourse/CloudComputeJumpServerInstall-7.png)
值得说的是这里的资产涉及面很广，那些东西能称为资产可见下图：
![CloudComputeJumpServerInstall-6.png](https://raw.githubusercontent.com/Alvin-Gongwang/notebook-img/main/CityuCourse/CloudComputeJumpServerInstall-6.png)
###  (3) 使用资产
资产使用方式有两种，一是web终端访问，二是通过客户端访问，这里讲第一种方式：
登录账户后选择右上角web终端后选择相关资产即可进行操作。
![CloudComputeJumpServerInstall-8.png](https://raw.githubusercontent.com/Alvin-Gongwang/notebook-img/main/CityuCourse/CloudComputeJumpServerInstall-8.png)
![CloudComputeJumpServerInstall-9.png](https://raw.githubusercontent.com/Alvin-Gongwang/notebook-img/main/CityuCourse/CloudComputeJumpServerInstall-9.png)
更多操作可查看[官方文档][官方文档]，也可查看[官方教程视频][官方教程视频]。

## 5.相关链接

官方文档:  https://docs.jumpserver.org/zh/master/
百度百科堡垒机:  https://baike.baidu.com/item/%E5%A0%A1%E5%9E%92%E6%9C%BA/2196549?fr=aladdin
官方教程视频: https://www.bilibili.com/video/BV19D4y1S7s4
[官方文档]: https://docs.jumpserver.org/zh/master/
[百度百科堡垒机]: https://baike.baidu.com/item/%E5%A0%A1%E5%9E%92%E6%9C%BA/2196549?fr=aladdin
[官方教程视频]:https://www.bilibili.com/video/BV19D4y1S7s4