# Mininet安装

## 安装步骤

### 1.查看Mininet代码，如果你还没有它：

```
git clone https://github.com/mininet/mininet
```

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111221951139.png)

### 出现错误

原因：没有mininet这个文件或文件夹

解决办法：换一种安装方式

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222005069.png)

## 2.若要确认运行的是哪个操作系统版本，请运行以下命令

```
lsb_release -a
```

```
Mininet 2.3.0 on Debian 11: sudo apt-get install mininet
Mininet 2.2.2 on Ubuntu 20.04 LTS: sudo apt-get install mininet
Mininet 2.2.2 on Ubuntu 18.04 LTS: sudo apt-get install mininet
```

如果拥有的Mininet版本不明显，可以尝试：

```
mn --version
```

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222031373.png)

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222032681.png)

## 3.Mininet 支持多个交换机和 OpenFlow 控制器。对于此测试，我们将在桥接式/独立模式下使用开放式 vSwitch。

要对此进行测试，请尝试：

```
sudo mn --switch ovsbr --test pingall
```

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222036674.png)

## 4.如果您希望完成 Mininet 演练，则需要安装其他软件。

```
git clone git://github.com/mininet/mininet
mininet/util/install.sh -fw
```

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222039662.png)

![](https://raw.githubusercontent.com/LUWEI2000/Typora-images/master/img/202111222046629.png)

