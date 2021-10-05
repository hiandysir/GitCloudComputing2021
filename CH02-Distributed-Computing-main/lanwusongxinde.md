在正式上课前我在宿舍中对这两款软件提前进行了安装，HTcondor我选择的是Windows版本来进行安装，在官网

https://research.cs.wisc.edu/htcondor/tarball/9.0/
处可以下载用于安装的msi包，但出于不明原因始终安装失败。根据报错代码原因与用户权限有关。然后我在ubuntu上进行JumpServer的安装。只要将ubuntu联网然后使用以下指令


```
curl -sSL https://github.com/jumpserver/jumpserver/releases/download/v2.14.1/quick_start.sh | bash 
cd /opt/jumpserver-installer-v2.14.1
```

就可以完成JumpServer的一键部署，耗时大约十、二十分钟。

在实际来到课堂上时虽然我携带了我的笔记本，但由于我的笔记本属于游戏本的类型功耗较高且机房内没有插座可以提供电源所以我只得采用机房内提供的电脑进行实践。首先在虚拟机的安装上我就遇到了不小的麻烦，我试图在学校的电脑上安装我常用的**VMware**但始终安装失败，最后决定使用机房内自带的**VirtualBox**进行实践。成功安装虚拟机后我试图进行JumpServer的一键部署，但由于网络的原因多次下载失败。

这次实践让我接触了JumpServer与HTcondor这两种工具，并进行了相关的实践操作，更多的知识还有待我课下进行进一步的学习。希望能在未来的学习过程中接触更多有用的知识。