## 编排模板基本语法说明
### 资源（Resources）

资源（Resources） 用于描述资源栈中每个资源的属性和资源之间依赖关系。一个 Resources 可以被其他资源和 Outputs 所引用。

#### 语法

Resources 由资源逻辑 ID 和资源描述组成。请参见以下 Resources 的语法结构示例代码段：

```
"Resources" : {
    "resource1 LogicID" : {
        "Type" : "资源类型",
        "Condition": "是否创建此资源的条件"，
        "Properties" : {
            资源属性描述
        }
        "DeletionPolicy":"资源的删除策略",
        "DependsOn":["资源依赖列表"],
    },
    "resource2 LogicID" : {
        "Type" : "资源类型",
        "Condition": "是否创建此资源的条件"，
        "Properties" : {
            资源属性描述
        }
    }
}
```

#### 资源逻辑ID

逻辑 ID 必须为字母数字（a-z、 A-Z 和 0-9），并且在模板中具有唯一性。使用逻辑名称在模板的其他部分中引用资源。

#### 资源类型（Type）

资源类型表示正在声明的资源的类型。例如，JDCLOUD::VM::Instance 表示京东云vm实例。有关 JDRO 支持的所有资源类型列表和详细信息，请参见 [资源类型列表](#)。

#### 创建条件（Condition）
在模板中，使用 Condition 属性可以指定是否需要创建此资源。只有 Condition 所指定的条件值为 true 时，才会创建此资源。

如以下代码段所示，设置根据 EnvType 参数值决定是否给 vm 实例绑定 弹性ip 。

```
{
    "JDCLOUDTemplateFormatVersion": "2018-10-01",
    "Parameters": {
        "EnvType": {
            "Description": "Environment type.",
            "Default": "test",
            "Type": "String",
            "AllowedValues": [
                "prod",
                "test"
            ],
            "ConstraintDescription": "must specify prod or test."
        }
    },
    "Mappings": {
        "AZInfo": {
            "cn-north-1": {
                "az1": "cn-north-1a",
                "az2": "cn-north-1b"
            },
            "cn-east-1": {
                "az1": "cn-east-1a",
                "az2": "cn-east-1b"
            },
            "cn-east-2": {
                "az1": "cn-east-2a",
                "az2": "cn-east-2b"
            },
            "cn-south-1": {
                "az1": "cn-south-1a",
                "az2": "cn-south-1b"
            }
        }
    },
    "Conditions": {
        "CreateProdResources": {
            "Fn::Equals": [
                {
                    "Ref": "EnvType"
                },
                "prod"
            ]
        }
    },
    "Resources": {
        "MyInstance": {
            "Type": "JDCLOUD::VM::Instance",
            "Properties": {
                "AZ": {
                    "Fn::FindInMap": [
                        "AZInfo",
                        {
                            "Ref": "JDCLOUD::Region"
                        },
                        "az1"
                    ]
                }
            }
        },
        "MyElasticIp": {
            "Type": "JDCLOUD::VPC::ElasticIp",
            "Condition": "CreateProdResources",
            "Properties": {
                "ElasticIpSpec": {
                    "BandwidthMbps": "1",
                    "Provider": "bgp"
                }
            }
        },
        "MyAssociateElasticIp": {
            "Type": "JDCLOUD::VPC::AssociateElasticIp",
            "Condition": "CreateProdResources",
            "Properties": {
                "InstanceId": {
                    "Ref": "MyInstance"
                },
                "InstanceType": "vm",
                "ElasticIpId": {
                    "Ref": "MyElasticIp"
                }
            }
        }
    },
    "Outputs": {
        "ElasticIpAddress": {
            "Value": {
                "Fn: : GetAtt": [
                    "MyInstance",
                    "ElasticIpAddress"
                ]
            },
            "Condition": "CreateProdResources"
        }
    }
}
```


#### 资源属性（Properties)

资源属性是为资源指定的附加选项。例如，必须为每个京东云 vm 实例指定一个 Image ID。

```
"Resources" : {
  "MyInstance" : {
    "Type" : "JDCLOUD::VM::Instance",
    "Properties" : {
      "ImageId" : "img-wcewkxc5c1"
    }
  }
}
```
如果资源不需要声明属性，可忽略该资源的属性部分。

属性值可以是文本字符串、字符串列表、布尔值、引用参数、或者函数返回值。如果属性值为文本字符串，该值会被双引号 ("") 括起来。如果属性值为任一类型的字符串列表，则会被中括号 ([ ]) 括起来。如果值为内部函数或引用的参数，则会被大括号 ({ }) 括起来。当您将文字、列表、引用参数、和函数返回值合并起来取值时，上述规则适用。

例如：
```
"Properties" : {
    "String" : "one-string-value",
    "Number" : 123,
    "LiteralList" : [ "first-value", "second-value" ],
    "Boolean" : true,
    "ReferenceForOneValue" :  { "Ref" : "MyLogicalResourceName" } ,
    "FunctionResultWithFunctionParams" : {
        "Fn::Join" : [ "%", [ "Key=", { "Ref" : "MyParameter" } ] ] }
}
```

#### 删除策略（DeletionPolicy）

在模板中，设置 DeletionPolicy 属性，可以声明在资源栈被删除时保留某个资源。可选值为Delete（删除）、Retain（保留）。

例如，设置在资源栈删除时，保留 vm 实例。可按照如下代码段进行声明：

```
"Resources" : {
  "MyInstance" : {
    "Type" : "JDCLOUD::VM::Instance",
    "Properties" : {
      "ImageId" : "img-wcewkxc5c1"
    },
    "DeletionPolicy" : "Retain"
  }
}
```

#### 资源依赖（DependsOn）

在模板中，设置 DependsOn 属性，可以指定特定资源紧跟着另一个资源后创建。为某个资源添加 DependsOn 属性后，该资源仅在 DependsOn 属性中指定的资源之后创建。

如以下代码段所示，MyInstance 将在 MyDBInstance 创建成功后才开始创建：

```
{
    "JDCLOUDTemplateFormatVersion": "2018-10-01",
    "Resources": {
        "MyDBInstance": {
            "Type": "JDCLOUD::RDS::DBInstance",
            "Properties": {
            }
        },
        "MyInstance": {
            "Type": "JDCLOUD::VM::Instance",
            "DependsOn": "MyDBInstance"
            "Properties": {
            }
        },
    }
}
```


