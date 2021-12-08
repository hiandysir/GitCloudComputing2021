# Git使用心得



## 1.  Git简介

Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。

Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。



## 2. Git安装

Git下载地址：  [Git for Windows](https://gitforwindows.org/)

下载 git-xx.exe 文件 可以直接安装git， 如下图  根据安装指引一步步执行即可完成Git安装



![](https://www.runoob.com/wp-content/uploads/2015/02/20140127131250906)



- 在window cmd控制台中输入git --version查看是否安装成功

```
git --version
```

- 设置用户名与邮箱

```
git config –-global user.name 'user_name'
git config –-global user.email 'user_email'
```

- 查看git配置

```
git config –-list
```



## 3. 创建本地仓库

- 初始化一个本地版本库

```
C:\Users\admin\test> git init
Initialized empty Git repository in C:/Users/admin/test/.git/
```

- 查看git仓库状态  

```
C:\Users\admin\test> git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        text.txt

nothing added to commit but untracked files present (use "git add" to track)
```

- 添加需要的提交文件

```
C:\Users\admin\test>git add text.txt
C:\Users\admin\test>git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   text.txt

```

- 提交变更

```
git commit -m "<填寫版本資訊>"
```



## 4. git常用命令

**git add**

- 暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。

**git commit**

- 暂存区的目录树写到版本库（对象库）中，HEAD 指向分支会做相应的更新。

**git reset HEAD**

- 暂存区的目录树会被重写，被HEAD 指向分支的目录树所替换，但是工作区不受影响。

**git rm --cached**

- 直接从暂存区删除文件，工作区则不做出改变。

**git checkout 或者 git checkout –**

- 暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。

**git checkout HEAD 或者 git checkout HEAD**

- HEAD 指向的分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

**git clone git@sdysit.com:/sdyouth/git/yangmaosen.git**

- 从远程库克隆项目

**git fetch**

- 从远程的分支获取最新的版本到本地。

**get push**

- 将本地版本库的分支推送到远程库上对应的分支。