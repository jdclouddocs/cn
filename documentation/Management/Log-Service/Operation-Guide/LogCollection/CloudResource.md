## 云产品日志采集

&#160;&#160;&#160;&#160;本期仅支持云产品类的日志采集。云产品类的日志，无需安装插件，需指定所属云产品、日志类型，配置需采集的实例，接口完成日志的采集。  

### 前提条件  
- 待采集日志的云产品已创建了实例。  
- 已创建了日志集和日志主题。

### 操作步骤
1.	进入日志集管理页面，选择已创建的日志集，点击“查看”按钮，进入日志详情页面，选择采集配置“未配置”状态日志主题，点击“采集配置”按钮，进入采集配置页面。

![](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/LogCollection/noConfig.png)

2.	点击“添加采集配置”按钮，进入添加采集配置页面

![](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/LogCollection/addConfig.png)

3.	选择所属产品 (例如：应用安全网关)、日志类型 (例如：Web安全日志)，采集实例“全部实例”或 “选择实例”，点击“保存”即完成采集配置的设置。  
- 全部实例：是用户该产品的全部实例都进行采集，包括后续新增的实例都将自动采集。  
- 选择实例：选择部分实例，可将你关注的实例进行采集，最多可添加20台实例。

![](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/LogCollection/configuration.png)
