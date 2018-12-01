# 编排模板基本语法说明
## 映射（Mappings）

映射是一个 Key-Value 映射表。在模板的 Resources 和 Outputs 中，可以使用 Fn::FindInMap 内部函数，通过指定 Key 而获取映射表的 Value。

### 语法

映射由　Key-Value 对组成。其中 Key 和 Value 可以为字符串类型或者数字类型。如果声明多个映射，用逗号分隔开。每个映射的名称不能重复。

```
{
    "Mappings" : {
        "Mapping01" : {
            "Key01" : {
                "Name" : "Value01",
                "Name" : "Value01-1"
            },
            "Key02" : {
                "Name" : "Value02",
                "Name" : "Value02-1"
            },
            "Key03" : {
                "Name" : "Value03",
                "Name" : "Value03-1"
            }
    }
}

```

### 示例

```
{
    "JDCLOUDTemplateFormatVersion": "2018-10-01",
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
    "Resources": {
        "MyInstance": {
            "Type": "JDCLOUD::VM::Instance",
            "Properties": {
                "AZ": { "Fn::FindInMap": [ "AZInfo", { "Ref": "JDCLOUD::Region" }, "az1"]},
            }
        }
    }
}

```

