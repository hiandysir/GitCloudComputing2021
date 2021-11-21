## Mininet配置与实验

环境：Windows10

### Mininet虚拟机安装

#### 相关下载

虚拟机软件下载：https://www.vmware.com/products/workstation-player.html

Mininet虚拟机下载：https://github.com/mininet/mininet/releases/download/2.3.0/mininet-2.3.0-210211-ubuntu-20.04.1-legacy-server-amd64-ovf.zip

#### 虚拟机导入与配置

选择“打开虚拟机”

选择mininet-2.3.0-210211-ubuntu-20.04.1-legacy-server-amd64.ovf

更新并安装GUI界面lxde，该步骤时间较长

更新：sudo apt-get update

安装lxde：sudo apt-get install xinit x11-xserver-utils lxde

### Mininet实验

Mininet 2.2.0之后的版本内置了一个构建网络拓扑的可视化工具miniedit.py，利用该工具可以自定义网络拓扑结构

路径：cd ~/mininet/examples

启动命令：sudo ./miniedit.py（不加sudo可能出现错误：“Mininet must run as root”）

构建拓扑结构，如下图

![Create](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/miniEditmd6.png)

在Edit-Preferences中设置运行后启动CLI，点击run运行，执行命令pingall

![Ping all](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/pingAllmd6.png)

点击stop停止运行，点击File，选择将脚本导出，方便下次使用

![Output](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/saveNetmd6.png)

