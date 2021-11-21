本次进行的实验为mininet的安装，根据老师在GitHub上给出的教程，第一种方法为直接下载Mininet-vm的虚拟机来进行安装。安装完成直接打开便可。在这里下载的Mininet-vm虚拟机是没有图形界面的，如果你想要图形界面可以自己手动安装
   ```
apt-get update
apt-get install vnc4server
apt-get install xfce4
apt-get install ubuntu-desktop
sudo apt-get install xrdp
   ```
这里图形界面安装耗时较长，所以建议直接使用命令行界面开始实验。开始试验后，我先转到环境设置来设置环境。网站需要自己去寻找，路径如下：
```
https://github.com/mininet/mininet/wiki/Environment-Setup
```
在进行环境配置时进行到安装 ltprotocol这一步会报错，显示
```
Could not find suitable distribution for Requirement.parse('twisted')
```
在网上查询了一下这个错误，发现可能是pypi被防火墙屏蔽了，首先设置https.proxy代理试图绕过防火墙
```
git config --global https.proxy 'socks5://127.0.0.1:1080'
```
尝试后无果，更换pypi下载源
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple [下载包名字]
```
下载到一半报错
```
no module named pathlib
```
上网查看发现可能是python版本的问题引起，查看python版本
```
python --version
```
指向的是python，我们切换为pyhon3看看
```
echo alias python=python3 >> ~/.bashrc

source ~/.bashrc
```
再次查看python版本，发现已切换为python3，但依然无法安装。后来我发现是要把pip改为pip3
```
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple twisted
```
但是操作完之后依旧无法安装，依然报错pip版本过低，尝试升级pip
```
python -m pip install --upgrade pip -i https://pypi.douban.com/simple
```
可以进行成功的安装，但之后再使用pip指令依旧会提示pip版本过低。所以我决定尝试使用source code安裝Mininet。也许是官方给出的Mininet-vm包体过于老旧导致错误不断。
在文档里安装教程使用了倒叙的手法，我在按提示安装下来后遇到报错才知道应该先安装OVS，但OVS应该先安装anaconda，但安装后OVS依旧安装失败，原因应该是服务器的问题。



