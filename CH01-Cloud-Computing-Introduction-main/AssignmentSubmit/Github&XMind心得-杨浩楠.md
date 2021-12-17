# Github心得

## 什么是 Github?

github是一个基于git的代码托管平台，付费用户可以建私人仓库，我们一般的免费用户只能使用公共仓库，也就是代码要公开。

Github 由Chris Wanstrath, PJ Hyett 与Tom Preston-Werner三位开发者在2008年4月创办。迄今拥有59名全职员工，主要提供基于git的版本托管服务。

## 注册账户以及创建仓库

## Github 安装

## 配置Git

首先在本地创建`ssh key；`

```
$ ssh-keygen -t rsa -C "your_email@youremail.com"
```

接下来我们要做的就是把本地仓库传到github上去

```
$ git config --global user.name "your name"
```

```
$ git config --global user.email "your_email@youremail.com"
```

进入要上传的仓库，右键git bash，添加远程地址：

```
$ git remote add origin git@github.com:yourName/yourRepo.git
```

## 分支

分支是用来将特性开发绝缘开来的。在你创建仓库的时候，*master* 是"默认的"分支。在其他分支上进行开发，完成后再将它们合并到主分支上。

# XMind 基本操作

1. tab （产生子主题）
2. enter （在下方产生并列主题） shift+enter （在上方产生并列主题）
3. Alt+Enter （给某个主题添加标注）
4. 按住Ctrl，选中连续的几个模块，再按下Ctrl+B，把他们用方框框起来
5. 按住Ctrl，选中几个模块，再按下Ctrl+]，即用大括号括起来，给它们添加概要
6. 选中一个模块，按下Ctrl+L，添加一个带箭头的连接线
7. 添加图片，直接拖进来
8. 选中模块，按下Ctrl+Enter，会在他前面产生一个父主题