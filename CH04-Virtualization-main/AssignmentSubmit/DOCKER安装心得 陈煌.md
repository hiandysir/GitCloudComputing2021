#### DOCKER安装心得

1. 更新`apt`包索引並安裝包以允許`apt`通過 HTTPS 使用存儲庫：

   ```
   $ sudo apt-get update
   
   $ sudo apt-get install \
       ca-certificates \
       curl \
       gnupg \
       lsb-release
   ```

2. 添加Docker官方的GPG密鑰：

   ```
   $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
   ```

3. 使用以下命令設置**穩定**存儲庫。

   ```
   $ echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
     $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
   ```

#### 安裝 Docker 引擎

1. 更新`apt`包索引，安裝*最新版本*的Docker Engine和containerd，或者到下一步安裝特定版本：

   ```
    $ sudo apt-get update
    $ sudo apt-get install docker-ce docker-ce-cli containerd.io
   ```

2. 通過運行`hello-world` 映像驗證 Docker Engine 是否已正確安裝。

   ```
   $ sudo docker run hello-world
   ```