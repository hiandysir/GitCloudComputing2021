Mininet

从源本地安装：（获取源代码）

git clone git://github.com/mininet/mininet



有源代码树后：

mininet/util/install.sh [options]



install.sh -a



安装完成后，测试基本的 Mininet 功能：

sudo mn --switch ovsbr --test pingall