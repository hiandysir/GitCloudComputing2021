# HTCondor搭建过程



## 一、简介

HTCondor是一个开源的高吞吐量计算软件框架，用于计算密集型任务的粗粒度分布式并行化。它可用于管理专用计算机群集上的工作负载，或将工作分配给空闲的台式计算机，即所谓的循环清理。HTCondor可在Linux，Unix，Mac OS X，FreeBSD和Microsoft Windows操作系统上运行。HTCondor可以将专用资源（机架式集群）和非专用台式机（循环清理）集成到一个计算环境中。高通量计算中的Throughput应该是吞吐量的意思，也就是调度计算机资源的能力。与高性能计算（HPC）不同，高通量计算（HTC）应对的问题是在高性能的同时能够长时间稳定运行的能力，并充分利用集群或网络内计算资源。长时间计算时，集群或网络内计算资源往往是不可靠的，这中间蕴含了计算资源管理和任务调度的问题。

具体来说，HTC的思想就是将规模的密集运算拆分成一个个的子任务，交给集群计算机运算。HTCondor提供了如下功能：

发布任务：根据设定的集群内计算资源条件，将任务发布到集群计算机。

调度任务：任务能够发送到满足条件计算机中运行，或者迁移到另外一台计算机。

监视任务：随时监视任务运行的情况和计算资源的情况。

注意拆分任务这一步还是需要用户自己控制的，拆分合适粒度的并行任务，有助于最大程度的负载均衡。



## 二、安装部署HTCondor

- ### 下载HTCondor repo仓库

```
yum install -y https://research.cs.wisc.edu/htcondor/repo/8.8/el7/release/htcondor-release-8.8-1.el7.noarch.rpm
```

- ### 通过rpm包方式安装 minicondor

```
yum install -y minicondor # or condor
```

- ### 启动condor

```
systemctl start  condor
systemctl enable condor
systemctl status condor
```

- ### 查看condor任务

```
condor_q # Should list out the jobs in the pool

-- Schedd: Centos-7.9 : <127.0.0.1:7120> @ 11/27/21 16:54:15
OWNER BATCH_NAME      SUBMITTED   DONE   RUN    IDLE   HOLD  TOTAL JOB_IDS

Total for query: 0 jobs; 0 completed, 0 removed, 0 idle, 0 running, 0 held, 0 suspended 
Total for all users: 0 jobs; 0 completed, 0 removed, 0 idle, 0 running, 0 held, 0 suspended

```

- ### 查看condor状态

```
condor_status

Name             OpSys      Arch   State     Activity LoadAv Mem   ActvtyTime

slot1@Centos-7.9 LINUX      X86_64 Unclaimed Idle      0.000  942  0+00:00:18
slot2@Centos-7.9 LINUX      X86_64 Unclaimed Idle      0.000  942  0+00:00:18
slot3@Centos-7.9 LINUX      X86_64 Unclaimed Idle      0.000  942  0+00:00:18
slot4@Centos-7.9 LINUX      X86_64 Unclaimed Idle      0.000  942  0+00:00:18

               Machines Owner Claimed Unclaimed Matched Preempting  Drain

  X86_64/LINUX        4     0       0         4       0          0      0

         Total        4     0       0         4       0          0      0

```



## 三、HTCondor运行测试

- ### 编写测试脚本

```
$ cat sleep.sh 
#!/bin/bash

file name: sleep.sh

TIMETOWAIT="6"
echo "sleeping for $TIMETOWAIT seconds"
/bin/sleep $TIMETOWAIT
```

- ### 编写sub提交文件

```
$ cat sleep.sub 
# sleep.sub -- simple sleep job

executable              = sleep.sh
log                     = sleep.log
output                  = outfile.txt
error                   = errors.txt
should_transfer_files   = Yes
when_to_transfer_output = ON_EXIT
queue
```

- ### 提交任务到HTCondor

```
condor_submit sleep.sub
```

- ### 查看任务状态

```
$ condor_q


-- Schedd: Centos-7.9 : <127.0.0.1:9618?... @ 11/27/21 17:04:26
OWNER BATCH_NAME    SUBMITTED   DONE   RUN    IDLE  TOTAL JOB_IDS
admin ID: 1       11/27 17:04      _      1      _      1 1.0

Total for query: 1 jobs; 0 completed, 0 removed, 0 idle, 1 running, 0 held, 0 suspended 
Total for admin: 1 jobs; 0 completed, 0 removed, 0 idle, 1 running, 0 held, 0 suspended 
Total for all users: 1 jobs; 0 completed, 0 removed, 0 idle, 1 running, 0 held, 0 suspended

```

- ### 查看任务结果

```
$ cat outfile.txt 
sleeping for 6 seconds

```

- ### 查看执行日志  

```
$ cat sleep.log 
000 (001.000.000) 11/27 17:04:17 Job submitted from host: <127.0.0.1:9618?addrs=127.0.0.1-9618&noUDP&sock=6290_fecb_4>
...
001 (001.000.000) 11/27 17:04:22 Job executing on host: <127.0.0.1:9618?addrs=127.0.0.1-9618&noUDP&sock=6290_fecb_5>
...
006 (001.000.000) 11/27 17:04:28 Image size of job updated: 284
	0  -  MemoryUsage of job (MB)
	0  -  ResidentSetSize of job (KB)
...
005 (001.000.000) 11/27 17:04:28 Job terminated.
	(1) Normal termination (return value 0)
		Usr 0 00:00:00, Sys 0 00:00:00  -  Run Remote Usage
		Usr 0 00:00:00, Sys 0 00:00:00  -  Run Local Usage
		Usr 0 00:00:00, Sys 0 00:00:00  -  Total Remote Usage
		Usr 0 00:00:00, Sys 0 00:00:00  -  Total Local Usage
	23  -  Run Bytes Sent By Job
	113  -  Run Bytes Received By Job
	23  -  Total Bytes Sent By Job
	113  -  Total Bytes Received By Job
	Partitionable Resources :    Usage  Request Allocated 
	   Cpus                 :     0.04        1         1 
	   Disk (KB)            :    13           1 267504737 
	   Memory (MB)          :     0           1       942 
...

```
