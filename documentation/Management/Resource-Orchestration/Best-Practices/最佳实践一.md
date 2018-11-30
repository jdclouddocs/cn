# 创建一个私有网络和子网

通过此模板，您可以快速创建部署云主机所需的网络，包括了VPC和子网络的设计与创建。

创建成功的资源栈，输出私有网络ID，子网ID和子网的CIDR。

----------
## 编排模板说明:

模板包含几个主要部分。Resources 和 Format Version 部分是必需部分。模板中的某些部分可以任何顺序显示。但是，在您构建模板时，使用以下列表的逻辑顺序可能会很有用，因为一个部分中的值可能会引用上一个部分中的值。列表简要概述了每个部分。

`Format Version`（必选）:JDRO支持的模板版本号，当前版本号：2018-10-01 

` Description`（可选）:一个描述模板的文本字符串，对模板进行详细描述。 

`Parameters`（可选）:定义创建资源栈时，模板用户可以定制化的参数。在运行时 (创建或更新堆栈时) 传递到模板。您可在模板的 Resources 和 Outputs 部分中引用定义的这些参数。使用参数可以增强模板的灵活性，提高复用性。 

`Mappings`（可选）:可用来指定条件参数值的密钥和关键值的映射，与查找表类似。您可通过使用 Resources 和 Outputs 部分中的 Fn::FindInMap 内部函数将键与相应的值匹配。 

` Conditions`（可选）:Conditions 使用 Fn::And、Fn::Or、Fn::Not、Fn::Equals 等定义条件。在创建或更新资源栈时，系统先计算模板中的所有条件，然后再创建资源。创建与 true 条件关联的所有资源，忽略与 false 条件关联的所有资源。 

` Resources`（可选）:用于详细定义使用该模板创建的资源栈所包含的资源，包括资源间的依赖关系、配置细节等。 

`Outputs`（可选）:用于输出一些资源属性等有用信息。可以通过 API 或控制台获取输出的内容。 


-----------
## 示例模板：
```  
{
    "JDCLOUDTemplateFormatVersion": "2018-10-01",
    "Description": "JDRO VPC_SUBNET TEMPLATE",
    "Parameters": {
        "VPCName": {
            "Default": "vpc",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "32",
            "Description": "The VPC Name",
            "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
            "ConstraintDescription": "Name only supports numbers, capital and lowercase letters, English underline and hyphen ."
        },
        "SubnetName": {
            "Default": "subnet",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "32",
            "Description": "My Subnet Name",
            "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
            "ConstraintDescription": "Name only supports  numbers, capital and lowercase letters, English underline and hyphen ."
        },
        "AddressPrefix": {
            "Default": "10.0.0.0/16",
            "Type": "String",
            "Description": "Give an CIDR",
            "AllowedValues": [
                "192.168.0.0/16",
                "172.16.0.0/16",
                "10.0.0.0/16"
            ],
            "ConstraintDescription": "Need give an exact CIDR."
        }
    },
    "Mappings": {
        "AZInfo": {
            "cn-north-1": {
                "az1": "cn-north-1a",
                "az2": "cn-north-1b"
            },
            "cn-east-1": {
                "az1": "cn-east-1a"
            },
            "cn-east-2": {
                "az1": "cn-east-2a",
                "az2": "cn-east-2b"
            },
            "cn-south-1": {
                "az1": "cn-south-1a"
            }
        },
        "ImageInfo": {
            "cn-north-1": {
                "image": "img-2qz094wxaz"
            },
            "cn-east-1": {
                "image": "img-nfrxl97pal"
            },
            "cn-east-2": {
                "image": "img-wcewkxc5c1"
            },
            "cn-south-1": {
                "image": "img-xkjedl0lgm"
            }
        }
    },
    "Resources": {
        "MyVPC": {
            "Type": "JDCLOUD::VPC::VPC",
            "Properties": {
                "VpcName": {
                    "Ref": "VPCName"
                }
            }
        },
        "MySubnet": {
            "Type": "JDCLOUD::VPC::Subnet",
            "Properties": {
                "VpcId": {
                    "Ref": "MyVPC"
                },
                "AddressPrefix": {
                    "Ref": "AddressPrefix"
                },
                "SubnetName": {
                    "Ref": "SubnetName"
                }
            }
        }
    },
    "Outputs": {
        "VPC_ID": {
            "Value": {
                "Ref": "MyVPC"
            }
        },
        "Subnet_ID": {
            "Value": {
                "Ref": "MySubnet"
            }
        },
        "Subnet_AddressPrefix": {
            "Value": {
                "Fn::GetAtt": [
                    "MySubnet",
                    "AddressPrefix"
                ]
            }
        }
    }
}
```
