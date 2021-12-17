---
CloudComputing2021
ch4 study notes
Author：zoxiii
---

[TOC]

> 已发布blog

# 1、安装Docker

[centos安装Docker官方文档](https://www.runoob.com/docker/centos-docker-install.html)

## （1）卸载旧版本
```bash
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
## （2）设置仓库
```bash
yum install -y yum-utils   ## 安装所需的软件包
yum-config-manager \
    --add-repo \
	http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo #推荐使用阿里云的加速下载，比较快
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/6f0d3a0cbaae4d7789eeee9da5e10c9b.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/a9d19d888a2f4f6285437dd043de6796.png)

## （3）安装Docker Engine-Community
```bash
yum install docker-ce docker-ce-cli containerd.io    ## 安装最新的版本
```
- 如果提示您接受 GPG 密钥，请选是。
- Docker 安装完默认未启动。并且已经创建好 docker 用户组，但该用户组下没有用户。
```bash
`方法二`## 要安装特定版本的 Docker Engine-Community，请在存储库中列出可用版本，然后选择并安装
### 1、列出可用稳定版本
yum list docker-ce --showduplicates | sort -r
	#> docker-ce.x86_64  3:18.09.1-3.el7         docker-ce-stable
### 2、通过完整的软件包名称安装特定版本
yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
	#> 例如：docker-ce-18.09.1
```
## （4）启动Docker
```bash
systemctl start docker   ## 启动Docker
## service docker start
docker version           ## 查看docker版本
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e265d1a604fc46c29081da6837376b08.png)

## （5）测试是否安装成功
```bash
docker run hello-world     ## 运行的时候会把该镜像下载下来
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/16237ca21464476684ff8e35c31a2934.png)

## （6）关闭Docker服务
```bash
systemctl stop docker
## service docker stop
```
## （7）卸载Docker
```bash
yum remove docker-ce docker-ce-cli containerd.io    ## 删除安装包
rm -rf /var/lib/docker         ## 删除镜像、容器、配置文件等内容
rm -rf /var/lib/containerd     ## 没有就不需要删除
## /var/lib/docker docker的默认工作路径
```

# 2、Docker 镜像加速

[参考地址](https://www.runoob.com/docker/docker-mirror-acceleration.html)

## （1）找镜像地址
[阿里云镜像获取地址](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)
![在这里插入图片描述](https://img-blog.csdnimg.cn/f7459ab9fc5e48ba8b519e48fcc099eb.png)

## （2）修改文件
在` /etc/docker/daemon.json `中写入如下内容（如果文件不存在请新建该文件）

```bash
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{"registry-mirrors":["https://reg-mirror.mirror.aliyuncs.com"]}    # 这句不要直接复制！！！！
EOF
```
<!--{"registry-mirrors": ["https://rt0tkr6m.mirror.aliyuncs.com"]}-->

## （3）重新启动服务

```bash
systemctl daemon-reload      ## 重启daemon文件
systemctl restart docker     ## 重启Docker
```

```bash
docker info        ## 显示docker的系统版本，包括镜像和容器的数量
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/135d23895ef34200b48a603341f103f5.png)

# 3、镜像

## （1）查看镜像
```bash
docker images    ## 查看当前docker中下载的镜像
```
## （2）载入镜像

```bash
docker pull ubuntu    ## 载入ubuntu镜像（可选择其他的导入）
```

## （3）启动镜像
```bash
docker run -p 本机映射端口:镜像映射端口 -d  --name 启动镜像的容器名称 -e 镜像启动参数  镜像名称:镜像版本号
例：docker run -p 3306:3306 -d --name mysql01 -e MYSQL_ROOT_PASSWORD=admin mysql:5.6
```
## （4）删除镜像

```bash
docker rmi ubuntu
```



# 4、容器

## （1）查看container
```bash
docker ps    	## 查看运行中的container
docker ps -a 	## 查看所有container
```
## （2）启动docker某个image的container
- `containerID`指容器的id，也可以使用容器name
```bash
docker run -t -i --name <containerName> ubuntu /bin/bash   ## 使用 ubuntu 镜像启动一个容器
docker run <containerID>
docker start <containerID>      ## 启动一个已停止的容器
```
- docker run：启动container
- ubuntu：你想要启动的image
- -t：进入终端
- -i：获得一个交互式的连接，通过获取container的输入
- -d：启动但不进入容器终端
- /bin/bash：在container中启动一个bash shell
## （3） 进入容器终端
```bash
docker attach <containerID>                ## container运行在后台，进入它的终端（不推荐使用）
docker exec -it <containerID> /bin/bash    ## container运行在后台，进入它的终端，且退出时container仍然在后台运行
```

## （4）退出container
```bash
exit    	                    ## 输入命令
Ctrl + D  	                    ## 按键交互
```
## （5）停止和重启container
```bash
docker stop <containerID>       ## 停止容器
docker restart <containerID>    ## 重启容器
```
## （6）删除container
```bash
docker rm -f <containerID>      ## 删除容器
docker container prune          ## 清理所有处于停止状态的容器
```
## （7）导出容器

```bash
docker export <containerID> > youname.tar
```

# 5、Test

[镜像库地址](https://hub.docker.com/)

## （1）CentOS 镜像

```bash
docker pull centos:centos7
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/3183e990eda3499891d7ff77f05388dc.png)

```bash
docker run -itd --name centos01 centos:centos7
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/e1884226267b41a69f2f6b0cf708ee35.png)

## （2）MySQL镜像
```bash
docker pull mysql    ## docker 中下载 mysql
```

```bash
docker run --name mysql01 -p 9492:3306 -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_DATABASE=zoxSQL -d mysql  ## 启动
```
- `-p 9492:3306`：将容器的3306端口映射到主机的9492端口
- `-e MYSQL_ROOT_PASSWORD=123456`：初始化root用户的密码
- `-e MYSQL_DATABASE=zoxSQL`：建立数据库zoxSQL，这里可以设置你的数据库名称
- `-d`: 后台运行容器，并返回容器ID
- `mysql`: mysql镜像
- `--name mysql01`：容器名

![在这里插入图片描述](https://img-blog.csdnimg.cn/8e5cdf8ffcd4490b8bdc9ed66a655309.png)


```bash
docker exec -it mysql01 /bin/bash   ## 进入容器
```

## （3）game2048镜像

```bash
docker pull yakexi007/game2048
```

```bash
docker run -d -p 800:80 --name game01 yakexi007/game2048
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/a2c9a49d3d1d46149d923897a95ae589.png)
访问网页`192.168.184.5:800`
![在这里插入图片描述](https://img-blog.csdnimg.cn/1b23ca879ae84753a536b2fc0839bc1b.png)

