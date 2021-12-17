# jumpserver
## 安装openssh-server
```command
## -y语句是默认同意安装
# 安装openssh-server
sudo apt install openssh-server -y
安装curl语法
sudo apt install curl  -y
```

## 安装配置jumpserver
```command
# 官方一键安装脚本
curl -sSL https://github.com/jumpserver/jumpserver/releases/download/v2.15.4/quick_start.sh | bash

# 配置文件
cd /opt/jumpserver/config
vim config.txt
```

## 启动jumpserver
```command
# 进入目录
cd /opt/jumpserver-installer-v2.15.4

# 启动
./jmsctl.sh start
```

## 访问jumpserver
```command
域名：
http://192.168.0.100:80
默认账号：admin
默认密码：admin
```

# HTCondor
## 安装步骤
按照官网教程[Install Instructions](https://research.cs.wisc.edu/htcondor/instructions/ubuntu/20/stable/)一步步安装即可