## 采集配置管理
### 查看采集实例
**前提说明**
- 日志主题下已添加采集配置

**操作步骤**
1.	登录京东云控制台。
2.	点击左侧导航栏选择“管理->日志服务->日志集管理”，进入日志集管理界面。
3.	选中待查看的日志主题的，点击日志主题名称或管理按钮下的“采集配置”按钮，进入该日志主题的采集配置页面，即可查看到该日志主题配置的采集实例信息。
4.	若当前日志主题配置的采集全部实例，页面展示如下：

![](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/LogSetManagement/collectAll.png)

5.	若当前日志主题仅采集部分实例，页面展示如下：

![](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/LogSetManagement/addCollectionConfig.png)

### 更改采集实例
**更改为选择部分实例**  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;日志主题配置为采集全部实例时，支持更改为选择部分实例。
1.	在采集配置页面，点击“更改”按钮，进入更改采集实例页面； 
2.	切换至“添加部分”实例，选择需要添加的实例，点击“确定”按钮即可。

![](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/LogSetManagement/editColletcionConfig.png)

**部分实例管理**  
&#160;&#160;&#160;&#160;&#160;&#160;&#160;&#160;若当前配置采集的部分实例，支持移出已添加的实例和添加新的实例。
1.	在实例列表页面，选中要移出的实例，点击移除按钮，或选中多条实例，点击批量“移出”按钮即可。
2.	点击“添加采集实例”按钮，进入添加部分实例页面。支持添加新的采集实例，同时也支持更改为采集“全部实例”。
