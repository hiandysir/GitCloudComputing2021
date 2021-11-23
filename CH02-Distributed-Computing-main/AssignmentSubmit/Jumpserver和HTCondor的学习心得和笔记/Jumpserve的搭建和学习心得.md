# Jumpserve的搭建和学习心得

author:Damon

参考连接：https://docs.jumpserver.org/zh/master/install/setup_by_fast/

## 什么是JumpServer？

> JumpServer 是全球首款开源的堡垒机，使用 GNU GPL v2.0 开源协议，是符合 4A 规范的运维安全审计系统。
>
> JumpServer 使用 Python 开发，遵循 Web 2.0 规范，配备了业界领先的 Web Terminal 方案，交互界面美观、用户体验好。
>
> JumpServer 采纳分布式架构，支持多机房跨区域部署，支持横向扩展，无资产数量及并发限制。  

## 使用JumpServer的优势

>开源: 零门槛，线上快速获取和安装；
>
>分布式: 轻松支持大规模并发访问；
>
>无插件: 仅需浏览器，极致的 Web Terminal 使用体验；
>
>多云支持: 一套系统，同时管理不同云上面的资产；
>
>云端存储: 审计录像云端存储，永不丢失；
>
>多租户: 一套系统，多个子公司和部门同时使用；
>
>多应用支持: 数据库，Windows远程应用，Kuberne  

下面是我安装的源码，仅供大家参考:

尝试了两种方法，

第一种：一键部署

```shell
#安装环境ubuntu20.4
#使用官方文档一键部署，这个用时十分久，估计等了两个多小时，才部署完成
#官方一键部署是基于docker,在docker安装了nginx,redis,mysql,web,core,koko,lion
root@damon:~# curl -sSL https://github.com/jumpserver/jumpserver/releases/download/v2.15.4/quick_start.sh | bash
 * Support:        https://ubuntu.com/advantage

Welcome to Alibaba Cloud Elastic Compute Service !

/usr/bin/xauth:  file /root/.Xauthority does not exist
root@damon:~# curl -sSL https://github.com/jumpserver/jumpserver/releases/download/v2.15.4/quick_start.sh | bash
curl: (52) Empty reply from server
root@damon:~# curl -sSL https://github.com/jumpserver/jumpserver/releases/download/v2.15.4/quick_start.sh | bash
E: Package 'zip' has no installation candidate
E: Package 'python' has no installation candidate
download install script to /opt/jumpserver-installer-v2.15.4 (开始下载安装脚本到 /opt/jumpserver-installer-v2.15.4)


       ██╗██╗   ██╗███╗   ███╗██████╗ ███████╗███████╗██████╗ ██╗   ██╗███████╗██████╗
       ██║██║   ██║████╗ ████║██╔══██╗██╔════╝██╔════╝██╔══██╗██║   ██║██╔════╝██╔══██╗
       ██║██║   ██║██╔████╔██║██████╔╝███████╗█████╗  ██████╔╝██║   ██║█████╗  ██████╔╝
  ██   ██║██║   ██║██║╚██╔╝██║██╔═══╝ ╚════██║██╔══╝  ██╔══██╗╚██╗ ██╔╝██╔══╝  ██╔══██╗
  ╚█████╔╝╚██████╔╝██║ ╚═╝ ██║██║     ███████║███████╗██║  ██║ ╚████╔╝ ███████╗██║  ██║
   ╚════╝  ╚═════╝ ╚═╝     ╚═╝╚═╝     ╚══════╝╚══════╝╚═╝  ╚═╝  ╚═══╝  ╚══════╝╚═╝  ╚═╝

								   Version:  v2.15.4  

....
....
....
....
>>> The Installation is Complete
1. You can use the following command to start, and then visit
cd /opt/jumpserver-installer-v2.15.4
./jmsctl.sh start

2. Other management commands
./jmsctl.sh stop
./jmsctl.sh restart
./jmsctl.sh backup
./jmsctl.sh upgrade
For more commands, you can enter ./jmsctl.sh --help to understand

3. Web access
http://172.25.3.100:80
Default username: admin  Default password: admin

4. SSH/SFTP access
ssh -p2222 admin@172.25.3.100
sftp -P2222 admin@172.25.3.100

5. More information
Official Website: https://www.jumpserver.org/
Documentation: https://docs.jumpserver.org/


root@damon:~# cd /opt/jumpserver-installer-v2.15.4
root@damon:/opt/jumpserver-installer-v2.15.4# ./jmsctl.sh start

WARNING: The HOSTNAME variable is not set. Defaulting to a blank string.
jms_redis is up-to-date
jms_mysql is up-to-date
Creating jms_core ... done
Creating jms_lion   ... done
Creating jms_web    ... done
Creating jms_celery ... done
Creating jms_koko   ... done





```





