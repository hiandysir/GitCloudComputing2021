## Github Desktop安装与简单使用

### Github Desktop安装

下载地址：https://desktop.github.com

这里我选择在MacOS下安装，MacOS自带git环境，版本号：git version 2.30.1 (Apple Git-130)

### Github Desktop简单使用

登陆Github账户后，可以通过Create a New Repository创建仓库，保存在本地路径Local Path下，可以在Descriprition中添加对仓库的描述。

![Create a New Repository](https://github.com/SHAOHUASONGgit/DataScienceRepo/raw/main/CloudCmoputingMarkDown/picture/createRepomd1.png)

我已经创建了一个仓库，用于存放日常学习中的产生的代码、作业及数据等等。在仓库文件夹下新建或修改文件后，可以在Desktop中的Changes中看到具体改动内容，选择确认修改的内容后commit到main主干上。

![Ready to Commit](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/readyToCommitmd1.png?token=AVNKEFWZXKVZ2DFOY5SVO5LBRJ3F6)

在History中可以查看提交历史，Push以后上传到Github。

![Ready to Push](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/readyToPushmd1.png?token=AVNKEFVZGCSZHS2TGQ772ETBRJ3I4)

可以在Github中Fork其他仓库后Pull到本地，我已经将本门课程的仓库拉取到本地，并Fetch更新或者Commit提交，Commit时应注意需要提交的文件，如图，我只准备提交“邵华松.md”这个文档。

![Fork and Fetch](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/forkAndFetchmd1.png?token=AVNKEFWK76CBMZNZHUEZLLLBRJ3NE)

写这篇markdown时遇到图片加载失败的错误，原因是引用Github上的图片时，仓库权限是private，设为public后解决。