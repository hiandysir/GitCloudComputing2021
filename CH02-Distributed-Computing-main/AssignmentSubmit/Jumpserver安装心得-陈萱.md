### Jumpserver安装步骤：

 

#### 在virtualbox中部署Ubuntu系统：

\1. 下载Ubuntu20.04版本，在virtualbox的常规设置里添加Ubuntu20.04的镜像，磁盘大小尽量设置大一点；

\2. 设置完成，启动虚拟机，设置个人与虚拟机的用户名和密码，需等待一段时间；

\3. 进入终端后，获取root权限，输入sudo su-命令；

 

#### 安装jumpserver：

\1. 在终端输入命令curl -sSL https://github.com/jumpserver/jumpserver/releases/download/v2.15.4/quick_start.sh | bash

出现Command ‘curl’ not found， but be installed with：的提示

输入apt-get install curl命令即可。

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wps9A18.tmp.jpg) 

\2. 安装成功后，输入cd /opt/jumpserver-installer-v2.15.4命令进入文件目录；

\3. 输入./jmsctl.sh start启动jumpserver

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wps9A19.tmp.jpg)启动后会出现HOSTNAME值没有设置的提示，第一次安装时未出现这个提示，第二次安装后出现此提示，但是并不影响最终进入到jumpserver开源堡垒机。

\4. ![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wps9A1A.tmp.jpg)

 

 

 