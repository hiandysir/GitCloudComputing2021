#                               Hadoop 安装及伪分布式配置：

* （本心得参考厦门大学数据实验室教程）

# 一、部署环境

* 1、系统环境： ubuntu-20.04.3-desktop-amd64

* 2、Hadoop版本：Hadoop 2.x 

装好了 Ubuntu 系统之后，在安装 Hadoop 前还需要做一些必备工作。

## 1、创建hadoop用户

在终端中输入如下命令创建新用户 :

```shell
sudo useradd -m hadoop -s /bin/bash
```

接着使用如下命令设置密码，按提示输入两次密码：

```shell
sudo passwd hadoop
```



可为 hadoop 用户增加管理员权限，方便部署，避免一些对新手来说比较棘手的权限问题：

```shell
sudo adduser hadoop sudo
```

最后注销当前用户，在登陆界面中选择刚创建的 hadoop 用户进行登陆。

## 2、更新apt

 打开终端窗口，执行如下命令：

```Shell
sudo apt-get update
```

```shell
sudo apt-get install vim
```

安装软件时若需要确认，在提示处输入 y 即可。

## 3、安装SSH、配置SSH无密码登陆

集群、单节点模式都需要用到 SSH 登陆，Ubuntu 默认已安装了 SSH client，此外还需要安装 SSH server：

```shell
sudo apt-get install openssh-server
```

安装后，可以使用如下命令登陆本机：

```Shell
ssh localhost
```

此时会有SSH首次登陆提示，输入 yes 。然后按提示输入密码 hadoop，这样就登陆到本机了。

![SSH首次登陆提示](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2014/08/install-hadoop-08-ssh-continue.png)

接下来配置无密码登录

首先退出刚才的 ssh，就回到了我们原先的终端窗口，然后利用 ssh-keygen 生成密钥，并将密钥加入到授权中：

```Shell
exit                           # 退出刚才的 ssh 
localhostcd ~/.ssh/            # 若没有该目录，请先执行一次ssh 
localhostssh-keygen -t rsa     # 会有提示，都按回车就可以
cat ./id_rsa.pub >> ./authorized_keys  # 加入授权
```

此时再用 `ssh localhost` 命令，无需输入密码就可以直接登陆了。

## 4、安装Java环境

把压缩格式的文件jdk-8u162-linux-x64.tar.gz下载到本地电脑，保存在“/home/bruce/Downloads/”目录下。

在Linux命令行界面中，执行如下Shell命令（注意：当前登录用户名是hadoop）：

```Shell
cd /usr/libsudo mkdir jvm #创建/usr/lib/jvm目录用来存放JDK文件
cd ~ #进入hadoop用户的主目录
cd Downloads  #注意区分大小写字母，刚才已经通过FTP软件把JDK安装包jdk-8u162-linux-x64.tar.gz上传到该目录下
sudo tar -zxvf ./jdk-8u162-linux-x64.tar.gz -C /usr/lib/jvm  #把JDK文件解压到/usr/lib/jvm目录下
```

JDK文件解压缩以后，可以执行如下命令到/usr/lib/jvm目录查看一下：

```Shell
cd /usr/lib/jvmls
```

在/usr/lib/jvm目录下执行如下命令，设置环境变量：

```Shell
cd ~vim ~/.bashrc
```

在这个文件的开头位置，添加如下几行内容：

```
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_162
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```

保存.bashrc文件并退出vim编辑器。然后，继续执行如下命令让.bashrc文件的配置立即生效：

```Shell
source ~/.bashrc
```

这时，可以使用如下命令查看是否安装成功：

```Shell
java -version
```

如果能够在屏幕上返回如下信息，则说明安装成功：

```Shell
hadoop@ubuntu:~$ java -version
java version "1.8.0_162"
Java(TM) SE Runtime Environment (build 1.8.0_162-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.162-b12, mixed mode)
```

# 二、安装及伪分布式配置

* 版本：hadoop-2.7.1.tar.gz

## 1、Hadoop安装

 可以通过 http://mirror.bit.edu.cn/apache/hadoop/common/ 或者 http://mirrors.cnnic.cn/apache/hadoop/common/ 下载，一般选择下载最新的稳定版本，即下载 “stable” 下的 **hadoop-2.x.y.tar.gz** 这个格式的文件。

将 Hadoop 安装至 /usr/local/ 中，进如安装文件的下载目录后：