第二种手动部署:手动的话，比一键部署快一些。部署的内容也是差不多的

```shell
root@damon:~# cd /opt
root@damon:/opt# wget https://github.com/jumpserver/installer/releases/download/v2.15.4/jumpserver-installer-v2.15.4.tar.gz
--2021-11-17 01:44:28--  https://github.com/jumpserver/installer/releases/download/v2.15.4/jumpserver-installer-v2.15.4.tar.gz
Resolving github.com (github.com)... 20.205.243.166
Connecting to github.com (github.com)|20.205.243.166|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://github-releases.githubusercontent.com/303679235/a764fbc0-dbee-4300-b6d4-fa4cb8598701?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20211116%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20211116T174428Z&X-Amz-Expires=300&X-Amz-Signature=698b5a71c5b1cdd07b8d8668f3e761393a169f103cfc26397c13245946cfe86d&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=303679235&response-content-disposition=attachment%3B%20filename%3Djumpserver-installer-v2.15.4.tar.gz&response-content-type=application%2Foctet-stream [following]
--2021-11-17 01:44:28--  https://github-releases.githubusercontent.com/303679235/a764fbc0-dbee-4300-b6d4-fa4cb8598701?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20211116%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20211116T174428Z&X-Amz-Expires=300&X-Amz-Signature=698b5a71c5b1cdd07b8d8668f3e761393a169f103cfc26397c13245946cfe86d&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=303679235&response-content-disposition=attachment%3B%20filename%3Djumpserver-installer-v2.15.4.tar.gz&response-content-type=application%2Foctet-stream
Resolving github-releases.githubusercontent.com (github-releases.githubusercontent.com)... 185.199.108.154, 185.199.109.154, 185.199.111.154, ...
Connecting to github-releases.githubusercontent.com (github-releases.githubusercontent.com)|185.199.108.154|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 34029 (33K) [application/octet-stream]
Saving to: ‘jumpserver-installer-v2.15.4.tar.gz’

jumpserver-installer-v2.15.4.tar.gz            100%[===================================================================================================>]  33.23K  --.-KB/s    in 0.001s  

2021-11-17 01:44:29 (26.9 MB/s) - ‘jumpserver-installer-v2.15.4.tar.gz’ saved [34029/34029]

root@damon:/opt# tar -xf jumpserver-installer-v2.15.4.tar.gz
root@damon:/opt# cd jumpserver-installer-v2.15.4
root@damon:/opt/jumpserver-installer-v2.15.4# ./jmsctl.sh install


       ██╗██╗   ██╗███╗   ███╗██████╗ ███████╗███████╗██████╗ ██╗   ██╗███████╗██████╗
       ██║██║   ██║████╗ ████║██╔══██╗██╔════╝██╔════╝██╔══██╗██║   ██║██╔════╝██╔══██╗
       ██║██║   ██║██╔████╔██║██████╔╝███████╗█████╗  ██████╔╝██║   ██║█████╗  ██████╔╝
  ██   ██║██║   ██║██║╚██╔╝██║██╔═══╝ ╚════██║██╔══╝  ██╔══██╗╚██╗ ██╔╝██╔══╝  ██╔══██╗
  ╚█████╔╝╚██████╔╝██║ ╚═╝ ██║██║     ███████║███████╗██║  ██║ ╚████╔╝ ███████╗██║  ██║
   ╚════╝  ╚═════╝ ╚═╝     ╚═╝╚═╝     ╚══════╝╚══════╝╚═╝  ╚═╝  ╚═══╝  ╚══════╝╚═╝  ╚═╝

								   Version:  v2.15.4  

1. Check Configuration File
Path to Configuration file: /opt/jumpserver/config
/opt/jumpserver/config/config.txt  [ √ ]
/opt/jumpserver/config/nginx/lb_rdp_server.conf  [ √ ]
/opt/jumpserver/config/nginx/lb_ssh_server.conf  [ √ ]
/opt/jumpserver/config/nginx/cert/server.crt   [ √ ]
/opt/jumpserver/config/nginx/cert/server.key   [ √ ]
complete

2. Backup Configuration File
Back up to /opt/jumpserver/config/backup/config.txt.2021-11-17_01-47-12
complete
...
...
...

```

