# 查看监控信息
在控制台上，您可以查看多项区块链数据服务指标，通过指标数据，您可以定位系统的问题所在，进行相应的系统优化。 

## 操作步骤
1. 登录[区块链数据服务控制台](https://bds-console.jdcloud.com/block/list)。 
2. 选择需要查看监控信息的目标实例，点击目标实例，进入实例详情页。
3. 选择 **监控** 标签，查看云数据库监控项，如下图所示。
    ![查看监控信息](Pic/查看监控信息.png)
4.  默认显示的是 1小时 维度的监控数据，同时您也可以选择 6小时，12小时，1天，3天，7天，14天 维度，如果需要看 30天 内的监控数据，可以通过选择日期范围来查看。 

## 监控项
|   监控项  |   说明    |   监控频率    |   监控周期    |
|   --- |   --- |   --- |   --  |
|   CPU 使用率  |   实例的 CPU 使用率   |   60 秒/次    |   30 天   |
|   内存使用率  |   实例的内存使用率    |   60 秒/次    |   30 天   |
|   硬盘使用量  |   实例的磁盘使用率    |   60 秒/次    |   30 天   |
|   硬盘使用率  |   实例的磁盘使用量    |   60 秒/次    |   30 天   |
|   网络出入速率  |   单位：kbps    |   60 秒/次    |   30 天   |
|   阻塞进程数  |   被阻塞的进程数  |   60 秒/次    |   30 天   |
|   每秒检查点写入Page数    |   每秒检查点写入Page数    |   60 秒/次    |   30 天   |
|   每秒登录次数    |   每秒登录次数    |   60 秒/次    |   30 天   |
|   每秒全表扫描次数    |   平均每秒全表扫描次数    |   60 秒/次    |   30 天   |
|   硬盘 IOPS  |   磁盘每秒 IO 读写次数  |   60 秒/次    |   30 天   |
|   每秒事务数  |   实例每秒钟事务处理数    |   60 秒/次    |   30 天   |
|   缓存命中率  |   缓存命中率  |   60 秒/次    |   30 天   |
|   当前总连接数    |   当前实例的总连接数  |   60 秒/次    |   30 天   |
|   阻塞进程数  |   被阻塞的进程数  |   60 秒/次    |   30 天   |
|   每秒 SQL 执行/编译次数   |   每秒 SQL 执行/编译次数   |   60 秒/次    |   30 天   |
|   磁盘队列长度    |   磁盘IO队列的长度    |   60 秒/次    |   30 天   |
|   锁异常统计    |   实例每秒锁超时，死锁，锁等待次数    |   60 秒/次    |   30 天   |
