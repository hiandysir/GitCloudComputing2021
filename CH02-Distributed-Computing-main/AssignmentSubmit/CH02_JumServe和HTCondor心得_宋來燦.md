# JumServe和HTCondor的安裝與使用心得__宋來燦_



## JumpServe安裝

安装环境：CentOS7(使用yum进行安装)

在网络上找到的额JumpServe安装方式有:

* Docker安装
* 手动安装

首先尝试手动安装，并未成功，于是采用了Docker安装的方式

### STEP1

阿里云配置源

```
# step 1:安装必要的一些系统工具 
sudo yum install -y yum-utils device-mapper-persistent-data 1vm2
# Step 2:添加软件源信息
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/l inux/centos/docker-ce.repo
# Step 3:更新并安装Docker-CE
sudo yum makecache fast
sudo yum -y install docker-ce
# step 4:开启Docker服务
sudo service docker start
```



### STEP2:自动执行命令，镜像安装

```
# 需要先定义两个外部变量 KEY和TOKEN
if [ ! "$SECRET KEY" ]; then
  SECRET KEY= `cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 50`;
  echo "SECRET_KEY=$SECRET_KEY" >> ~/.bashrc;
  echo $SECRET_KEY;
else
  echo $SECRET_KEY;
fi
if [ ! "$BOOTSTRAP_TOKEN" ]; then
  BOOTSTRAP_TOKEN=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 16` ;
  echo "BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN" >> ~/.bashrc;
  echo $BOOTSTRAP_TOKEN;
else
  echo $BOOTSTRAP_TOKEN;
fi

```

###  STEP3:直接执行官方命令

```
 -p 80:80 -p 2222:2222 \
 -e SECRET_KEY=$SECRET_KET \
 -e BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN \
 jumpserver/jms_all:latest
```







# HTCond安装--官网安装

* 环境配置：Ubuntu+Linux 且必须以根目录进行安装

* 安装过程：
  + 执行命令1：apt-get update && apt-get install -y curl
  + 执行命令2：curl -fsSL https://get.htcondor.org | sudo /bin/bash -s -- --no-dry-run
  + 完成安装



