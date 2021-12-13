# HTCondor安装心得



## 1. 简要介绍

HTCondor是一个开源的高吞吐量计算软件框架，用于计算密集型任务的粗粒度分布式并行化。它可用于管理专用计算机群集上的工作负载，或将工作分配给空闲的台式计算机，即所谓的循环清理。HTCondor可在Linux，Unix，Mac OS X，FreeBSD和Microsoft Windows操作系统上运行。HTCondor可以将专用资源（机架式集群）和非专用台式机（循环清理）集成到一个计算环境中。



## 2. 环境信息

| 项目     | 版本   | 下载地址                                                 |
| -------- | ------ | -------------------------------------------------------- |
| HTCondor | 8.9.2  | https://github.com/htcondor/htcondor/releases/tag/V8_9_2 |
| munge    | 0.5.13 | https://github.com/dun/munge/releases/tag/munge-0.5.13   |
|          |        |                                                          |



## 3. 部署Htcondor

### 3.1 下载安装包

1. 下载Htcondor安装包“htcondor-8_9_2.tar.gz”。

   下载地址：https://github.com/htcondor/htcondor/archive/V8_9_2.tar.gz

   

2. 下载munge安装包“munge-0.5.13.tar.xz”。

   下载地址：https://github.com/dun/munge/releases/tag/munge-0.5.13

   

3. 下载sqlite安装包“sqlite-autoconf-3340100.tar.gz”。

   下载地址：https://www.sqlite.org/2021/sqlite-autoconf-3340100.tar.gz





### 3.2 依赖库安装

1. 执行以下命令安装系统依赖包。

   **yum install bzip2-devel boost-devel-1.53.0-28.el7.aarch64 libuuid-devel.aarch64 libX11-devel.aarch64 -y**

   

2. 执行以下命令构建munge的rpm包。

   **rpmbuild -tb --clean munge-0.5.13.tar.xz**

   

3. 执行以下命令检查是否成功生成rpm包。

   **ls /root/rpmbuild/RPMS/aarch64/ | grep munge**

   **munge-0.5.13-1.el7.aarch64.rpm**

   **munge-debuginfo-0.5.13-1.el7.aarch64.rpm**

   **munge-devel-0.5.13-1.el7.aarch64.rpm**

   **munge-libs-0.5.13-1.el7.aarch64.rpm**

   

4. 执行以下命令安装munge的rpm包。

   yum install -y munge-*







### 3.3 编译HTCondor

1. 执行以下命令解压安装包。

   ```
   cd /path/to/HTCONDOR
   tar -xvf htcondor-8_9_2.tar.gz
   ```

   

2. 执行以下命令进入“condor-8.9.2”目录。

   ```
   cd htcondor-8_9_2
   ```

   

3. 执行以下命令编辑HTCondor的config文件。

   ```
   cp configure_redhat buildarm.sh
   ```

   1. vi buildarm.sh

   2. 按“i”进入编辑模式，修改如下内容。

      ```
      #!/bin/sh 
        
      echo "-------------------------------------------------------------------" 
      echo "* NOTE: Attempting to configure a Red Hat-esk build" 
      echo "* which builds against system libs and selectively " 
      echo "* enables and disables portions of condor" 
      echo "* If you are unsure, you should run \"cmake .\"" 
      echo "*" 
      echo "* add -D_DEBUG:BOOL=FALSE to get non-optimized code for debugging" 
      echo "* Another option would be to run ccmake or cmake-gui" 
      echo "* and select the options you care to build with" 
      echo "-------------------------------------------------------------------" 
      cmake  \ 
        -D_DEBUG:BOOL=TRUE \
        -DWITH_CREAM:BOOL=FALSE \
        -DNO_PHONE_HOME:BOOL=TRUE \
        -DHAVE_BACKFILL:BOOL=FALSE \
        -DHAVE_BOINC:BOOL=FALSE \
        -DHAVE_KBDD:BOOL=TRUE \
        -DHAVE_HIBERNATION:BOOL=TRUE \
        -DWANT_CONTRIB:BOOL=ON \
        -DWANT_MAN_PAGES:BOOL=TRUE \
        -DWANT_FULL_DEPLOYMENT:BOOL=FALSE \
        -DWANT_GLEXEC:BOOL=FALSE \
        -D_VERBOSE:BOOL=TRUE \
        -DBUILDID:STRING=RH_development \
        -DWITH_GLOBUS:BOOL=FALSE \
        -DWITH_VOMS:BOOL=FALSE \
        -DSQLITE3_LIB:FILEPATH=/path/to/SQLITE/lib/libsqlite3.so \
        -DHAVE_SQLITE3_H:FILEPATH=/path/to/SQLITE/include \
        -DCMAKE_INSTALL_PREFIX:PATH=${PWD}/release_dir "$@"
      ```

      

   3. 按“Esc”键，输入**:wq!**，按“Enter”保存并退出编辑。

   

