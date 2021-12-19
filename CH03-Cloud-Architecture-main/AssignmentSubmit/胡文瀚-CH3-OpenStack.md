# OpenStack

## 安装microstack
```command
# 进入管理员模式
sudo -i

# 下载安装
snap install microstack --beta --devmode

# 初始化
microstack init --auto --control

# 配置开机启动
snap enable microstack
```

## 查看情况
```command
# 查看网络
microstack.openstack network list

# 查看配置
snap get microstack config.credentials.keystone-password
```

## 访问
```command
# 内网IP
http://192.168.0.76/
```