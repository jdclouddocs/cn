# 创建一个Node.js Web开发测试环境

通过此模板，您可以快速创建一个私有网络下的Node.js应用部署。该模版会自动创建一个私有网络和子网，然后创建一台弹性云主机用来下载安装Node.js环境。

该模版通过弹性云主机的userdata功能支持，安装和部署Node.js。

为了监测userdata的安装部署脚本执行结果，我们在模版中添加了两个虚拟资源， 类型分别为：“JDCLOUD::ResourceOrchestration::WaitCondition” 和 “JDCLOUD::ResourceOrchestration::WaitConditionHandle”。关于这两种资源类型的详细介绍，请参考JDRO资源类型介绍页面。

在弹性云主机的userdata执行脚本中，我们建议用户首先通过资源编排服务指定的对象存储地址，下载资源编排服务提供的发送消息脚本。在Node.js安装完成后，调用该发送消息脚本反馈给资源编排服务端，详见该实例模版。  
`注`：资源编排服发送消息脚本指定的下载地址为：  

- 华北-北京  
  Linux主机： jdro-userdata-cn-north-1.oss.cn-north-1.jcloudcs.com/signal.py  
  Windows主机: jdro-userdata-cn-north-1.oss.cn-north-1.jcloudcs.com/signal.exe
- 华南-广州    
  Linux主机： jdro-userdata-cn-south-1.oss.cn-south-1.jcloudcs.com/signal.py  
  Windows主机: jdro-userdata-cn-south-1.oss.cn-south-1.jcloudcs.com/signal.exe  
- 华东-上海   
  Linux主机： jdro-userdata-cn-east-2.oss.cn-east-2.jcloudcs.com/signal.py  
  Windows主机： jdro-userdata-cn-east-2.oss.cn-east-2.jcloudcs.com/signal.exe  
- 华东-宿迁  
  Linux主机： jdro-userdata-cn-east-1.oss.cn-east-1.jcloudcs.com/signal.py  
  Windows主机： jdro-userdata-cn-east-1.oss.cn-east-1.jcloudcs.com/signal.exe  

创建成功的资源栈，输出"hello world"

------

### 编排模板说明:

模板包含几个主要部分。Format Version 部分是必需部分。模板中的某些部分可以任何顺序显示。但是，在您构建模板时，使用以下列表的逻辑顺序可能会很有用，因为一个部分中的值可能会引用上一个部分中的值。列表简要概述了每个部分。

`Format Version`（必选）:JDRO支持的模板版本号，当前版本号：2018-10-01 

` Description`（可选）:一个描述模板的文本字符串，对模板进行详细描述。 

`Parameters`（可选）:定义创建资源栈时，模板用户可以定制化的参数。在运行时 (创建或更新堆栈时) 传递到模板。您可在模板的 Resources 和 Outputs 部分中引用定义的这些参数。使用参数可以增强模板的灵活性，提高复用性。 

`Mappings`（可选）:可用来指定条件参数值的密钥和关键值的映射，与查找表类似。您可通过使用 Resources 和 Outputs 部分中的 Fn::FindInMap 内部函数将键与相应的值匹配。 

` Conditions`（可选）:Conditions 使用 Fn::And、Fn::Or、Fn::Not、Fn::Equals 等定义条件。在创建或更新资源栈时，系统先计算模板中的所有条件，然后再创建资源。创建与 true 条件关联的所有资源，忽略与 false 条件关联的所有资源。 

` Resources`（可选）:用于详细定义使用该模板创建的资源栈所包含的资源，包括资源间的依赖关系、配置细节等。 

`Outputs`（可选）:用于输出一些资源属性等有用信息。可以通过 API 或控制台获取输出的内容。 

------

### 示例模板：

