## 基于Docker的HTCondor实践

### HTCondor镜像拉取

拉取命令：docker pull htcondor/mini:9.0.7-el7

这里我选择拉取minicondor进行简单演示

### Minicondor容器启动

启动命令：docker run -d htcondor/mini:9.0.7-el7

启动后容器会在后台持续运行，通过以下命令进入容器

进入命令：docker exec -ti Container /bin/bash

随后便可以进行作业提交

### 第一份作业提交与运行

作业的运行需要提交诸多详细信息，以提交睡眠作业为例



要提交作业，首先需要成为submituser用户，通过输入以下命令实现

成为submituser：su - submituser



编写睡眠作业sleep.sh：

```
#!/bin/bash
# file name: sleep.sh

TIMETOWAIT="6"
echo "sleeping for $TIMETOWAIT seconds"
/bin/sleep $TIMETOWAIT
```

随后通过chmod u+x sleep.sh将sleep.sh更改为可执行文件



编写作业描述文件sleep.sub：

```
# sleep.sub -- simple sleep job

executable              = sleep.sh
log                     = sleep.log
output                  = outfile.txt
error                   = errors.txt
should_transfer_files   = Yes
when_to_transfer_output = ON_EXIT
queue
```

executable指定要运行的作业；log指定日志文件；output指定输出文件......



通过以下命令提交作业

提交作业：condor_submit sleep.sub

提交成功后终端显示如下图

![submit sucess](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/wordSubmitmd2.png)



通过以下命令监控作业运行情况，

监控：condor_q

![condor_q](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/wordRunningmd2.png)



查看输出结果

![submit result](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/submitResultmd2.png)

![output](https://raw.githubusercontent.com/SHAOHUASONGgit/DataScienceRepo/main/CloudCmoputingMarkDown/picture/outPutmd2.png)

