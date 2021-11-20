#                               docker 安装过程：

## 1、准备阶段

### 1.1、curl

使用以下指令安装curl

```bash
sudo apt-get update
sudo apt-get install  curl 
```

### 1.2、安装docker

按照[官网教程](https://docs.docker.com/engine/install/ubuntu/)手动安装docker

更新apt索引并安装以下依赖包

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg lsb-release
```

添加docker官方的GPG钥匙

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

使用以下指令设置稳定版仓库

```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

更新 apt 包索引并安装最新版本的 Docker Engine-Community 和 containerd

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```



使用以下指令看看docker是否安装完成

```bash
docker --version
```





