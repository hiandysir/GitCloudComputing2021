# CH02

## 课内复习
1.	分布式计算的定义和特征是什么？
2.	什么是ACID原则？
3.	什么是CAP理论？
4.	什么是BASE理论？
5.	如何理解最终一致性？
6.	分布式存储与分布式计算的区别于联系是什么？

## 课外思考
1.	在我们的日常生活当中，为什么我们所接触到的分布式系统越来越多了？
2.	CAP定理中的几个关键因素为什么不能同时保证？不同的组合有什么样的应用场景？
3.	通过了解区块链的背景，说说你所理解的区块链做为一种分布式系统背后的全新理念。

## 动手实践
### 實踐 1 HTCondor
HTCondor是一个专门用于计算密集型作业的负载管理系统，它为用户提供了作业排队机制、调度策略、优先计划、资源监测和资源管理，诞生于美国Wisconsin大学Madison分校。

	任务：通过HTCondor的官方网站https://research.cs.wisc.edu/htcondor/，下载并安装使用HTCondor，了解它是如何实现大吞吐量计算过程的。



HTConda 入門簡介
內容來自互聯網 僅供參考

高通量计算框架HTCondor(一)——概述
https://www.cnblogs.com/charlee44/p/12204715.html

高通量计算框架HTCondor(二)——环境配置
https://www.cnblogs.com/charlee44/p/12207128.html

高通量计算框架HTCondor(三)——使用命令
https://www.cnblogs.com/charlee44/p/12231485.html

說明：原文有小許錯誤：（3.3 3.4部分）
Condor 誤輸入為 conodr


高通量计算框架HTCondor(四)——案例准备
https://www.cnblogs.com/charlee44/p/12232507.html

高通量计算框架HTCondor(五)——分布计算
https://www.cnblogs.com/charlee44/p/12233064.html

高通量计算框架HTCondor(六)——拾遗
https://www.cnblogs.com/charlee44/p/12233502.html


https://htcondor.readthedocs.io/en/latest/admin-manual/quick-start-condor-pool.html
https://www.youtube.com/watch?v=p2X6s_7e51k

軟件下載地址
https://research.cs.wisc.edu/htcondor/tarball/8.8/8.8.12/release/

### 實踐 2 JumpServer

JumpServer 是全球首款开源的堡垒机，使用 GNU GPL v2.0 开源协议，是符合 4A 规范的运维安全审计系统。
JumpServer 使用 Python 开发，遵循 Web 2.0 规范，配备了业界领先的 Web Terminal 方案，交互界面美观、用户体验好。
JumpServer 采纳分布式架构，支持多机房跨区域部署，支持横向扩展，无资产数量及并发限制。

    任务：通过 開源項目 https://github.com/jumpserver/jumpserver 部署 JumpServer 堡垒机, 並了解其工作過程。
體驗網站： https://demo.jumpserver.org/core/auth/login/

介紹視頻：https://www.bilibili.com/video/BV1ZV41127GB 

在Docker裡安裝JumperSever https://github.com/jumpserver/Dockerfile


## 參考文獻
第2章 分布式原理

7.	W. Vogels, Eventually consistent, Communications of the ACM, 2009, 52(1): 40-44.
8.	Antony Rowstron, Peter Druschel, Peer-to-Peer Systems, Communication of the ACM, 2010, 53(10): 72-82. 
9.	R Buyya, S Venugopal, A Gentle Introduction to Grid Computing and Technologies, Csi Communications, 2005 , 29 (1).
10.	D Thain, T Tannenbaum, M Livny, Distributed Computing in Practice The Condor Experience, Concurrency and Computation: Practice & Experience, 2005 , 17 (2-4) :323-356.
11.	JC Corbett,J Dean, M Epstein, et al., Spanner: Google’s Globally Distributed Database, ACM Transactions on Computer Systems, 2013, 31(3):8.
12.	Y Zhang, K Guo, J Ren, et al., Transparent Computing: A Promising Network Computing Paradigm, Computing in Science & Engineering , 2016 , 19 (1) :7-20.

