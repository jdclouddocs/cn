# 应用安全网关日志
## 应用场景 
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;通过日志服务收集应用安全网关的日志，对收集到的Web安全日志进行查询分析。
## 前提说明
- 已开通应用安全网关服务，并创建了实例
- 已创建了日志集和日志主题

## 配置流程
1.	进入日志集管理页面，选择已创建的日志集，点击“查看”按钮，进入日志详情页面，选择采集配置“未配置”状态的日志主题。  
![日志集详情](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/rzjxq-3.png)
2.  点击操作列下的 "管理"->"采集配置"，进入采集配置页面。  
![采集配置页](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/cjpz-1.png)
3.	点击 “添加采集配置”按钮，进入添加采集配置页面。  
![添加采集配置](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/tjcjpz.png)
3.	选择所属产品 “应用安全网关”、日志类型 “Web安全日志”，采集实例选择“全部”，点击确定按钮即可。

## 检索日志
1.	切换至“日志检索”页面，选中采集应用安全网关日志的日志集和日志主题，选择查询的时间段；  
![检索-0](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/js-0.png)
2.	切换至“键值索引”，使用快速检索，Key值进行如下配置， 
server_addr = 10.253.253.6  
server_protocol = https  
![检索-0](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/js-4.png)

3.  点击搜索按钮，即可查出所有server_addr为“10.253.253.6”，同时 server_protocol为“https”的日志记录信息。
