ubuntu  部署ceph：
wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add -
echo deb https://download.ceph.com/debian-{ceph-stable-release}/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.lis



添加完安装源后，更新apt缓存，然后就可以安装ceph-deploy了

sudo apt update
sudo apt -y install ceph-deploy



安装ceph软件包

mkdir myceph
cd myceph
ceph-deploy new zhangsn



[global]
osd pool default size = 1
osd pool default min size = 1



**安装 ceph 软件** 修改完配置文件之后，我们就可以安装Ceph软件包了。以安装L版本的软件为例，执行如下命令即可。



ceph-deploy install --release luminous zhangsn



**初始化 mon** Ceph的整个集群的状态和配置信息等都是通过一个名为Monitor的集群管理的，因此首先需要启动Monitor服务。可以执行如下命令完成：

ceph-deploy mon create-initial
ceph-deploy admin zhangsn



**部署ceph mgr** ceph mgr

ceph-deploy  mgr create zhangsn



**部署ceph osd** 



pvcreate /dev/sdb
  Physical volume "/dev/sdb" successfully created.
vgcreate  ceph-pool /dev/sdb
  Volume group "ceph-pool" successfully created
lvcreate -n osd0.wal -L 1G ceph-pool
  Logical volume "osd0.wal" created.
lvcreate -n osd0.db -L 1G ceph-pool
  Logical volume "osd0.db" created.
lvcreate -n osd0 -l 100%FREE ceph-pool
  Logical volume "osd0" created.



ceph-deploy osd create \
    --data ceph-pool/osd0 \
    --block-db ceph-pool/osd0.db \
    --block-wal ceph-pool/osd0.wal \
    --bluestore zhangsn



创建一个运行在整个裸盘上的OSD，也是通过ceph-deploy进行创建：



ceph-deploy osd create --bluestore zhangsn --data /dev/sdc