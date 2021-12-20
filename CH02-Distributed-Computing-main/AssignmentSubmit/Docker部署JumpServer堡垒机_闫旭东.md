# Docker部署JumpServer堡垒机



**JumpServer 是全球首款开源的堡垒机，使用 GPLv3 开源协议，是符合 4A 规范的运维安全审计系统。**

**JumpServer 使用 Python 开发，遵循 Web 2.0 规范，配备了业界领先的 Web Terminal 方案，交互界面美观、用户体验好。**

**JumpServer 采纳分布式架构，支持多机房跨区域部署，支持横向扩展，无资产数量及并发限制。**

**改变世界，从一点点开始 ...**





#### 1）生成随机加密密钥

```
if [ ! "$SECRET_KEY" ]; then
  SECRET_KEY=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 50`;
  echo "SECRET_KEY=$SECRET_KEY" >> ~/.bashrc;
  echo $SECRET_KEY;
else
  echo $SECRET_KEY;
fi  
if [ ! "$BOOTSTRAP_TOKEN" ]; then
  BOOTSTRAP_TOKEN=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 16`;
  echo "BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN" >> ~/.bashrc;
  echo $BOOTSTRAP_TOKEN;
else
  echo $BOOTSTRAP_TOKEN;
fi

```

###### ![image](https://user-images.githubusercontent.com/90243359/146743209-b33c3df9-2ccf-4172-a7f1-c93816d55139.png)




#### 2）下载JumpServer的Docker镜像

```
docker pull registry.jumpserver.org/public/jumpserver:1.0.0
```

#### 3）部署JumpServer环境

```
docker run --name jms_all -d \
  -p 80:80 -p 2222:2222 \
  -e SECRET_KEY=$SECRET_KEY \
  -e BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN \
  -v /opt/jumpserver/data:/opt/jumpserver/data \
  -v /opt/jumpserver/mysql:/var/lib/mysql \
  jumpserver/jms_all:v2.1.2
```

###### ![image](https://user-images.githubusercontent.com/90243359/146743301-62f05e76-5463-4285-8f11-a2434c085614.png)

#### 4）访问localhost

账号密码均为admin

###### ![image](https://user-images.githubusercontent.com/90243359/146743442-16fb9e52-eab2-4dcb-b7b7-cc2d3c87ab60.png)

#### 5)进行资产设置（使用个人腾讯云服务器设置）

###### ![image](https://user-images.githubusercontent.com/90243359/146743548-0f0475cd-6a3b-4bf4-80e9-41113a234668.png)

###### ![image](https://user-images.githubusercontent.com/90243359/146743597-1bd3b7e6-4b36-4f01-8912-4b8e291ca34a.png)
###### ![image](https://user-images.githubusercontent.com/90243359/146743726-71524053-457c-442f-b09b-abc2e4626820.png)


