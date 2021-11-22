# HTCondor的搭建和学习心得

author:Damon

HTCondor是威斯康星大学麦迪逊分校构建的分布式计算软件和相关技术，用来处理高通量计算（High Throughput Computing ）的相关问题。



安装部署参考官方文档：https://htcondor.readthedocs.io/en/latest/getting-htcondor/install-linux-as-root.html#

下面是源码：

```shell
#安装和更新curl
root@damon:~# apt-get update && apt-get install -y curl
#设置存储仓库
root@damon:~# wget -qO - https://research.cs.wisc.edu/htcondor/ubuntu/HTCondor-Release.gpg.key | sudo apt-key add -
OK
root@damon:~# # echo "deb http://research.cs.wisc.edu/htcondor/ubuntu/8.8/focal focus contrib" >> /etc/apt/sources.list
root@damon:~# # echo "deb-src http://research.cs.wisc.edu/htcondor/ubuntu/8.8/focal focus contrib" >> /etc/apt/sources.list
#安装htcondor
root@damon:~# sudo apt-get update
Get:1 http://mirrors.cloud.aliyuncs.com/ubuntu focal InRelease [265 kB]
Get:2 http://mirrors.cloud.aliyuncs.com/ubuntu focal-updates InRelease [114 kB]
Get:3 http://mirrors.cloud.aliyuncs.com/ubuntu focal-backports InRelease [101 kB]
...
...
...
root@damon:~#  sudo apt-get install htcondor # or minihtcondor
Reading package lists... Done
Building dependency tree       
Reading state information... Done
...
...
...
#运行htcondor
root@damon:~#  systemctl start condor
root@damon:~# systemctl enable condor
Synchronizing state of condor.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable condor
Created symlink /etc/systemd/system/multi-user.target.wants/condor.service → /lib/systemd/system/condor.service.


#If condor is running, you will see several lines of output, something like this:
root@damon:~# ps auxww | grep condor
condor     13337  0.0  0.6  21264 13420 ?        Ss   12:16   0:00 /usr/sbin/condor_master -f
root       13390  0.0  0.2  11104  5052 ?        S    12:16   0:00 condor_procd -A /var/run/condor/procd_pipe -L /var/log/condor/ProcLog -R 1000000 -S 60 -C 111
condor     13391  0.0  0.5  18392 10512 ?        Ss   12:16   0:00 condor_shared_port -f
condor     13392  0.0  0.6  19568 12532 ?        Ss   12:16   0:00 condor_collector -f
condor     13393  0.0  0.6  19244 13372 ?        Ss   12:16   0:00 condor_startd -f
condor     13394  0.0  0.6  20164 13372 ?        Ss   12:16   0:00 condor_schedd -f
condor     13395  0.0  0.6  19020 12560 ?        Ss   12:16   0:00 condor_negotiator -f
root       13517  0.0  0.0   9032   736 pts/0    S+   12:18   0:00 grep --color=auto condor


```

