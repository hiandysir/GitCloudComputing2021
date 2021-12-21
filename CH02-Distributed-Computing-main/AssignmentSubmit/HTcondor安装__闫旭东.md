# HTcondor安装



#### **准备4个虚拟机（均为Docker安装）**![image](https://user-images.githubusercontent.com/90243359/146742174-96ef9eb1-9e66-4faf-9d9b-d23c880d27ef.png)

#### 通过ifconfig查看4个主机的ip地址
![image](https://user-images.githubusercontent.com/90243359/146742229-24322f96-5fa7-4607-bb7b-9fcfa0a4aeec.png)



#### 充当Central Manager角色的虚拟机

curl -fsSL https://get.htcondor.org | GET_HTCONDOR_PASSWORD=wmcoder /bin/bash -s -- --no-dry-run --central-manager 172.17.0.2



#### 充当Submit 角色的虚拟机

curl -fsSL https://get.htcondor.org | GET_HTCONDOR_PASSWORD=wmcoder /bin/bash -s -- --no-dry-run --submit 172.17.0.2



#### 充当Execute角色的虚拟机（两台）

curl -fsSL https://get.htcondor.org | GET_HTCONDOR_PASSWORD=wmcoder /bin/bash -s -- --no-dry-run --execute 172.17.0.2



#### 安装完成

#### ![image](https://user-images.githubusercontent.com/90243359/146742310-ad6dd674-555c-4b3e-8e1b-34b528dcd8c6.png)

#### ![image](https://user-images.githubusercontent.com/90243359/146742359-555c80d6-fb3b-4026-9499-a84a41e2f5b7.png)
