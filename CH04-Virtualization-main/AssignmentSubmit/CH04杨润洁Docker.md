# 实践：Docker容器心得

1.  我采用的安装教程是github上的开源安装教程。由于我的本科不是计算机专业，所以一直对容器的概念不是很了解，专业化的名词对我来说也是晦涩难懂的，在我查阅了容器的相关名词，比如:镜像文件、资源隔离之后，对容器理解的还不是很透彻。但是，看了这个教程之后，我对容器的理解就更进一步了，如果用一句话来概括的话，就是“**容器化是将一切打包放入盒子中，使其在各种环境中都可移植且可回溯**”，别人可以不用配置复杂的环境而直接调用盒子中已经配置好的文件。



2.  我们首先需要在Ubuntu操作系统上安装Docker容器，安装完后测试是否安装成功，这是成功安装后的图片：

![img](https://github.com/yrj903474887/YRJpicture/blob/main/images/4.1.png) 

接下来就可以使用Docker容器进行很多操作



3.  **<u>运行第一个Docker容器：拉取镜像并运行容器。我制作了流程图直观概括一下整个过程</u>**：

![img](https://github.com/yrj903474887/YRJpicture/blob/main/images/4.2.png) 



首先，将容器镜像拉取到本地:

```
docker pull alpine
```



查看本地已经有的镜像文件：

```
docker image
```

**<u>由于我之前在docker里安装了Hadoop，所以我现在的容器里有alpine、hadoop_proto、java_ssh、hello-world和centos五个镜像文件，如图所示：</u>**![](https://github.com/yrj903474887/YRJpicture/blob/main/images/4.3.PNG)



运行容器

```
docker run alpine ls -1
```

**<u>结果显示</u>**：![](https://github.com/yrj903474887/YRJpicture/blob/main/images/4.4.PNG)

**<u>原理就是上面的流程图</u>**

