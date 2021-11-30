# Hadoop安装心得

1. 搭建环境

首先创建新用户

```
sudo useradd -m hadoop -s /bin/bash
```

设施密码

```
sudo passwd hadoop
```

为新用户添加管理权限

```
sudo adduser hadoop sudo
```

切换到新用户

```
su -i hadoop
```

2. Hadoop的环境配置

首先更新系统软件包

```
sudo apt-get update
```

安装SSH

```
sudo apt-get install openssh-server
```

设置无密码登录

```
ssh localhost           #登录到本机#
cd ~/.ssh/
ssh-keygen -t rsa 
cat ./id_rsa.pub >> ./authorized_keys  #实现无密码登录#
```

3. 安装Java

安装openjdk-8-jdk

```
sudo apt-get install openjdk-8-jdk
```

验证安装成功

```
java -version
```

配置Java环境

```
sudo vim /etc/profile
export JAVA_HOME=/usr/lib/jvm/java-8-openjkd-amd64
export JRE_HOME=${JAVA_HOME}/jre  
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib  
export PATH=${JAVA_HOME}/bin:$PATH 
```

使环境生效

```
source /etc/profile  #打开环境配置文件#
export JAVA_HOME=/usr/lib/jvm/java-8-openjkd-amd64  #在文件前加上以上代码#
source ~/.bashrc  #使环境生效#
```

4.Hadoop安装

安装hadoop

```
sudo tar -zxf hadoop-2.6.0.tar.gz -C /usr/local     #解壓到/usr/local中
cd /usr/local/sudo mv ./hadoop-2.6.0/ ./hadoop             #將文件夾名改為hadoop 
sudo chown -R hadoop ./hadoop        #修改文件權限
```

检验hadoop是否可用

```
cd /usr/local/hadoop./bin/hadoop -2.6.0
```

5.配置hadoop基础框架

修改etc/hadoop/core-site.xml

```
<configuration>
<property>
<name>hadoop.tmp.dir</name>
<value>file:/usr/local/hadoop/tmp/</value>
   <description>Abase for other temporary directories.</description>
      </property>
      <property> 
            <name>fs.defaultFS</name>
<value>hdfs://localhost:9000</value>
</property>
</configuration>
```

配置文件hdfs-site.xml

```
<configuration>
<property>
<name>dfs.replication</name>
<value>1</value>
</property>
<property>
<name>dfs.namenode.name.dir</name>
<value>file:/usr/local/hadoop/tmp/dfs/name</value>
</property>
<property>
<name>dfs.datanode.data.dir</name>
<value>file:/usr/local/hadoop/tmp/dfs/data</value>
</property>
</configuration>
```

执行namenode格式化

```
./bin/hdfs namnode -format
```

验证启动是否成功

```
./sbin/start-dfs.sh
```
