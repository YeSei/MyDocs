# 【2】Flink架构
![599415cbfeb774cc3d457be9d4a96d67](【2】Flink架构.resources/processes.png)
> Flink整个系统主要包括两个组件：**JobManager**和**TaskManager**。遵循Master-Slave结构设计原则。

## 2.1 Client客户端
> 客户端负责将任务提交到集群，与JobManager构建Akka连接，然后将任务提交到JobManager。可以通过CLI方式、Flink WebUI、RPC端口三种方式提交。

## 2.2 JobManager
> JobManager相当于整个集群的Master结点，且整个集群中有且只有一个活跃的JobManager。
> 负责将JobGraph转换为ExecutionGraph，任务调度。checkpoint等操作。

### 2.2.1 JobManager单点故障
> 任何时候都有唯一一个Master JobManager，和多个Standby JobManagers。在JobManager挂掉的情况下，就会有一个Standby JobManagers接管成为Master。
> Standby JobManagers和Master JobManager保持内容同步。

## 2.3 TaskManager
> 管理多个slot资源，负责具体的任务执行和对应任务在每个节点上的资源申请与管理。