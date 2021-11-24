## M1基于Docker的完全分布式Hadoop搭建

硬件：M1芯片Macbook，arm架构

### 基本环境配置

拉取镜像(ubuntu:16.04)：docker pull ubuntu:16.04



初次启动容器：docker run -it ubuntu:16.04



更新系统：apt-get updata



安装jdk8：apt-get install java-1.8.0-openjdk



安装scala：apt-get install scala



安装vim：apt-get install vim



安装net-tools：apt-get install



安装ssh服务端与客户端：

apt-get install openssh-server

apt-get install openssh-client



设置免密登陆：

cd ~

ssh-keygen -t rsa -P ""

cd .ssh

cat id_rsa.pub >> authorized_keys



设置开机启动ssh，追加"service ssh start"

vim ~/.bashrc



### Hadoop配置

下载并解压hadoop：

wget http://archive.apache.org/dist/hadoop/core/hadoop-3.2.1/hadoop-3.2.1.tar.gz

tar -zxvf hadoop-3.2.1.tar.gz -C /usr/local

cd /usr/local

mv hadoop-3.2.1 hadoop



配置环境变量

export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-arm64

export JRE_HOME=${JAVA_HOME}/jre   

export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib   

export PATH=${JAVA_HOME}/bin:$PATH

export HADOOP_HOME=/usr/local/hadoop

export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

export HADOOP_COMMON_HOME=$HADOOP_HOME 

export HADOOP_HDFS_HOME=$HADOOP_HOME 

export HADOOP_MAPRED_HOME=$HADOOP_HOME

export HADOOP_YARN_HOME=$HADOOP_HOME 

export HADOOP_INSTALL=$HADOOP_HOME 

export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native 

export HADOOP_LIBEXEC_DIR=$HADOOP_HOME/libexec 

export JAVA_LIBRARY_PATH=$HADOOP_HOME/lib/native:$JAVA_LIBRARY_PATH

export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop

export HDFS_DATANODE_USER=root

export HDFS_DATANODE_SECURE_USER=root

export HDFS_SECONDARYNAMENODE_USER=root

export HDFS_NAMENODE_USER=root

export YARN_RESOURCEMANAGER_USER=root

export YARN_NODEMANAGER_USER=root



创建默认文件路径

mkdir /root/hadoop/tmp(自行更正)



进入/usr/local/hadoop/etc/hadoop，hadoop-env.sh末尾追加

export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-arm64

export HDFS_NAMENODE_USER=root

export HDFS_DATANODE_USER=root

export HDFS_SECONDARYNAMENODE_USER=root

export YARN_RESOURCEMANAGER_USER=root

export YARN_NODEMANAGER_USER=root



core-site.xml添加内容

~~~xml
<configuration>

  <property>

​    <name>fs.default.name</name>

​    <value>hdfs://master:9000</value>

  </property>

  <property>

​    <name>hadoop.tmp.dir</name>

​    <value>/root/hadoop/tmp</value>

  </property>

</configuration>
~~~



hdfs-site.xml添加内容

~~~xml
<configuration>

  <property>

​    <name>dfs.replication</name>

​    <value>2</value>

  </property>

  <property>

​    <name>dfs.namenode.name.dir</name>

​    <value>/root/hadoop/hdfs/name</value>

  </property>

  <property>

​    <name>dfs.namenode.data.dir</name>

​    <value>/root/hadoop/hdfs/data</value>

  </property>

</configuration>
~~~



mapred-site.xml添加内容

~~~xml
<configuration>

  <property>

​    <name>mapreduce.framework.name</name>

​    <value>yarn</value>

  </property>

  <property>

​    <name>mapreduce.application.classpath</name>

​    <value>

​      /usr/local/hadoop/etc/hadoop,

​      /usr/local/hadoop/share/hadoop/common/*,

​      /usr/local/hadoop/share/hadoop/common/lib/*,

​      /usr/local/hadoop/share/hadoop/hdfs/*,

​      /usr/local/hadoop/share/hadoop/hdfs/lib/*,

​      /usr/local/hadoop/share/hadoop/mapreduce/*,

​      /usr/local/hadoop/share/hadoop/mapreduce/lib/*,

​      /usr/local/hadoop/share/hadoop/yarn/*,

​      /usr/local/hadoop/share/hadoop/yarn/lib/*

​    </value>

  </property>

</configuration>
~~~



yarn-site.xml添加内容

~~~xml
<configuration>

  <property>

​    <name>yarn.resourcemanager.hostname</name>

​    <value>master</value>

  </property>

  <property>

​    <name>yarn.nodemanager.aux-services</name>

​    <value>mapreduce_shuffle</value>

  </property>

</configuration>
~~~



修改workers为以下内容

slave1

slave2



进入/usr/local/hadoop/bin，格式化namenode

namenode -format



保存容器为镜像

docker commit {CONTAINER ID} ubuntu:hadoopinstalled



创建hadoop网络环境

docker network create —dirver=bridge hadoop



### 启动集群服务

启动

docker run -it --network hadoop -h "master" --name "master" -p 9870:9870 -p 8088:8088 ubuntu:hadoopinstalled /bin/bash

docker run -it --network hadoop -h "slave1" --name "slave1" ubuntu:hadoopinstalled /bin/bash

docker run -it --network hadoop -h "slave2" --name "slave2" ubuntu:hadoopinstalled /bin/bash



启动所有节点

cd /usr/local/hadoop/sbin

./start-all.sh



访问localhost:9870和localhost:8088检查启动是否成功



进入/usr/local/hadoop/bin调用wordcount检验

cat ../README.txt > ../test.txt

./hadoop fs -mkdir /input

./hadoop fs -put ../test.txt /input

./hadoop fs -ls /input

./hadoop jar ../share/hadoop/mapreduce/hadoop-mapreduce-examples-3.2.1.jar wordcount /input /output



出现map 100% reduce 100%，执行成功，查看文件

./hadoop fs -ls /output

./hadoop fs -cat /output/part-r-0000
