## 安装Ceph

**<u>在做安装之前，同学说要使用Ubuntu18，所以我重新下载了Ubuntu18，创建了一台虚拟机，操作步骤和结果如下</u>**

1.对于Ubuntu系统**需要添加安装源**，如果使用系统默认的安装源ceph-deploy版本太低，后续操作会有问题。

```text
wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add -
echo deb https://download.ceph.com/debian-{ceph-stable-release}/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list
```

上述命令中的`ceph-stable-release`是希望安装Ceph的版本，在具体安装时需要替换掉，比如希望安装L版本则需要替换成字符串`luminous`， 如果希望安装M版时，需要替换成字符串`mimic`。下面是一个具体的例子。**<u>我安装的就是L版本</u>**

```text
echo deb https://download.ceph.com/debian-luminous/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list
```

添加完安装源后，更新apt缓存，然后就可以安装ceph-deploy了，具体命令如下：

```text
sudo apt update
sudo apt -y install ceph-deploy
```

**创建新集群** 安装完ceph-deploy之后就可以通过它安装ceph软件了（后续安装与发行版没有关系了），当然首先要创建一个ceph集群。创建ceph集群也是通过ceph-deploy命令

#### <u>在这里要在root用户下执行！！！</u>

```text
mkdir myceph
cd myceph
ceph-deploy new zhangsn
```

因为我们是单节点ceph集群，因此需要将集群的副本数量设置为1，这样方便一些。具体方法是把如下内容加入到 ceph.conf 里面。

```text
[global]
osd pool default size = 1
osd pool default min size = 1
```

**安装 ceph 软件** 修改完配置文件之后，我们就可以安装Ceph软件包了。以安装L版本的软件为例，执行如下命令即可。

```text
ceph-deploy install --release luminous zhangsn
```

**初始化 mon** Ceph的整个集群的状态和配置信息等都是通过一个名为Monitor的集群管理的，因此首先需要启动Monitor服务。可以执行如下命令完成：

```text
ceph-deploy mon create-initial
ceph-deploy admin zhangsn
```

启动Monitor节点后Ceph集群就可以访问了，通过`ceph -s`命令可以查看集群的状态。**<u>这是我的运行成功截图</u>**

![](https://raw.githubusercontent.com/yrj903474887/YRJpicture/main/images/5.1.PNG)

