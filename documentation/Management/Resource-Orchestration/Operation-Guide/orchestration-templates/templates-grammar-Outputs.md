# 编排模板基本语法说明

## 输出（Outputs）

在输出（Outputs）中，定义在调用查询资源栈接口时返回的值。例如，定义 vm 实例 privateIpAddress 的输出，然后可在调用查询资源栈的接口时，查看该实例的内网ip地址。

### 语法

Outputs 部分包含键名称 Outputs，后跟一个空格和一个冒号。您最多可在一个模板中声明 60 个输出。例如：

```
"Outputs" : {
  "output1 LogicID" : {
    "Description" : "输出的描述",
    "Condition": "是否输出此资源属性的条件",
    "Value" : "输出值的表达式"
  },
  "output2 LogicID" : {
    "Description" : "输出的描述",
    "Condition": "是否输出此资源属性的条件",
    "Value" : "输出值的表达式"
  }
}
```

### 输出逻辑 ID
当前输出的标识符。逻辑 ID 必须为字母数字（a-z、A-Z 和 0-9），并且在模板中具有唯一性。

### Description（可选）
用于描述输出值的 String 类型。

### Value（必需）
在调用查询资源栈接口时，返回的属性值。输出值可以包括文字值、参数引用、伪参数、映射值或内部函数。

### Condition（可选）
使用 Condition 属性可以指定是否需要创建某个资源和输出资源的信息。当 Condition 所指定的条件值为 true 时，才创建此资源和输出资源信息。

例如：
```
{
    "Outputs": {
        "ElasticIpAddress": {
            "Value": "ElasticIpAddress value",
            "Condition": "CreateProdResources"
        }
    }
}
```


