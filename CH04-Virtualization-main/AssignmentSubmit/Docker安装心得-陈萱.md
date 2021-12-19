### Docker安装心得

\1.输入命令sudo apt-get remove docker docker-engine docker.io containerd runc

清除已安装的环境

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsA9A6.tmp.jpg) 

说明此前没有安装过docker。

\2. 安装依赖包

sudo apt-get update

sudo apt-get install \

  apt-transport-https \

  ca-certificates \

  curl \

  gnupg \

  lsb-release

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsA9A7.tmp.jpg) 

\3. 添加docker官方密钥

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

 

\4. 设置安装docker

echo \

 "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \

 $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

 

\5. 安装docker

运行命令sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsA9A8.tmp.jpg) 

\6. 运行sudo docker run hello-world，验证是否安装成功

![img](file:///C:\Users\sy\AppData\Local\Temp\ksohtml\wpsA9B9.tmp.jpg) 

 

 

 

 

 