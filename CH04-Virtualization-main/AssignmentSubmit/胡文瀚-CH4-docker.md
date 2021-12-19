# docker

##  以自家服务器的ddns-go为例
```command
# 安装docker
yum install docker

# 拉取镜像
docker pull jeessy/ddns-go

# 运行镜像
docker run -d --name ddns-go --restart=always --net=host jeessy/ddns-go
```

docker不过多赘述，随便照dockerhub就能运行起来  
本案例所用镜像：  
[ddns-go](https://hub.docker.com/r/jeessy/ddns-go)
若在国内有需要可以配置镜像加速