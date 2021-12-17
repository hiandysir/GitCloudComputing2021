---
CloudComputing2021
ch1 study notes
Author：zoxiii
---

[TOC]

> ==所贴blog均为本人学习所作步骤，内容太多，所以直接引用了==


# 1、MarkDown学习

&emsp;&emsp;之前本科就使用过Typora、用来总结笔记还是很不错的。

&emsp;&emsp;以下两个功能是我认为比较好的、尤其是公式功能，当你学会了认识公式中的符号是怎么使用代码打出来的，不仅学会了符号的读法、而且在使用Word打公式的时候就会节省很多时间。

- `公式`功能：如`$\left(\frac{x}{y}\right)$ `
- `流程图`功能


# 2、Git的学习
## 1.1、GitBash的使用
（1）第一次使用需要注册git用户

```c
$ git config --global user.name “你的名字”
$ git config --global user.email “你的邮箱”
```
（2）创建本地仓库或者拉取github或gitee的仓库到本地

（3）提交项目
```c
$ git pull origin master
$ git add .
$ git commit -m “你的第一次提交”
$ git push origin master
```
## 1.2、进阶：SourceTree及TortoiseGit的使用
1. 因为在之前就使用的是TortoiseGit，所以这里是以该软件的使用为主
   使用过程中遇到过的错误：
   【1】[Git上传本地文件到github时，执行到push时报错的解决办法]()
   【2】[Git在pull仓库pull下来的时候遇到错误git did not exit cleanly (exit code 1)的解决办法]( https://blog.csdn.net/qq_41315788/article/details/121045713)

2. SourceTree的使用、可视化界面相比较TortoiseGit比较好使用![](https://img-blog.csdnimg.cn/2f5186ec0fe24ab18c3e94cf6740e3bd.png)
3. 在学习过后，觉得还是可视化界面比较香、但这样其实比较难学到git的命令，所以想要提升自己还是得使用Gitbash。


# 3、虚拟机的安装
## 2.1、工具：VirtualBox和VMWare WorkStation的安装与选择

- VirtualBox 是一款开源虚拟机软件。VirtualBox是由德国Innotek公司开发，由Sun Microsystems公司出品的软件，使用Qt编写，在Sun被Oracle收购后正式更名成Oracle VM VirtualBox。

[VM VirtualBox安装过程](https://blog.csdn.net/qq_41315788/article/details/120175617)

- VMWare WorkStation也是之前有学习过的软件，个人觉得`快照`的功能实在是yyds，可以通过拍摄快照来保存虚拟机的当前状态，这样在输错命令，部署环境失败的时候还可以修改。

![](https://img-blog.csdnimg.cn/b8f385a6885b42bbac9364e07a229438.png)

## 2.2、虚拟机映像Ubuntu和Centos的选择

&emsp;&emsp;在尝试使用VM VirtualBox来安装一个Ubuntu系统的时候，需要的错误是tools工具配置的时候失败了，由于时间问题，所以没有继续在尝试，依旧使用VMWare WorkStation来安装Linux系统。

Ubuntu21.04操作步骤如下：

1. [选择Ubuntu系统](https://blog.csdn.net/qq_41315788/article/details/120415201)
2. [配置vmware-tools](https://blog.csdn.net/qq_41315788/article/details/120443319)
3. [配置静态IP](https://blog.csdn.net/qq_41315788/article/details/121205795)

Centos 7操作步骤如下：

1. [选择Centos系统](https://blog.csdn.net/qq_41315788/article/details/120197645)
2. [配置vmware-tools](https://blog.csdn.net/qq_41315788/article/details/120216207)
3. [配置静态IP](https://blog.csdn.net/qq_41315788/article/details/109263850)

&emsp;&emsp;这些步骤是使用一个新的虚拟机前必须要操作的，当执行完这些步骤，一个新的可用的虚拟机就出现了，最好的方式就是在此时保存一个快照，此后只需要`克隆`该状态的虚拟机修改一个新的IP就可以使用了，非常方便。

&emsp;&emsp;当然如果可以的话，还可以修改一下hostname，即主机地址，这样在之后配置通信的时候比较有用。

# 4、XShell的使用

&emsp;&emsp;虽然我们配置了vmware-tools，鼠标可以在虚拟机和我们本身的电脑系统中切换，以及复制文件，但是如果是大的文件，在传送过程中很容易出现文件损坏的情况，所以我们使用XShell来确保文件的质量。

主要用到以下两个功能：

1. [向虚拟机上传文件](https://blog.csdn.net/qq_41315788/article/details/109266568)
2. [从虚拟机下载文件到本地](https://blog.csdn.net/qq_41315788/article/details/121564843)

