# Mininet安装心得

## Mininet在Ubuntu下的安装过程

### 下载源码的方法，从github上下载安装

### step1:打开Ubuntu终端，首先安装git命令

	 输入代码：apt-get install git

###  step2:等待即可完成git命令的安装，然后利用git下载mininet源代码
	输入代码：git clone http://github.com/mininet/mininet.git

### step3:等待即可完成mininet源代码的获取，这个源代码位于Ubuntu的home/用户名/mininet文件夹中，安装过程中要用到的文件是mininet/util/install.sh文件，所以，我们用这个脚本文件安装mininet，输入代码:
	mininet/util/install.sh -n3V 2.5.0
### step4:等待安装完成之后，输入下面代码，检测是否安装完成：(如果进入了root用户模式，则不用写代码前面的root)
	sudo mn --test pingall
### 安装成功
