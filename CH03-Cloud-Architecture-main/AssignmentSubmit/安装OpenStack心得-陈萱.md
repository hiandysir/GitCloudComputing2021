### 安装OpenStack心得

\1. 在virtuaBox上安装Ubuntu镜像；

\2. 运行 sudo apt update

sudo snap install microstack --beta --devmode

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsCA97.tmp.jpg) 

\3. 安装完成后，配置资源

运行sudo microstack init --auto --control 完成初始化：

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsCA98.tmp.jpg) 

\4. 运行sudo snap get microstack config.credentials.keystone-password 获取管理界面的密码：

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsCA99.tmp.jpg) 

 

\5. 在浏览器输入虚拟机IP地址进入OpenStack的网站：

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsCA9A.tmp.jpg) 

\6. 输入用户名和密码后，管理界面为：

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsCA9B.tmp.jpg)![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsCA9C.tmp.jpg)![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsCAAD.tmp.jpg) 

\7. 运行microstack launch cirros --name test命令，管理界面会变：

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsCAAE.tmp.jpg) 

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsCAAF.tmp.jpg) 

此实验按照安装教程一步步走，没有出现报错或警告等信息。

 

 

 

 

 

 

 