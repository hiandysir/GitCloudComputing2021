# CH05-Distributed-storage

## 课内复习
1.	分布式存储的定义是什么？
2.	分布式存储有哪几种类型？
3.	SAN和NAS的区别是什么？
4.	是比较不同文件系统的特点。

## 课外思考
1.	是否存在一种文件系统能够应对所有类型的文件存储？为什么？
2.	Paxos的原理和机制是什么？

## 參考文獻
第5章 分布式存储

23.	S Ghemawat, H Gobioff, ST Leung, The Google File System,  ACM symposium on Operating systems principles, 2003, 37(5): 29-43.
24.	SA Weil, SA Brandt, EL Miller,et al., Ceph: A Scalable, High-Performance Distributed File System, Symposium on Operating systems design and implementation ,2006, 307-320.
25.	G Decandia , D Hastorun , M Jampani , et al., Dynamo: Amazon's Highly Available Key-value Store, ACM SIGOPS Symposium on Operating Systems Principles, 2007 , 41 (6): 205-220.
26.	B Calder, J Wang, A Ogus, et al., Windows Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency, ACM Symposium on Operating Systems Principles, 2011: 143-157.
27.	J Shafer , S Rixner , AL Cox, The Hadoop Distributed Filesystem: Balancing Portability and Performance, Performance Analysis of Systems & Software (ISPASS), 2010, pp122-133.
28.	Y Mansouri, A Toosi, R Buyya, Data Storage Management in Cloud Environments: Taxonomy, Survey, and Future Directions, ACM Computing Surveys, 2017, 50(6): 1-51.
29.	R Liu , R Liu, R Liu, et al., Slacker: Fast Distribution with Lazy Docker Containers, Usenix Conference on File and Storage Technologies, 2016, pp181-195.


# Distributed storage

1.本地文件系統
2.網絡文件系統
3.分佈式文件系統
4.塊存儲
5.對象存儲


1.本地文件系统
     ntfs(wind)、ext2、ext3、ext4、xfs
     
     ext2不带日志，3和4带有日志：文件系统的日志作用（防止机器突然断电）：所有的数据在给磁盘存数据之前会先给文件系统的日志里面存一份，防止机器突然断电之后数据没有存完，这样它还可以从日志里面重新将数据拷贝到磁盘。
             
 2.网络文件系统----做远程共享    
     
     非分布式
         nfs           网络文件系统--称之为nas存储（网络附加存储）
     分布式
         hdfs          分布式网络文件系统
         glusterfs     分布式网络文件系统，不需要管理服务器
         ceph         分布式网络文件系统，块存储，对象存储
         
         #分布式文件系统特点
             1.共享的是文件系统。共享的最小单位是文件
             2.可扩展性强、安全。实现PB级别的存储
 
 2.1 分布式文件系统存储使用架构
 
                              client
                                 |
                             namenode  元数据服务器-管理服务器，存储这个文件的数据存放的位置信息    
                                 |
                 ------------------------------------
                 |               |                  |
               datanode        datanode            datanode  #存储数据，数据节点
  

補充知識
元數據服務器 元數據
https://www.bilibili.com/video/BV1Vi4y1M7te

2.2 对象存储使用架构

 文件---数字（innode号）innode信息（文件属性：）
 文件组成：
         文件属性   innode（存放文件的属性:文件名称、大小、权限、存放数据块....）
         纯数据
 
                              client
                                 |
                             namenode  元数据服务器-管理服务器，存储这个文件的属性信息    
                                 |
                 ------------------------------------
                 |               |                  |
               datanode        datanode            datanode  #存储数据的数据节点
         
注意：
         1.分布式存储不一定是对象存储，所有的对象存储一定是分布式存储
         2.分布式文件系统的元数据服务器存储的各个数据的位置信息
         3.对象存储服务的的元数据服务器存储的是数据的属性信息

     
