# OpenStack安装心得

## 1 安装步骤

​	在创建虚拟机时要注意的点有：内存分配不小于8G，硬盘不小于50G，最小化安装。注：一定要按这个创建虚拟机，否则就会安装失败！

​	因为创建虚拟机时速度很慢，所以以后可以先创建一个空白的2G内存，硬盘50G的虚拟机来克隆，后期再做会提高速度。

### 	1.运行以下命令

```
sudo apt update
sudo snap install microstack --beta --devmode
```

​	运行成功后显示：microstack (beta) ussuri from Canonical✓ installed

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/image-20211121153457167.png)

​	安装完成，开始配置：

### 	2.初始化

```
sudo microstack init --auto --control
```

初始化完成后的截图：

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222139717.png)

### 	3.获取管理界面密码，进入Web GUI界面

```
sudo snap get microstack config.credentials.keystone-password
```

```
密钥为：g5nsbGA1B8GWzvfBYPSAwzzotvGgwocL
```

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/image-20211121155440938.png)

### 	4.打开浏览器，输入虚拟机IP地址即可进入

​	使用：$ ip a 命令行来查看自己的IP地址

​	注：查看IP地址后，到浏览器输入为：ip地址/auth/login/

​	用户名：admin 密码：刚刚获取的密码

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/image-20211121161333200.png)

### 	5.登陆成功后

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/image-20211121161420426.png)

