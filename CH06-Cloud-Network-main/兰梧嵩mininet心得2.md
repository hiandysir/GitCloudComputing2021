在上次的失误后，我上网寻找教材，在 https://www.cnblogs.com/JCpeng/p/14993506.html处寻找到了合适的教程，前面配备vmware的环节就跳过，首先将源码克隆到本地

```
sudo apt-get update
sudo apt-get install git
sudo git clone git://github.com/mininet/mininet
```

克隆完后安装mininet

```
sudo ./mininet/util/install.sh -a
```

安装完成后测试一下性能

```
sudo apt install net-tools
sudo mn --test pingall
```

接下来搭建网络环境，由于之前安装官方文档不太可行我决定尝试这里的教程，首先安装ryu

```
sudo apt install python3-pip
pip install ryu
```

然后去克隆Open vSwitch的源代码

```
sudo git clone https://github.com/openvswitch/ovs
```

启动ryu

```
sudo apt install python3-ryu
ryu-manager --verbose ryu.app.simple_switch_13
```

启动mininet

```
sudo mn --topo linear,2 --mac --switch ovsk --controller remote
```

在启动后可以用指令pingall测试一下网络是否畅通，相关的图片我也一并上传到文件夹内了。