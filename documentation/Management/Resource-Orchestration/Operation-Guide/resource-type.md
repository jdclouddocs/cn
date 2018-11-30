# 资源类型

　　资源类型是京东云资源编排服务可以支持的资源类型的列表，包含云主机、云硬盘、私有网络、负载均衡、云数据库RDS等。 
 
　　打开控制台,选择管理-资源编排-资源类型,在该页面可以查看展示支持编排的资源类型列表。

## 支持的资源类型列表

 | 资源类型 | 描述 | 
 |---|---|
 |JDCLOUD::VM::Instance |创建云主机实例 |
 |JDCLOUD::VM::Disk | 创建云硬盘|
 |JDCLOUD::VM::AttachDisk| 将已有硬盘挂载到运行中的实例 | 
 |JDCLOUD::VPC::VPC| 创建VPC |
 |JDCLOUD::VPC::Subnet| 在VPC中创建子网 | 
 |JDCLOUD::VPC::EIP |
 |JDCLOUD::VPC::AssociateEIP| 负载均衡。本期支持云主机和负载均衡。
 |JDCLOUD::LB::LoadBalancer| 创建负载均衡 |
 |JDCLOUD::LB::Listener| 创建监听 |
 |JDCLOUD::LB::TargetGroup| 创建目标组 |
 |JDCLOUD::LB::RegisterTargets| 目标组中添加实例 |
 |JDCLOUD::LB::Backend | 创建后端服务 | 
 |JDCLOUD::RDS::DBInstance | 创建云数据库实例 |

　　表1 资源模板服务支持的资源类型 
  
## 查看资源类型详情

您可通过控制台，选择“管理-资源编排-资源类型”，进入资源类型列表页面，选择列表中某一个资源类型，点击操作列的“查看详情”按钮，查看资源类型详情。

- 返回值：指代该资源类型返回的相关信息；展示“名称”和“描述”信息。 

- 属性：指代在模板中定义该资源时，需要指定的属性信息；列表展示“名称”、“类型”、“是否必须”、“描述”、“限制”和“详情”。

### 返回值

　　资源类型详情页面为用户提供了查询资源类型返回的相关信息。 
  
　　如下图所示，以 JDCLOUD::VM::Disk 为例，它的返回值名称是Status，描述为云硬盘状态，取值为 creating、available、in-use、extending、restoring、deleting、deleted、error_create、error_delete、error_restore、error_extend 之一。
  
![返回值](https://raw.githubusercontent.com/jdclouddocs/cn/resource-orchestration/image/resource/resourcetype002.png)

### 属性

　　资源类型详情页面为用户提供了在资源编排模板中定义该资源时，需要指定的属性信息。
  
　　如下图所示，以 JDCLOUD::VM::Disk 为例，如AZ、Charge、Description等等。

![属性](https://raw.githubusercontent.com/jdclouddocs/cn/resource-orchestration/image/resource/resourcetype003.png)

