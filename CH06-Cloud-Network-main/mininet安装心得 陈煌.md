# mininet安装心得

1. 打开VMware 启动虚拟机
2. 安装mininet

获取源代码

```
sudo apt-get update
sudo apt-get install git
sudo git clone git://github.com/mininet/mininet
```

下载源代码后，执行安装指令

```
sudo ./mininet/util/install.sh -a
```

<img src="C:\Users\chinese\AppData\Roaming\Typora\typora-user-images\image-20211123174441121.png" alt="image-20211123174441121" style="zoom:50%;" />

测试基本功能

```
sudo apt install net-tools
```

```
sudo mn --test pingall
```

3. 可能遇到的问题

不安装网络工具是会提示找不到所需要的可执行ifconfig

当运行ifconfig命令时，提示以下错误

```
hadoop@ubuntu-20-04：~$ ifconfig

Command 'ifconfig'not found, but can be installed with:

sudo apt install net-tools
```

或

```
-bash: ifconfig: command not found
```

按照操作安装网络工具即可解决