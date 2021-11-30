# 1. 安装前的准备工作

**<u>我是按照Github上的Mininet安装教程操作的，但是安装到一半的时候发生了报错，报错内容如下：</u>**

![](https://raw.githubusercontent.com/yrj903474887/YRJpicture/main/images/6.1.PNG)

**<u>所以我就自己找教程先在虚拟机上安装OpenVSwitch，在安装OpenVSwitch的时候又遇到这个报错：</u>**

![](https://raw.githubusercontent.com/yrj903474887/YRJpicture/main/images/6.2.PNG)

**<u>所以就又不得不安装Python，Ubuntu 20.04 和其他版本的 Debian Linux 预装了 Python 3，经过更新索引包、升级系统上安装的软件包、检查系统中安装的 Python 3 版本之后，我成功的安装了Python3.8.10，然后又重新安装了OpenVSwitch和Mininet。</u>**

# 2. 安装Mininet

**<u>经过之前的准备工作，Mininet就很好安装了</u>**

```
sudo apt install git
git clone git://github.com/mininet/mininet
cd mininet/util
./install.sh -a 
```



**<u>測試 Mininet</u>**

```
sudo mn --test pingall
```

****

**<u>最终也是成功安装了，如截图所示：</u>**![](https://raw.githubusercontent.com/yrj903474887/YRJpicture/main/images/6.3.PNG)

