# JumpServer安装

\# 默认会安装到 /opt/jumpserver-installer-v2.16.1 目录 

```
curl -sSL  https://github.com/jumpserver/jumpserver/releases/download/v2.16.1/quick_start.sh | bash 

cd /opt/jumpserver-installer-v2.16.1
```

\# 安装完成后配置文件 /opt/jumpserver/config/config.txt

```
cd /opt/jumpserver-installer-v2.16.1
```

\# 启动 

```
./jmsctl.sh start
```

\# 停止 

```
./jmsctl.sh down
```

# HTCondor安装

#安装HTCondor存储库密钥

```
$ wget -qO - https://research.cs.wisc.edu/htcondor/ubuntu/HTCondor-Release.gpg.key | sudo apt-key add -
```

#安装HTCondor

```
$ sudo apt-get update
$ sudo apt-get install htcondor # or minihtcondor
```

#检查是否在运行

```
$ ps ax | grep condor
```

##注意，如果condor没有运行，这个ps命令的唯一输出将如下所示

```
$ ps auxww | grep condor
user    1466  0.0  0.0  14736  1048 pts/1    S+   13:05   0:00 grep condor
```

如果condor正在运行，将显示：

```
user    1433  0.0  0.0  14736  1092 pts/1    S+   13:05   0:00 grep condor
user    6404  0.0  0.0  72488 12956 ?        Ss    2020   0:55 condor_master -f
user   27159  0.0  0.0  27888  6084 ?        S     2020   1:11 condor_procd -A 
user   27201  0.0  0.0  60744 12620 ?        Ss    2020   4:04 condor_collector
user   27202  0.0  0.0  62016 14124 ?        Ss    2020   0:35 condor_schedd
user   27203  0.0  0.0  60784 12576 ?        Ss    2020   2:20 condor_negotiator
user   27204  0.0  0.3 103864 55784 ?        Ss    2020   3:14 condor_startd
```

