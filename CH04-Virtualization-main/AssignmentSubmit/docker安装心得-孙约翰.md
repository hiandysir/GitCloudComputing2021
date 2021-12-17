### docker安装心得

\1.卸载旧版本

```
sudo apt-get remove docker docker-engine docker.io containerd runc
/var/lib/docker
```

\2. 安装依赖包

sudo apt-get update

sudo apt-get install \

  apt-transport-https \

  ca-certificates \

  curl \

  gnupg \

  lsb-release



\3. 添加docker官方密钥

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

 

\4. 设置安装docker

echo \

 "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \

 $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

 

\5. 安装docker

运行命令sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

![image-20211217195637194](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211217195637194.png)

\6.验证docker

![image-20211217195704107](C:\Users\sy\AppData\Roaming\Typora\typora-user-images\image-20211217195704107.png)