```shell
sudo tar -zxf hadoop-2.6.0.tar.gz -C /usr/local    # 解压到/usr/local中
cd /usr/local/sudo mv ./hadoop-2.6.0/ ./hadoop            # 将文件夹名改为hadoop
sudo chown -R hadoop ./hadoop       # 修改文件权限
```

Hadoop 解压后即可使用。输入如下命令来检查 Hadoop 是否可用，成功则会显示 Hadoop 版本信息：

```shell
cd /usr/local/hadoop./bin/hadoop version
```

## 2、Hadoop伪分布式配置

Hadoop 可以在单节点上以伪分布式的方式运行节点既作为 NameNode 也作为 DataNode，同时，读取的是 HDFS 中的文件。

Hadoop 的配置文件位于 /usr/local/hadoop/etc/hadoop/ 中，伪分布式需要修改2个配置文件 **core-site.xml** 和 **hdfs-site.xml** 。Hadoop的配置文件是 xml 格式，每个配置以声明 property 的 name 和 value 的方式来实现。

修改配置文件 **core-site.xml** 

```shell
hgedit ./etc/hadoop/core-site.xml
```

将当中的

```xml
<configuration>
</configuration>
```

修改为下面配置：

```xml
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/usr/local/hadoop/tmp</value>
        <description>Abase for other temporary directories.</description>
    </property>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```



同样的，修改配置文件 **hdfs-site.xml**：

```xml
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

配置完成后，执行 NameNode 的格式化:

```shell
cd /usr/local/hadoop./bin/hdfs namenode -format
```

成功的话，会看到 “successfully formatted” 和 “Exitting with status 0” 的提示，若为 “Exitting with status 1” 则是出错。

如果在这一步时提示 **Error: JAVA_HOME is not set and could not be found.** 的错误，则说明之前设置 JAVA_HOME 环境变量那边就没设置好，请按教程先设置好 JAVA_HOME 变量。如果已经按照前面教程在.bashrc文件中设置了JAVA_HOME，还是出现 **Error: JAVA_HOME is not set and could not be found.** 的错误，那么，请到hadoop的安装目录修改配置文件“/usr/local/hadoop/etc/hadoop/hadoop-env.sh”，在里面找到“export JAVA_HOME=${JAVA_HOME}”这行，然后，把它修改成JAVA安装路径的具体地址，比如，“export JAVA_HOME=/usr/lib/jvm/default-java”，然后，再次启动Hadoop。

接着开启 NameNode 和 DataNode 守护进程。

```shell
cd /usr/local/hadoop./sbin/start-dfs.sh  #start-dfs.sh是个完整的可执行文件，中间没有空格
```

若出现如下SSH提示，输入yes即可。

![启动Hadoop时的SSH提示](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2014/08/install-hadoop-15-ssh-continue.png)

启动时可能会出现如下 WARN 提示：WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform… using builtin-java classes where applicable WARN 提示可以忽略，并不会影响正常使用。

```shell
./sbin/start-dfs.sh         #启动 Hadoop
```

启动完成后，可以通过命令 `jps` 来判断是否成功启动，若成功启动则会列出如下进程: “NameNode”、”DataNode” 和 “SecondaryNameNode”（如果 SecondaryNameNode 没有启动，请运行 sbin/stop-dfs.sh 关闭进程，然后再次尝试启动尝试）。如果没有 NameNode 或 DataNode ，那就是配置不成功，请仔细检查之前步骤，或通过查看启动日志排查原因。

![通过jps查看启动的Hadoop进程](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2014/08/install-hadoop-16-jps.png)

***



## 3、异常问题处理

如果启动 Hadoop 时遇到输出非常多“ssh: Could not resolve hostname xxx”的异常情况，如下图所示：

![启动Hadoop时的异常提示](http://dblab.xmu.edu.cn/blog/wp-content/uploads/2014/08/install-hadoop-15-resolve-hostname.png)

这个并不是 ssh 的问题，可通过设置 Hadoop 环境变量来解决。首先按键盘的 **ctrl + c** 中断启动，然后在 ~/.bashrc 中，增加如下两行内容（设置过程与 JAVA_HOME 变量一样，其中 HADOOP_HOME 为 Hadoop 的安装目录）：

```shell
export HADOOP_HOME=/usr/local/hadoopexport HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
```

保存后，务必执行 `source ~/.bashrc` 使变量设置生效

****



