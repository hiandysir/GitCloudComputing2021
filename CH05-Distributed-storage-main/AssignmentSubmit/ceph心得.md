   本次进行的实验为单机版Ceph的部署实验，参考的[教程](https://zhuanlan.zhihu.com/p/67832892)来自知乎，我个人认为这个教程是比较不完善的，写的过于简洁。
   首先配置好安装源
   ```
   wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add -
   echo deb https://download.ceph.com/debian-luminous/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list
   ```
   我在执行这步时忘记了将下载链接内的下载源修改为我要下载的版本，犯下了第一个错误。
   然后添加完安装源后更新apt缓存，安装ceph-deploy
   ```
   sudo apt update
   sudo apt -y install ceph-deploy
   ```
   下面的创建新集群开始便十分地有难度，短短的三行代码
   ```
   mkdir myceph
   cd myceph
   ceph-deploy new zhangsn
   ```
   复制下去之后我遭遇了localhost无法连接的错误，按网上教程我先在/etc/hosts中修改了配置，然后又关闭了防火墙，依然收到22号端口无法连接的报错。最后我发现主要的问题是在复制代码的时候我没有想
   ```
   zhangsn
   ```
   是什么东西，我简单地以为这是集群的名字，可以随意命名，但实际上你需要将它修改成你自己的host名，这一步浪费了我很多时间。
   ```
    mkdir myceph
   cd myceph
   ceph-deploy new [your host name]
   ```
   更改完之后继续报错
   ```
   AttributeError: module ‘platform‘ has no attribute ‘linux_distribution‘
   ```
   引起这个错误的原因是ubuntu中默认安装的python3.8已经不存在platform.linux_distribution()方法，而在本次任务中我们需要这个方法，于是我需要安装旧版本的python。
   我选择在ubuntu上安装pyenv，这是一个方便你安装，管理多个版本python的工具，[此处](https://www.jianshu.com/p/8aaf2525fa80)提供一个教学链接，具体安装之后我一度遇到无法切换python版本的问题，多次试图切换python但始终运行的是ubuntu已经预装好的python3.8
   我将~/.bash_profile处和~/.zshrc处的环境变量都修改并进行多次尝试后依旧无法切换python版本，最后我只能尝试更简单的[方法](https://blog.csdn.net/qq_27657429/article/details/53482595)
   ```
    sudo add-apt-repository ppa:fkrull/deadsnakes
    sudo apt-get update
    sudo apt-get install python3.5
   ```
   然后进行了以下的操作
   ```
    sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.5 200
    # 单独使用这个命令让python默认指向我按照的3.5
    取消了原来的旧python
    sudo mv /usr/bin/python3 /usr/bin/python3-old
    sudo ln -s /usr/bin/python3.5 /usr/bin/python3
    #取消了原来的旧python
    wget https://bootstrap.pypa.io/get-pip.py
    sudo python3 get-pip.py
    sudo pip3 install setuptools --upgrade
    #安装了新pip
    sudo rm /usr/bin/python3
    sudo mv /usr/bin/python3-old /usr/bin/python3
    #切换回来链接文件
   ```
   终于完成了python的切换。然后终于完成了ceph集群的配置
   完成集群的配置后修改ceph.conf，由于是单机配置单独加入以下内容，问题不大
   ```
   [global]
   osd pool default size = 1
   osd pool default min size = 1
   ```
   然后正式安装ceph软件
   ```
   ceph-deploy install --release luminous [your host name]
   ```
   但是此时又报错了
   ```
   [ceph_deploy][ERROR ] RuntimeError: Failed to execute command: env DEBIAN_FRONTEND=noninteractive DEBIAN_PRIORITY=critical apt-get --assume-yes -q update
   ```
   我在网上搜索，少数记载了这个错误的[网页](https://blog.csdn.net/don_chiang709/article/details/91511828)给出的解决方案是在在admin-node上修改/etc/apt/sources.list.d/ceph.list内容如下：
   ```
   deb http://download.ceph.com/debian-{ceph-stable-release}/ bionic main =>deb http://download.ceph.com/debian-luminous/ bionic main
   ```
   由于我是单机安装我觉得admin-node应该是我的主机，我在本地进行了相应的修改，也把{ceph-stable-release}修改为了luminous，但已经无法执行，怀疑是宿舍网络环境导致的错误，如果有更好的网络环境应该能完成实验。