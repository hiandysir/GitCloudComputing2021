### jumpserver安装心得

\1. 在终端输入命令

#curl -sSL https://github.com/jumpserver/jumpserver/releases/download/v2.15.4/quick_start.sh | bash

出现Command ‘curl’ not found， but be installed with：的提示

输入apt-get install curl命令即可。

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wps9A18.tmp.jpg) 

\2. 安装成功后，输入cd /opt/jumpserver-installer-v2.15.4命令进入文件目录；

\3. 输入./jmsctl.sh start启动jumpserver

![image-20211217191845868](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211217191845868.png)

\4. 

![image-20211217191633510](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211217191633510.png)