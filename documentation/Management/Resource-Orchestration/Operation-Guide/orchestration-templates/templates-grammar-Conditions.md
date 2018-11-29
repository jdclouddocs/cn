# 编排模板基本语法说明
## 条件（Conditions）

根据您在创建或更新堆栈时，指定的输入参数值进行计算。在每个条件中，都可以引用其他条件、参数值或映射。

当您需要重新使用可在不同环境（如测试环境与生产环境）中创建资源的模板时，可能会使用条件。在模板中，您可以添加 EnvironmentType 输入参数，它接受 prod 或 test 作为输入。对于生产环境，您可以包括带特定功能的 Amazon EC2 实例；但对于测试环境，您需要使用更少的功能来节约资金。使用条件，您可以定义对每个环境类型创建哪些资源以及如何配置它们。

### 语法

每个条件由条件名和条件本身对组成。其中条件名是字符串类型。条件是由 Fn::And、Fn::Or、Fn::Not、或Fn::Equals 定义。在本条件中还可以引用其他条件，多个条件用逗号隔开，每个条件名不能重复。

```
"Conditions" : {
  "Logical ID" : {Intrinsic function}
}

```

### 示例

以下示例如何关联 Conditions 和 Resources。本示例中，设置根据 EnvType 参数值决定是否给 vm 实例绑定 弹性ip 。

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
