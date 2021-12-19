# Ceph 安装心得

## 添加安装源

`wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add -`

`echo deb https://download.ceph.com/debian-{ceph-stable-release}/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list`

添加完安装源后，更新apt缓存，然后就可以安装ceph-deploy了，具体命令如下：

`sudo apt update`

`sudo apt -y install ceph-deploy`

## 安装ceph软件包

创建配置文件和key文件

`mkdir myceph`

`cd myceph`

`ceph-deploy new zhangsn`

将集群的副本数量设置为1

```text
[global]
osd pool default size = 1
osd pool default min size = 1
```

## 安装 ceph 软件

```text
ceph-deploy install --release luminous zhangsn
```

## 初始化 mon

```text
ceph-deploy mon create-initial
ceph-deploy admin zhangsn
```

## 部署ceph mgr

```text
ceph-deploy  mgr create zhangsn
```

## 部署ceph osd

```text
$ pvcreate /dev/sdb
  Physical volume "/dev/sdb" successfully created.
$ vgcreate  ceph-pool /dev/sdb
  Volume group "ceph-pool" successfully created
$ lvcreate -n osd0.wal -L 1G ceph-pool
  Logical volume "osd0.wal" created.
$ lvcreate -n osd0.db -L 1G ceph-pool
  Logical volume "osd0.db" created.
$ lvcreate -n osd0 -l 100%FREE ceph-pool
  Logical volume "osd0" created.
```

## 创建 OSD 

```text
ceph-deploy osd create \
    --data ceph-pool/osd0 \
    --block-db ceph-pool/osd0.db \
    --block-wal ceph-pool/osd0.wal \
    --bluestore zhangsn
```