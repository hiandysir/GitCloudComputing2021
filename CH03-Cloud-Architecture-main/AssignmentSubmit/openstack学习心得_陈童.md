# Openstack 安装心得

## 搭建环境与安装

* ### 配置apt下载源和pip下载源

  #### apt下载源

  修改/etc/apt/sources.list：

  ```
  cp /etc/apt/sources.list /etc/apt/sources.list.bak
  vim /etc/apt/sources.list
  ```

  在本次实验中使用的是清华的镜像源，修改完毕sources.list后需要更新下载源

  ```
  sudo apt-get update
  sudo apt-get upgrade
  ```

  #### pip下载源

  创建pip.conf

  ```
  mkdir ~/.pip
  vim ~/.pip/pip.conf
  ```

  在pip.conf中配置为清华的下载源

  ```
  [global]
  index-url = https://pypi.tuna.tsinghua.edu.cn/simple
  [install]
  trusted-host = https://pypi.tuna.tsinghua.edu.cn
  ```

* ### 创建stack用户

  devstack的安装与运行最好都要在一个新创建的stack用户下进行，不能通过root用户来安装。所以要先创建stack用户并赋予该用户sudo权限

  ```
  sudo useradd -s /bin/bash -d /opt/stack -m stack
  sudo passwd 123456 #编辑stack用户的登录密码
  echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
  ```

  这样就创建了一个拥有sudo权限的stack用户，接下来可以切换到stack用户

  ```
  su - stack
  ```

- ### 下载源码并修改配置文件

  通过下载源码，可以通过-b指定代码分支，这里我们指定的分支为stable/ocata，并且进入到devstack目录

  ```
  git clone https://opendev.org/openstack/devstack -b stable/ocata
  cd devstack
  ```

  接下来需要修改配置文件local.conf为如下内容

  ```
  vim local.conf
  ```

  ```
  [[local|localrc]]
  ADMIN_PASSWORD=选择一个密码
  DATABASE_PASSWORD=$ADMIN_PASSWORD
  RABBIT_PASSWORD=$ADMIN_PASSWORD
  SERVICE_PASSWORD=$ADMIN_PASSWORD
  HOST_IP=主机ip地址
  GIT_BASE=http://git.trystack.cn
  NOVNC_REPO=http://git.trystack.cn/kanaka/noVNC.git
  SPICE_REPO=http://git.trystack.cn/git/spice/spice-html5.git
  TEMPEST_REPO=http://git.trystack.cn/openstack/tempest
  LOGFILE=$DEST/logs/stack.sh.log
  LOGDAYS=2
  ```

  以上都配置完毕后，可以进行安装脚本

  ```
  ./stack.sh
  ```

  以上为devstack搭建openstack开发环境的全部过程。

## 过程中遇到的问题

- #### 系统提示git not found

  纯净的ubuntu系统中需要先执行sudo apt install git 才能正常使用git命令。

- #### 再次执行git clone，出现error: server certificate verification failed

  查询资料可知，是证书的校验未通过，执行命令修改证书校验后，可正常执行git clone

  ```
  git config --global http.sslverify false
  ```

- #### 运行./stack.sh，出现DevStack should be run as a user with sudo permissions, not root.

  发现此时的用户被自动切换到root，再将用户切换回之前创建好的stack用户，该错误解决。

  ```
  su - stack
  ```

- #### 运行./stack.sh，出现No such file or directory FORCE=yes ./stack.sh

  根据错误提示，可以看到需要在./stack.sh前加上FORCE=yes，该错误解决。

  ```
  FORCE=yes ./stack.sh
  ```

- #### 再次运行./stack.sh，出现ImportError: cannot import name 'spawn' E: Sub-process /usr/bin/dpkg returned an error code (1)

  经测试，发现是源的问题，执行update以及upgrade会有部分软件无法更新，于是将源更改为阿里云源后，能够正常update以及upgrade。

- #### 执行./stack.sh时出现download of get-pip.py failed

  此处卡了很久，因为网上没有太多相关的问题答复，并且能够找的大多无法解决该问题，从而使用apt-get命令在终端先下载pip，再到devstack/tools/install_pip.sh中把pip相关的下载相关命令注释掉，可以解决该问题。

- #### PermissionError：[Errno 13]权限被拒绝：'/opt/stack/.cache/pip/wheel

  #### s/a7/c1/ea/cf5bd31012e735

  经过一番搜索，我发现将堆栈用户与堆栈目录相关联可解决该错误

  ```
  sudo chown -R stack:stack /opt/stack
  ```

- #### Command "python setup.py egg_info" failed with error code 1 no module named setuptools_rust

  该问题可通过更新最新的pip可解决。

- #### 遇到cannot uninstall 'simplejson’错误

  这个是由于软件的依赖原因导致的，所以执行命令忽视掉相关依赖即可。

  ```
  sudo pip install simplejson --ignore-installed simplejson
  ```





