## 键值检索
键值检索是将日志中的字段以键值对的形式建立索引，方便用户更加快捷精确的对日志进行检索。用户可以指定key，对value进行特定条件的检索。同时，根据用户和需求不同，京东云日志服务提供快捷检索和高级检索两种检索方式。
云产品系统日志自动建立键值索引，无需用户设置。

### 快捷检索

对日志所展示的字段不太熟悉或者仅需对个别检索条件进行检索时，推荐使用快捷检索。

1. 用户点击添加检索条件，最多支持添加5个检索条件，多个检索条件之间的关系为“and”。
2. 检索条件中的Key是日志中所有建立键值检索的KV对中的Key，云产品默认将日志中的所有KV建立索引。
3. 选择Key之后，根据需求选择比较符，输入Value的值，输入值得时候需要注意Key对应得value的类型。

![](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/LogSearch/quickSearch.png)

**使用场景**

如需对近一小时内字段“statuscode”大于400的日志进行检索。

1. 点击新增检索条件
2. 选择Key为statuscode，比较符选择“>”，Value输入400
3. 点击检索，显示近一小时内所有statuscode > 400的日志。

### 高级检索

对日志展示字段较为熟悉或者需要对多个KV进行组合查询时，推荐使用高级检索。

1. 用户按照规定的语法规则和业务的需求，输入检索条件，高级检索最多支持30个KV的组合关系。
2. 高级检索条件中的Key必须都是日志字段中真实存在的Key，如需了解云产品系统日志中的所有字段说明，请在云产品日志中查看。
3. 点击“检索”，展示符合检索条件日志数据。

![](https://raw.githubusercontent.com/luolei-laurel/cn-1/patch-1/image/LogService/LogSearch/advanceSearch.png)

**使用场景**

如需对近一小时内的符合status < 400 and payment = "jdpay"的日志进行检索。

1. 输入检索条件：status < 400 and payment = "jdpay"，注意如果是string类型的需要加上双引号，如果是数值类型的则不用加。
2. 点击检索，显示近一小时内所有满足status < 400 and payment = "jdpay"的日志。
