# 基础架构
智臻链BaaS平台主要由4个组件组成：

baas-backend：负责管理维护多个Kubernetes集群，将其他组件对集群的操作异步下发给具体集群进行处理，实时监控更新操作任务运行状态，以便用户查询。

baas-clustertool：负责处理用户通过界面配置的创建区块链网络的请求。当区块链网络创建完成后，与区块链网络中的agent实时通信，转发用户通过管理console对区块链网络的操作，并返回操作结果。

baas-jdcloud-adaptor：负责BaaS平台与JDCloud资源层交互，包括IaaS层资源创建，用户订单及计费等。

baas-monitor：负责监控及搜集用户区块链网络的运行状态，并定时上报给JDCloud监控服务。

![图片](https://github.com/jdclouddocs/cn/blob/BaaS-Platform/documentation/Block-Chain/Block-Chain-BaaS-Platform/Introduction/Pic/TIM%E6%88%AA%E5%9B%BE20190328185458.png)
