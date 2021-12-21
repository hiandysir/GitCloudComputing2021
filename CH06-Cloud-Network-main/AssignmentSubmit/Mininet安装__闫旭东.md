# Mininet安装



### 1.什么是Mininet

Mininet 在一台计算机上模拟由主机、链路和交换机组成的完整网络。要创建示例双主机、单交换机网络，只需运行：

```
sudo mn
```

Mininet对于交互式开发，测试和演示非常有用，特别是那些使用OpenFlow和SDN的开发，测试和演示。在 Mininet 中原型化的基于 OpenFlow 的网络控制器通常可以通过最小的更改传输到硬件，以实现全线速执行。



### 2.Mininet安装

准备一台Ubuntu虚拟机

###### ![1640005631(1)](https://user-images.githubusercontent.com/90243359/146771785-377b18b8-9b6d-43ba-983c-2e45163cafb6.png)

进入虚拟机，打开终端执行以下命令：

```
sudo apt install git
git clone git://github.com/mininet/mininet
cd mininet/util
./install.sh -a 
```

安装完成

###### ![1640005196(1)](https://user-images.githubusercontent.com/90243359/146771438-baa8e63e-1858-44e1-8bd4-3eba73e3e135.png)



### 3.测试Mininet

执行以下命令

```
sudo mn --test pingall
```

###### <img src="https://user-images.githubusercontent.com/90243359/146772578-e51bf0b8-284e-462e-9ac9-e98f0bd5c069.png" alt="1640006043(1)" style="zoom:80%;" />
