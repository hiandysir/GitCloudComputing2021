---
CloudComputing2021
ch5 study notes
Author：zoxiii
---

[TOC]


# 1、单机版Ceph环境部署
[参考文档](https://zhuanlan.zhihu.com/p/67832892) 

## （1）安装ceph-deploy

```c
# yum install https://download.ceph.com/rpm-luminous/el7/noarch/ceph-deploy-2.0.0-0.noarch.rpm
```
## （2）创建新集群 

```c
# mkdir myceph
# cd myceph
# ceph-deploy new zoxiii
```
## （3）将集群的副本数量设置为1
```c
# vi ceph.conf
[global]
osd pool default size = 1
osd pool default min size = 1
```
## （4）安装Ceph

```c
ceph-deploy install --release luminous zoxiii
```
## （5）启动Monitor服务

```c
ceph-deploy mon create-initial
ceph-deploy admin zhangsn
```
## （6）部署ceph mgr

```c
ceph-deploy  mgr create zhangsn
```
## （7）部署ceph osd
为了便于我们后续研究BlueStore的架构及原理，我们创建2种类型的OSD，一个是具有3个逻辑卷的OSD（模拟不同类型的存储介质），另外一个是只有一个逻辑卷的OSD。 首先创建具有3个逻辑卷的OSD，分别是db、wal和data。分别执行如下命令创建卷组和逻辑卷。
```c
# pvcreate /dev/sdb
  Physical volume "/dev/sdb" successfully created.
# vgcreate  ceph-pool /dev/sdb
  Volume group "ceph-pool" successfully created
# lvcreate -n osd0.wal -L 1G ceph-pool
  Logical volume "osd0.wal" created.
# lvcreate -n osd0.db -L 1G ceph-pool
  Logical volume "osd0.db" created.
# lvcreate -n osd0 -l 100%FREE ceph-pool
  Logical volume "osd0" created.
```
创建 OSD

```c
ceph-deploy osd create \
    --data ceph-pool/osd0 \
    --block-db ceph-pool/osd0.db \
    --block-wal ceph-pool/osd0.wal \
    --bluestore zhangsn
```
## （8）创建一个运行在整个裸盘上的OSD

```c
ceph-deploy osd create --bluestore zhangsn --data /dev/sdc
```

# 2、HDFS

在Hadoop这门课已经配置过伪分布式

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201025121721277.png)

# 3、Ceph和HDFS进行比较

## Ceph

Ceph是一个分布式存储系统，诞生于2004年，最早致力于开发下一代高性能分布式文件系统的项目。经过多年的发展之后，已得到众多云计算和存储厂商的支持，成为应用最广泛的开源分布式存储平台。

Ceph存在一些缺点

1. 去中心化的分布式解决方案，需要提前做好规划设计，对技术团队的要求能力比较高。
2. Ceph扩容时，由于其数据分布均衡的特性，会导致整个存储系统性能的下降。

Ceph相比于其他存储方案的优势

1. **高可用**：Ceph中的数据副本数量可以由管理员自行定义，并可以通过CRUSH算法指定副本的物理存储位置以分隔故障域，支持数据强一致性； Ceph可以忍受多种故障场景并自动尝试并行修复；Ceph支持多份强一致性副本，副本能够垮主机、机架、机房、数据中心存放。所以安全可靠。Ceph存储节点可以自管理、自动修复。无单点故障，容错性强。
2. **高性能**：因为是多个副本，因此在读写操作时候能够做到高度并行化。理论上，节点越多，整个集群的IOPS和吞吐量越高。另外一点Ceph客户端读写数据直接与存储设备(osd) 交互。在块存储和对象存储中无需元数据服务器。
3. **高扩展性**：Ceph不同于Swift，客户端所有的读写操作都要经过代理节点。一旦集群并发量增大时，代理节点很容易成为单点瓶颈。Ceph本身并没有主控节点，扩展起来比较容易，并且理论上，它的性能会随着磁盘数量的增加而线性增长。Ceph扩容方便、容量大。能够管理上千台服务器、EB级的容量。
4. 特性丰富：Ceph支持三种调用接口：对象存储，块存储，文件系统挂载。三种方式可以一同使用。在国内一些公司的云环境中，通常会采用Ceph作为openstack的唯一后端存储来提升数据转发效率。Ceph是统一存储，虽然它底层是一个分布式文件系统，但由于在上层开发了支持对象和块的接口，所以在开源存储软件中，优势很明显。



##  HDFS

  HDFS（Hadoop Distributed File System）是一个分布式文件系统,是hadoop生态系统的一个重要组成部分，是hadoop中的的存储组件.HDFS是一个高度容错性的系统，HDFS能提供高吞吐量的数据访问，非常适合大规模数据集上的应用。

  HDFS的优点：
    1. 高容错性 ：数据自动保存多个副本、副本丢失后,自动恢复
    2. 良好的数据访问机制 ：一次写入、多次读取,保证数据一致性
    3. 适合大数据文件的存储：TB、 甚至PB级数据 
    4. 扩展能力很强

  HDFS的缺点：

1. 海量小文件存取、占用NameNode大量内存
2. 一个文件只能有一个写入者  仅支持append(追加)