```
{
  "JDCLOUDTemplateFormatVersion": "2018-10-01",
  "Description": "JDRO Nodejs TEMPLATE",
  "Parameters": {
    "VPCName": {
      "Default": "vpc",
      "Type": "String",
      "MinLength": "1",
      "MaxLength": "32",
      "Description": "Define the VPC Name. It cannot be same as an existing VPC name, otherwise the resource will fail to be created",
      "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
      "ConstraintDescription": "Name only supports numbers, capital and lowercase letters, English underline and hyphen ."
    },
    "SubnetName": {
      "Default": "subnet",
      "Type": "String",
      "MinLength": "1",
      "MaxLength": "32",
      "Description": "Define the Subnet Name. It cannot be same as an existing Subnet name, otherwise the resource will fail to be created",
      "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
      "ConstraintDescription": "Name only supports numbers, capital and lowercase letters, English underline and hyphen ."
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
    },
    "InstanceName": {
      "Default": "vm",
      "Type": "String",
      "MinLength": "1",
      "MaxLength": "32",
      "Description": "The Instance Name",
      "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
      "ConstraintDescription": "Name only supports numbers, capital and lowercase letters, English underline and hyphen ."
    },
    "VMPassword": {
      "NoEcho": true,
      "Description": "Password for vm access",
      "Type": "String",
      "MinLength": "8",
      "MaxLength": "16",
      "AllowedPattern": "[a-zA-Z0-9]*"
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
        "image": "img-wbnwemkixm"
      },
      "cn-east-1": {
        "image": "img-nfrxl97pal"
      },
      "cn-east-2": {
        "image": "img-ssazsh60t6"
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
    },
    "MyInstance": {
      "Type": "JDCLOUD::VM::Instance",
      "Properties": {
        "Name": {
          "Ref": "InstanceName"
        },
        "ImageId": {
          "Fn::FindInMap": [
            "ImageInfo",
            {
              "Ref": "JDCLOUD::Region"
            },
            "image"
          ]
        },
        "InstanceType": "g.n2.medium",
        "AZ": {
          "Fn::FindInMap": [
            "AZInfo",
            {
              "Ref": "JDCLOUD::Region"
            },
            "az1"
          ]
        },
        "PrimaryNetworkInterface": {
          "NetworkInterface": {
            "SubnetId": {
              "Ref": "MySubnet"
            }
          }
        },
        "Password": {
          "Ref": "VMPassword"
        },
        "Userdata": {
          "Fn::Base64": {
            "Fn::Join": [
              "",
              [
                "#!/bin/bash \n",
                " Region=",
                {
                  "Ref": "JDCLOUD::Region"
                },
                "\n",
                " wget jdro-userdata-${Region}.s3.${Region}.jcloudcs.com/signal.py -O /tmp/signal.py  \n",
                " chmod +x /tmp/signal.py \n",
                " #user code begin \n",
                "cd /root \n",
                "curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash - \n",
                "yum -y install lsof nodejs \n",
                "cat << EOF >> example.js \n",
                "var http = require('http'); \n",
                "http.createServer(function (req, res) { \n",
                "  res.writeHead(200, {'Content-Type' : 'text/plain'}); \n",
                "  res.end('Hello World\\n'); \n",
                "}).listen(3000); \n",
                "EOF\n",
                "node example.js & \n",
                " # user code end \n",
                "/tmp/signal.py --exit-code $? ",
                {
                  "Ref": "MyWaitConditionHandle"
                },
                " \n "
              ]
            ]
          }
        }
      }
    },
    "MyElasticIp": {
      "Type": "JDCLOUD::VPC::ElasticIp",
      "Properties": {
        "ElasticIPSpec": {
          "BandwidthMbps": 1,
          "Provider": "bgp"
        }
      }
    },
    "MyAssociateElasticIp": {
      "Type": "JDCLOUD::VPC::AssociateElasticIp",
      "Properties": {
        "InstanceId": {
          "Ref": "MyInstance"
        },
        "InstanceType": "vm",
        "ElasticIpId": {
          "Ref": "MyElasticIp"
        }
      }
    },
    "MyWaitCondition": {
      "Type": "JDCLOUD::ResourceOrchestration::WaitCondition",
      "DependsOn": [
        "MyInstance"
      ],
      "Properties": {
        "Count": 1,
        "Handle": {
          "Ref": "MyWaitConditionHandle"
        },
        "Timeout": "3600"
      }
    },
    "MyWaitConditionHandle": {
      "Type": "JDCLOUD::ResourceOrchestration::WaitConditionHandle",
      "Properties": {}
    }
  },
  "Outputs": {
    "Server_Domain": {
      "Value": {
        "Fn::Join": [
          ":",
          [
            {
              "Fn::GetAtt": [
                "MyInstance",
                "ElasticIpAddress"
              ]
            },
            "3000"
          ]
        ]
      }
    }
  }
}

```
