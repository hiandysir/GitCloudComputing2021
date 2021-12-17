### GitHub使用心得

#### 一、下载并安装git

\1. 在windows环境下使用msysgit来安装，下载地址：http://code.google.com/p/msysgit/

\2. 下载后双击，按提示一路next进行安装，其包括一个命令行工具和一个GUI。

\3. 进行初始化设置，用户名及邮箱。打开git bash，执行命令：

git config –global user.name your_name

git config –global user.email your_email

#### 二、生成SSH Key

使用git前需要先生成一个SSH Key，保证本地与服务器之间通信的安全。在git bash中执行：

ssh-keygen -C your_email -t rsa

email地址是注册git时的地址，下面一路next。有一步会提示输入密码（passphrase），可以填入密码或回车留空。之后窗口会显示生成的rsa文件存放的位置。

#### 三、创建github账号

github是一个代码管理及分享的平台，注册即可使用其免费服务，但要注意其免费服务是public模式。地址：www.github.com。

1.在github网站上注册后，上传公钥。在github界面上，选择account settings，然后选择SSH Public Key，选择新加。title可随便命名；找到上一步生成的密钥文件（id_rsa.pub），用文本编辑器打开，复制内容并粘贴至key框内，完成。可 使用如下命令进行测试看是否连通：

ssh -v git@github.com

2.登录github，点击“new repository”，填写项目名称、描述，即可创建一个新的项目。

#### 四、建立本地git仓库

在git bash中执行以下命令来创建本地git仓库：

cd your_path

mkdir my-code

cd my-code

mkdir project-directory

cd project-directory]

git init

touch README.txt

\#(add some files)

git add .

git commit -m ‘first commit’

解释：先创建git仓库目录，然后建立项目并初始化git仓库，init命令是在项目目录下创建一个隐藏目录（.git），该目录就是git用来管 理软件版本的仓库。然后使用touch命令创建readerme文件，在项目目录里添加文件，add命令是将当前更改或新增的文件加入到git索引中，表 示记入了版本历史，这是提交前必须执行的一步；commit命令提交当前工作空间修改内容，是真正将文件添加到git仓库中去，-m选项给出每次添加的简 短说明。

#### 五、将项目提交到github

执行如下命令：

git remote add origin git@github.com:username/project-directory.git

使用git remote add命令来增加一个远程服务器端版本库，url地址为git@github.com:username/project-directory.git，名称为origin。

执行以下命令将本地档案同步到服务器端：

git push -u origin master

