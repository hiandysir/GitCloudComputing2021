

# mininet部署心得



## 1. mininet简介

 Mininet是由一些虚拟的终端节点（end-hosts）、交换机、路由器连接而成的一个网络仿真器，它采用轻量级的虚拟化技术使得系统可以和真实网络相媲美。

 Mininet可以很方便地创建一个支持SDN的网络：host就像真实的电脑一样工作，可以使用ssh登录，启动应用程序，程序可以向以太网端口发送数 据包，数据包会被交换机、路由器接收并处理。有了这个网络，就可以灵活地为网络添加新的功能并进行相关测试，然后轻松部署到真实的硬件环境中。



## 2. 安装Mininet虚拟机

#### 2.1  下载mininet镜像

  [mininet 镜像下载地址 ](https://github.com/mininet/mininet/releases/)



![image-20211201122400181](http://m.qpic.cn/psc?/V51qUp8Q4ORGKu4Dhczg2JyCx12uI1yY/45NBuzDIW489QBoVep5mcW3FqHiYebba*XzIT5DOshko3dbqh1fyYqcpej2tnyDlZJRHSKqPwRLMDwkeYaFYpkklstxiVqBelTfckRpTGyE!/b&bo=ngOVAQAAAAADFzs!&rf=viewer_4)



#### 2.1 导入mininet镜像

![](http://m.qpic.cn/psc?/V51qUp8Q4ORGKu4Dhczg2JyCx12uI1yY/45NBuzDIW489QBoVep5mcfbEFf.KD4XWRGr423X2lfTXPzGjg2Ef7clo2EeQRVJL6Zu5p7f8pvBT9y.W8nrkVoM9iVjfQMukfXv8zjyvUJs!/b&bo=RAPyAQAAAAADF4Y!&rf=viewer_4)





![屏幕截图 2021-12-01 122709](http://m.qpic.cn/psc?/V51qUp8Q4ORGKu4Dhczg2JyCx12uI1yY/45NBuzDIW489QBoVep5mcfbEFf.KD4XWRGr423X2lfQnOxzzGXZvQMFfb.HWggUBfxFmUc0IzUVJlCheWOoEkE2FbYPlNuI6LJaExUufiF4!/b&bo=mAS*AgAAAAADFxM!&rf=viewer_4)





![屏幕截图 2021-12-01 122729](http://m.qpic.cn/psc?/V51qUp8Q4ORGKu4Dhczg2JyCx12uI1yY/45NBuzDIW489QBoVep5mcfbEFf.KD4XWRGr423X2lfQ5VzhHIoqTI5HNxTd0qveq1dXe*N1lFI4r3ZvxY9Rn45tJxk9N2B5nb0tRb*baGjU!/b&bo=EATRAgAAAAADF*U!&rf=viewer_4)



#### 2.3 启动mininet虚拟机

![屏幕截图 2021-12-01 124451](http://m.qpic.cn/psc?/V51qUp8Q4ORGKu4Dhczg2JyCx12uI1yY/45NBuzDIW489QBoVep5mcfbEFf.KD4XWRGr423X2lfQPpAr1W6ftEXpb5yqO5D*td4T*RjKfKadk.S9.ePEsEYMaR65xyTH0rpr1rkEDiRs!/b&bo=NgPIAgAAAAADF80!&rf=viewer_4)

#### 2.4 登录虚拟机

![屏幕截图 2021-12-01 130421](http://m.qpic.cn/psc?/V51qUp8Q4ORGKu4Dhczg2JyCx12uI1yY/45NBuzDIW489QBoVep5mceYIVbGcetdtABA6gU8bqhSHPqqkHDPlBgYgKhjh2nCX8Y7E*uiVqgPOFaoAMWqRgJKCszeLcFgMuuUEUWTclLs!/b&bo=fgLCAAAAAAADF4w!&rf=viewer_4)

#### 2.5 测试mininet是否安装正确

```
mininet@mininet-vm:~/mininet/util$ sudo mn --test pingall
*** Creating network
*** Adding controller
*** Adding hosts:
h1 h2 
*** Adding switches:
s1 
*** Adding links:
(h1, s1) (h2, s1) 
*** Configuring hosts
h1 h2 
*** Starting controller
c0 
*** Starting 1 switches
s1 ...
*** Waiting for switches to connect

s1 
*** Ping: testing ping reachability
h1 -> h2 
h2 -> h1 
*** Results: 0% dropped (2/2 received)
*** Stopping 1 controllers
c0 
*** Stopping 2 links
..
*** Stopping 1 switches
s1 
*** Stopping 2 hosts
h1 h2 
*** Done
completed in 5.624 seconds

```

























