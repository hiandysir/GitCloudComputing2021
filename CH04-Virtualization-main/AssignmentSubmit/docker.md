## 使用官方安装脚本自动安装

安装命令如下：

curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

或者

curl -sSL https://get.daocloud.io/docker | sh

### 设置仓库

更新 apt 包索引

sudo apt-get update



添加 Docker 的官方 GPG 密钥：

curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add --fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -



9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88 通过搜索指纹的后8个字符，验证您现在是否拥有带有指纹的密钥。

**sudo** **apt-key** fingerprint 0EBFCD88

pub  rsa4096 2017-02-22 **[**SCEA**]**
   9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid      **[** unknown**]** Docker Release **(**CE deb**)** **<**docker**@**docker.com**>**
sub  rsa4096 2017-02-22 **[**S**]**



### 安装 Docker Engine-Community

更新 apt 包索引。

sudo apt-get update

安装最新版本的 Docker Engine-Community 和 containerd ，或者转到下一步安装特定版本：

sudo apt-get install docker-ce docker-ce-cli containerd.io

apt-cache madison docker-ce

测试 Docker 是否安装成功，输入以下指令：

sudo docker run hello-world



