# 第二章jumpsever与htcondor安装心得



## Linux之手动安装jumpserver

### 安装步骤

#### 安装mysql，安装后创建jumpserver库及用户

#### 百度安装redis,安装方式比较简单

#### 下载jumpserver安装程序，解压安装包，执行安装脚本

#### 在安装语言的选择默认中文，配置网络选择默认no

#### 配置mysql与redis

#### 配置docker并启动

#### 安装成功后启动jumpserver，访问堡垒机URL

##### 若启动失败，可能是端口已经被占用，或者数据库账号密码不对，需要检查

## 在Windows操作系统中htcondor安装

### 步骤

 \1.  下载 condor-8.5.0-344547-Windows-x86.msi安装包

 \2.  运行condor-8.5.0-344547-Windows-x86.msi安装包。出现安装包启始界面

#### 如果是创建一个新HTCondor机群，需设置机群名称,计算机重启后，在console 控制台 （cmd）中运行 "condor_store_cred add"命令,输入管理员密码。

#### 在console 控制台 （cmd）中运行 "sc ceate condor binpath= c:\condor\bin\condor_master.exe"命令。

#### 在console 控制台 （cmd）中运行 "net start condor"命令。







