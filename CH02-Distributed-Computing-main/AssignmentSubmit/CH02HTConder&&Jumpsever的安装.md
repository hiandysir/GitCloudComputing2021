## **安装HTConder**

作者：陈淇舒

根据官方安装文档：https://htcondor.readthedocs.io/en/latest/getting-htcondor/install-linux-as-root.html

1. Linux（以根用户身份）因为没有进入根目录敲命令提示错误

   ```
   E:Could not open lock file /var/lib/dpkg/lock-fror ntend - open(13: Permission denied) 
   E:Unable to acquire the dpkg frontend lock(/var/1.ib/dpkg/lock-frontend), are you root?
   ```

   必须进入根目录（命令为su root）

```
su root
```

​                         

2.由于Ubuntu容器没curl安装

```
apt-get update && apt-get install -y curl
```



3.配置网络

```
vim /etc/netplan/01-network-manager-all.yml
```

```
# Let NetworkManager manage all devices on this system
network:
  version: 2
  renderer: NetworkManager
  ethernets:
          ens33:
                  dhcp4: no
                  dhcp6: no
                  addresses: [192.168.174.102/24]
                  gateway4: 192.168.174.2
                  nameservers:
                          addresses: [114.114.114.114, 8.8.8.8] 
```

让命令生效

```
netplan apply
```

ifconfig查看是否配置成功

<img src="https://github.com/QSue/CC-BDA/raw/cdf70a0330bc8ed30bbd568a640ee40f78bc0bfe/CH2_1.png" style="zoom: 200%;" />

4.安装HTconder

```
curl -fsSL https://get.htcondor.org | sudo /bin/bash -s -- --no-dry-run 
```



5.验证单机安装（如果以下两个命令都有效，则安装可能成功）

```
condor_status
```

 <img src="https://github.com/QSue/CC-BDA/raw/cdf70a0330bc8ed30bbd568a640ee40f78bc0bfe/CH2_2.png" alt="image-20211128171702795" style="zoom:200%;" />

```
condor_q
```

 <img src="https://github.com/QSue/CC-BDA/raw/cdf70a0330bc8ed30bbd568a640ee40f78bc0bfe/CH2_3.png" alt="image-20211128171711425" style="zoom:200%;" />

心得：总体来说还是很顺利地安装，碰到的问题还是算小场面，提示出来的错误信息搜索引擎搜索还是能得到自己想要的信息。

 

##  JumpServer

根据安装文档（https://docs.jumpserver.org/zh/master/install/setup_by_fast/）

采用了最方便的安装方式一键部署

根据文档说明在虚拟机安装Linux系统，配置网络，并且进去根目录，安装cur一切准备完毕后，敲以下命令：

<img src="https://github.com/QSue/CC-BDA/raw/cdf70a0330bc8ed30bbd568a640ee40f78bc0bfe/CH2_4.png" alt="image-20211128171859002" style="zoom:150%;" />

则jumpserver安装成功

心得：由于安装HTconder后再来安装JumpServer以后，基本无阻地安装好，而且所占内存比较小，比较快的安装完。