2.3 非分布式文件系统

 典型设备： FTP、NFS服务器
 为了克服块存储文件无法共享的问题，所以有了文件存储。在服务器上安装FTP与NFS服务，就是文件存储。
 优点：
             造价低，随便一台机器就可以了。
             方便文件共享。
 缺点：
             读写速率低。
             传输速率慢。
 使用场景：
             日志存储。
             有目录结构的文件存储。
     
 
 3.分布式文件系统的特性
 
         可扩展
         分布式存储系统可以扩展到几百台甚至几千台的集群规模，而且随着集群规模的增长，系统整体性能表现为线性增长。分布式存储的水平扩展有以下几个特性：
           1) 节点扩展后，旧数据会自动迁移到新节点，实现负载均衡，避免单点故障的情况出现;
           2) 水平扩展只需要将新节点和原有集群连接到同一网络，整个过程不会对业务造成影响;
 ​
         低成本
         分布式存储系统的自动容错、自动负载均衡机制使其可以构建在普通的PC机之上。
 ​
         易管理
         可通过一个简单的WEB界面就可以对整个系统进行配置管理，运维简便，极低的管理成本。
 
 4.块存储
 
     块存储的特点：
     1.主要是将裸磁盘空间映射给主机使用的，共享的最小单位是块
     2.使用的交换机是光纤交换机价格贵成本高
     3.性能最好，扩展性好
     4.不能做文件系统的共享

         最典型的就是SAN（storage area network）(存储区域网)----有一个局域网里面有一个交换机，交换机上面连着服务器，所有服务器都是专业存储的设备，他们组成一个存储区域网，当我们用的时候只需要在这个区域网里面拿空间使用
         
 
    典型设备： 磁盘阵列，硬盘
    优点：
         通过Raid与LVM等手段，对数据提供了保护。
         多块廉价的硬盘组合起来，提高容量。
         多块磁盘组合出来的逻辑盘，提升读写效率。
    缺点：
         采用SAN架构组网时，光纤交换机，造价成本高。
         主机之间无法共享数据。
    使用场景：
         虚拟机磁盘存储分配。
         日志存储。
         文件存储。
 
 5.对象存储
                    
      为什么需要对象存储？
      首先，一个文件包含了属性（术语叫metadata，元数据，例如该文件的大小、修改时间、存储路径等）以及内容（以下简称数据）。
 
     而对象存储则将元数据独立了出来，控制节点叫元数据服务器（服务器+对象存储管理软件），里面主要负责存储对象的属性（主要是对象的数据被打散存放到了那几台分布式服务器中的信息），而其他负责存储数据的分布式服务器叫做OSD，主要负责存储文件的数据部分。当用户访问对象，会先访问元数据服务器，元数据服务器只负责反馈对象存储在哪些OSD，假设反馈文件A存储在B、C、D三台OSD，那么用户就会再次直接访问3台OSD服务器去读取数据。
 
     由于是3台OSD同时对外传输数据，所以传输的速度就加快了。当OSD服务器数量越多，这种读写速度的提升就越大，通过此种方式，实现了读写快的目的。
 
     另一方面，对象存储软件是有专门的文件系统的，所以OSD对外又相当于文件服务器，那么就不存在文件共享方面的困难了，也解决了文件共享方面的问题。
 
     所以对象存储的出现，很好地结合了块存储与文件存储的优点。
 
    优点：
         具备块存储的读写高速。
         具备文件存储的共享等特性。
 
    使用场景： (适合更新变动较少的数据)
         图片存储。
         视频存储。                 

6.常见分布式存储---了解

      Hadoop HDFS
      HDFS（Hadoop Distributed File System）是一个分布式文件系统,是hadoop生态系统的一个重要组成部分，是hadoop中的的存储组件.HDFS是一个高度容错性的系统，HDFS能提供高吞吐量的数据访问，非常适合大规模数据集上的应用。

      HDFS的优点：
         1.  高容错性 
              数据自动保存多个副本
              副本丢失后,自动恢复
        2.  良好的数据访问机制 
              一次写入、多次读取,保证数据一致性
         3.  适合大数据文件的存储
              TB、 甚至PB级数据 
              扩展能力很强
 
      HDFS的缺点：
         1.  海量小文件存取
               占用NameNode大量内存
         2.  一个文件只能有一个写入者 
              仅支持append(追加)


參考 

單機存儲系統 https://blog.csdn.net/ChenVast/article/details/72866755

閱讀：
Ceph分布式存储工作原理 及 部署介绍


練習：
1）单机版Ceph环境部署，Linux平台 https://zhuanlan.zhihu.com/p/67832892

2）用Docker搭建Ceph集群(nautilus版本) https://feige.blog.csdn.net/article/details/109159160?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-7.control&dist_request_id=&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-7.control

3）Blob應用小練習


