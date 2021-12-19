### 安装OpenStack心得

\1.在终端输入

#sudo apt update

#sudo snap install microstack --beta --devmode

\2. 安装完成后，配置资源

运行sudo microstack init --auto --control 完成初始化：

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsCA98.tmp.jpg) 

\3. 运行sudo snap get microstack config.credentials.keystone-password 获取管理界面的密码：

![image-20211217193103985](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211217193103985.png)

\4. 在浏览器输入虚拟机IP地址进入OpenStack的网站：

![image-20211217193123296](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211217193123296.png)

![image-20211217193139561](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211217193139561.png)