4. 执行以下命令进行配置。

   

   **./buildarm.sh**

   ```
   ……
   -- Configuring done
   -- Generating done
   -- Build files have been written to: /home/htcondor/condor-8.9.2
   ```

   

   

5. 执行以下命令进行编译安装。

   ```
   make -j 64
   make install
   ```

   



### 3.4 配置HTCondor

1. 执行以下命令进入“release_dir”目录。

   **cd** */path/to/**HTCONDOR***/htcondor-8_9_2/release_dir**

   

2. 执行以下命令创建“condor.sh”文件。

   1. **vi condor.sh**

   2. 按“i”进入编辑模式，添加如下内容。

      ```
      export CONDOR_CONFIG=/path/to/HTCONDOR/htcondor-8_9_2/release_dir/etc/condor_config
      export PATH=/path/to/HTCONDOR/htcondor-8_9_2/release_dir/bin:/path/to/HTCONDOR/condor-8.9.2/release_dir/sbin:$PATH
      ```

   3. 按“Esc”键，输入**:wq!**，按“Enter”保存并退出编辑。

   4. 执行以下命令使文件生效。

      **source** **condor.sh**

   

3. 执行以下命令进入“release_dir/etc”目录。

   ```
   cd /path/to/HTCONDOR/htcondor-8_9_2/release_dir/etc
   ```

   

4. 执行以下命令创建“condor_config”配置文件。

   1. **vi condor_config**

   2. 按“i”进入编辑模式，修改如下内容。

      ```
      CONDOR_HOST             = 192.168.47.111
      RELEASE_DIR             = /path/to/HTCONDOR/htcondor-8_9_2/release_dir
      LOCAL_DIR               = /data/
      LOCAL_CONFIG_DIR        = $(LOCAL_DIR)/config
      LOCAL_CONFIG_FILE       = $(LOCAL_DIR)/condor_config.local
      CONDOR_ADMIN            = root@192.168.47.111
      MAIL                    = /usr/bin/mail
      ALLOW_ADMINISTRATOR     = $(CONDOR_HOST)
      ALLOW_NEGOTITATOR       = $(CONDOR_HOST)
      LOCK                    = $(LOG)
      CONDOR_IDS              = 2001.2001
      
      use SECURITY : HOST_BASED
      
      LOG                     = $(LOCAL_DIR)/log
      SPOOL                   = $(LOCAL_DIR)/spool
      BIN                     = $(RELEASE_DIR)/bin
      LIB                     = $(RELEASE_DIR)/lib
      SBIN                    = $(RELEASE_DIR)/sbin
      LIBEXEC                 = $(RELEASE_DIR)/libexec
      HISTORY                 = $(RELEASE_DIR)/history
      
      
      MASTER_LOG              = $(LOG)/MasterLog
      SCHEDD_LOG              = $(LOG)/SchedLog
      SHADOW_LOG              = $(LOG)/ShadowLog
      
      SHADOW_LOCK             = $(LOCK)/ShadowLock
      
      DAEMON_LIST = COLLECTOR MASTER NEGOTIATOR SCHEDD STARTD
      CONDOR_HOST = $(CONDOR_HOST)
      USE_CLONE_TO_CREATE_PROCESSES = False
      ```

      

   3. 按“Esc”键，输入**:wq!**，按“Enter”保存并退出编辑。

   

5. 执行以下命令创建一个condor用户和组。

   ```
   groupadd -g 2001 condor
   useradd -u 2001 -g 2001 condor
   ```

   

6. 执行以下命令创建HTCondor所需的目录和文件。

   

   ```
   mkdir -p /data
   cd /data
   mkdir -p config examples execute log spool
   touch condor_config.local
   touch log/MasterLog log/SchedLog log/ShadowLog log/ShadowLock
   chown -R condor.condor ./
   ```

   

7. 执行以下命令配置condor的init.d服务。

   ```
   cp /path/to/HTCONDOR/htcondor-8_9_2/release_dir/etc/init.d/condor /etc/init.d/ -f
   ```

   1. **vi /etc/init.d/condor**

   2. 按“i”进入编辑模式，修改如下内容。

      ```
      ……
      # Path to your primary condor configuration file.
      CONDOR_CONFIG="/path/to/HTCONDOR/htcondor-8_9_2/release_dir/etc/condor_config"
      
      # Path to condor_config_val
      CONDOR_CONFIG_VAL="/path/to/HTCONDOR/htcondor-8_9_2/release_dir/bin/condor_config_val"
      ……
      ```

      

   3. 按“Esc”键，输入**:wq!**，按“Enter”保存并退出编辑。



#### 3.5 启动HTCondor

1. 执行以下命令启动HTCondor。

   ```
   /etc/init.d/condor start
   ```

   