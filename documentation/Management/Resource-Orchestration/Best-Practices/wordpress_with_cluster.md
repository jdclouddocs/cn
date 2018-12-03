# 创建一个集群模式的WordPress应用

通过此模板，您可以快速创建一个私有网络下双机版本的WordPress应用样例。该模版会自动创建一个私有网络和子网，云数据库（RDS）MySQL5.7，两台弹性云主机部署WordPress服务，以及一个弹性负载均衡提供外部访问。

该模版通过弹性云主机的userdata功能支持，将WordPress安装部署脚本同步下发到弹性云主机，在主机创建完成后，自动去下载安装WordPress，并且启动WordPress服务。具体使用userdata过程请参考上面创建单机WordPress应用中的介绍。

创建成功的资源栈，输出弹性负载均衡公网IP。

`注意`：目前资源编排服务不支持高可用组的创建。在创建多个弹性云主机时，需要在模版中定义多个云主机实例资源。

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
    "Description": "JDRO WORDPRESS_WITH_CLUSTER TEMPLATE",
    "Parameters": {
        "VPCName": {
            "Default": "vpc",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "32",
            "Description": "My VPC Name",
            "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
            "ConstraintDescription": "Name only supports numbers, capital and lowercase letters, English underline and hyphen."
        },
        "SubnetName": {
            "Default": "subnet",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "32",
            "Description": "The Subnet Name",
            "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
            "ConstraintDescription": "Name only supports numbers, capital and lowercase letters, English underline and hyphen."
        },
        "InstanceName1": {
            "Default": "vm1",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "32",
            "Description": "Define the Instance Name",
            "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
            "ConstraintDescription": "Name only supports numbers, capital and lowercase letters, English underline  and hyphen."
        },
        "InstanceName2": {
            "Default": "vm2",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "32",
            "Description": "Define the Instance Name",
            "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
            "ConstraintDescription": "Name only supports numbers, capital and lowercase letters, English underline  and hyphen."
        },
        "DiskName1": {
            "Default": "disk1",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "32",
            "Description": "The Disk Name",
            "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
            "ConstraintDescription": "Name only supports numbers, capital and lowercase letters, English underline  and hyphen."
        },
        "DiskName2": {
            "Default": "disk2",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "32",
            "Description": "The Disk Name",
            "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
            "ConstraintDescription": "Name only supports  numbers, capital and lowercase letters, English underline  and hyphen."
        },
        "DBName": {
            "Default": "db",
            "Description": "MySQL database name",
            "Type": "String",
            "MinLength": "2",
            "MaxLength": "32",
            "AllowedPattern": "^[a-z][a-z0-9]*$",
            "ConstraintDescription": "The name only supports figures letters both in upper case and lower case and English underline, no less than 2 characters and no more than 32 characters."
        },
        "DBUser": {
            "Default": "root",
            "Description": "Username for MySQL database access",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "16",
            "AllowedPattern": "^[a-zA-Z][a-zA-Z0-9]*$",
            "ConstraintDescription": "must begin with a letter and contain only alphanumeric characters."
        },
        "AddressPrefix": {
            "Default": "10.0.0.0/16",
            "Type": "String",
            "Description": "Give an exact CIDR",
            "AllowedValues": [
                "192.168.0.0/16",
                "172.16.0.0/16",
                "10.0.0.0/16"
            ],
            "ConstraintDescription": "Need give an exact CIDR."
        },
        "Password": {
            "Default": "Ptest1130",
            "NoEcho": true,
            "Description": "Password for vm or dbuser access",
            "Type": "String",
            "MinLength": "8",
            "MaxLength": "16",
            "AllowedPattern": "[a-zA-Z0-9]*"
        },
        "LoadBalancerName": {
            "Default": "lb",
            "Description": "LoadBalancer name",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "32",
            "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
            "ConstraintDescription": "Name only supports  numbers, capital and lowercase letters, English underline  and hyphen."
        },
        "TargetGroupName": {
            "Default": "lbtargetgroup",
            "Description": "TargetGroup Name",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "32",
            "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
            "ConstraintDescription": "Name only supports  numbers, capital and lowercase letters, English underline  and hyphen."
        },
        "LBBackendName": {
            "Default": "lbbackend",
            "Description": "Backend Name",
            "Type": "String",
            "MinLength": "1",
            "MaxLength": "32",
            "AllowedPattern": "^[a-zA-Z_][a-zA-Z0-9_-]*$",
            "ConstraintDescription": "Name only supports  numbers, capital and lowercase letters, English underline  and hyphen."
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
        },
        "MyInstance1": {
            "Type": "JDCLOUD::VM::Instance",
            "Properties": {
                "Name": {
                    "Ref": "InstanceName1"
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
                    "Ref": "Password"
                },
                "DataDisks": [
                    {
                        "AutoDelete": true,
                        "CloudDiskSpec": {
                            "Name": {
                                "Ref": "DiskName1"
                            },
                            "AZ": {
                                "Fn::FindInMap": [
                                    "AZInfo",
                                    {
                                        "Ref": "JDCLOUD::Region"
                                    },
                                    "az1"
                                ]
                            },
                            "DiskSizeGB": 20,
                            "DiskType": "premium-hdd"
                        }
                    }
                ],
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
                                " wget jdro-userdata-1031.s3.${Region}-stag.jcloudcs.com/jdro-signal.py -O /tmp/jdro-signal.py \n",
                                " chmod +x /tmp/jdro-signal.py \n",
                                " #user code begin \n",
                                "DatabaseUser=",
                                {
                                    "Ref": "DBUser"
                                },
                                "\n",
                                "DatabasePwd=",
                                {
                                    "Ref": "Password"
                                },
                                "\n",
                                "DatabaseName=",
                                {
                                    "Ref": "DBName"
                                },
                                "\n",
                                "DatabaseHost=",
                                {
                                    "Fn::GetAtt": [
                                        "MyDBInstance",
                                        "InternalDomainName"
                                    ]
                                },
                                "\n",
                                "WebRootPath='/var/www/html'\n",
                                "yum install -y curl httpd mysql-server php php-common php-mysql\n",
                                "yum install -y php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc\n",
                                "chkconfig httpd on\n",
                                "wget http://wordpress.org/latest.tar.gz \n",
                                "tar -xzvf latest.tar.gz \n",
                                "sed -i \"s/database_name_here/$DatabaseName/\" wordpress/wp-config-sample.php\n",
                                "sed -i \"s/username_here/$DatabaseUser/\" wordpress/wp-config-sample.php\n",
                                "sed -i \"s/password_here/${DatabasePwd:-$DatabasePwdDef}/\" wordpress/wp-config-sample.php\n",
                                "sed -i \"s/localhost/$DatabaseHost/\" wordpress/wp-config-sample.php\n",
                                "sed -i \"s/bpache/apache/\" wordpress/wp-config-sample.php\n",
                                "mv wordpress/wp-config-sample.php wordpress/wp-config.php\n",
                                "cp -a wordpress/* $WebRootPath \n",
                                "rm -rf wordpress*\n",
                                "service httpd stop\n",
                                "usermod -d $WebRootPath apache &>/dev/null\n",
                                "chown apache:apache -R $WebRootPath\n",
                                "service httpd start\n",
                                " # user code end \n",
                                "/tmp/jdro-signal --exit-code $? ",
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
        "MyInstance2": {
            "Type": "JDCLOUD::VM::Instance",
            "Properties": {
                "Name": {
                    "Ref": "InstanceName2"
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
                    "Ref": "Password"
                },
                "DataDisks": [
                    {
                        "AutoDelete": true,
                        "CloudDiskSpec": {
                            "Name": {
                                "Ref": "DiskName2"
                            },
                            "AZ": {
                                "Fn::FindInMap": [
                                    "AZInfo",
                                    {
                                        "Ref": "JDCLOUD::Region"
                                    },
                                    "az1"
                                ]
                            },
                            "DiskSizeGB": 20,
                            "DiskType": "premium-hdd"
                        },
                        "DiskCategory": "cloud"
                    }
                ],
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
                                " wget jdro-userdata-1031.s3.${Region}-stag.jcloudcs.com/jdro-signal.py -O /tmp/jdro-signal.py \n",
                                " chmod +x /tmp/jdro-signal.py \n",
                                " #user code begin \n",
                                "DatabaseUser=",
                                {
                                    "Ref": "DBUser"
                                },
                                "\n",
                                "DatabasePwd=",
                                {
                                    "Ref": "Password"
                                },
                                "\n",
                                "DatabaseName=",
                                {
                                    "Ref": "DBName"
                                },
                                "\n",
                                "DatabaseHost=",
                                {
                                    "Fn::GetAtt": [
                                        "MyDBInstance",
                                        "InternalDomainName"
                                    ]
                                },
                                "\n",
                                "WebRootPath='/var/www/html'\n",
                                "yum install -y curl httpd mysql-server php php-common php-mysql\n",
                                "yum install -y php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc\n",
                                "chkconfig httpd on\n",
                                "wget http://wordpress.org/latest.tar.gz \n",
                                "tar -xzvf latest.tar.gz \n",
                                "sed -i \"s/database_name_here/$DatabaseName/\" wordpress/wp-config-sample.php\n",
                                "sed -i \"s/username_here/$DatabaseUser/\" wordpress/wp-config-sample.php\n",
                                "sed -i \"s/password_here/${DatabasePwd:-$DatabasePwdDef}/\" wordpress/wp-config-sample.php\n",
                                "sed -i \"s/localhost/$DatabaseHost/\" wordpress/wp-config-sample.php\n",
                                "sed -i \"s/bpache/apache/\" wordpress/wp-config-sample.php\n",
                                "mv wordpress/wp-config-sample.php wordpress/wp-config.php\n",
                                "cp -a wordpress/* $WebRootPath \n",
                                "rm -rf wordpress*\n",
                                "service httpd stop\n",
                                "usermod -d $WebRootPath apache &>/dev/null\n",
                                "chown apache:apache -R $WebRootPath\n",
                                "service httpd start\n",
                                " # user code end \n",
                                "/tmp/jdro-signal --exit-code $? ",
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
        "MyDBInstance": {
            "Type": "JDCLOUD::RDS::DBInstance",
            "Properties": {
                "Engine": "MySQL",
                "AZId": [
                    {
                        "Fn::FindInMap": [
                            "AZInfo",
                            {
                                "Ref": "JDCLOUD::Region"
                            },
                            "az1"
                        ]
                    }
                ],
                "ChargeSpec": {
                    "ChargeMode": "postpaid_by_duration"
                },
                "EngineVersion": "5.7",
                "InstanceClass": "db.mysql.s1.micro",
                "InstanceName": {
                    "Ref": "DBName"
                },
                "InstanceStorageGB": 20,
                "VpcId": {
                    "Ref": "MyVPC"
                },
                "SubnetId": {
                    "Ref": "MySubnet"
                }
            }
        },
        "MyLoadBalancer": {
            "Type": "JDCLOUD::LoadBalance::LoadBalancer",
            "Properties": {
                "LoadBalancerName": {
                    "Ref": "LoadBalancerName"
                },
                "SubnetId": {
                    "Ref": "MySubnet"
                },
                "Azs": [
                    {
                        "Fn::FindInMap": [
                            "AZInfo",
                            {
                                "Ref": "JDCLOUD::Region"
                            },
                            "az1"
                        ]
                    }
                ]
            }
        },
        "MyElasticIp": {
            "Type": "JDCLOUD::VPC::ElasticIp",
            "Properties": {
                "AutoDelete": true,
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
                    "Ref": "MyLoadBalancer"
                },
                "InstanceType": "lb",
                "ElasticIpId": {
                    "Ref": "MyElasticIp"
                }
            }
        },
        "MyLoadBalancerTargetGroup": {
            "Type": "JDCLOUD::LoadBalance::TargetGroup",
            "Properties": {
                "TargetGroupName": {
                    "Ref": "TargetGroupName"
                },
                "LoadBalancerId": {
                    "Ref": "MyLoadBalancer"
                }
            }
        },
        "MyLoadBalancerRegisterTargets": {
            "Type": "JDCLOUD::LoadBalance::RegisterTargets",
            "Properties": {
                "TargetGroupId": {
                    "Ref": "MyLoadBalancerTargetGroup"
                },
                "TargetSpecs": [
                    {
                        "InstanceId": {
                            "Ref": "MyInstance1"
                        }
                    },
                    {
                        "InstanceId": {
                            "Ref": "MyInstance2"
                        }
                    }
                ]
            }
        },
        "MyLoadBalancerBackend": {
            "Type": "JDCLOUD::LoadBalance::Backend",
            "Properties": {
                "LoadBalancerId": {
                    "Ref": "MyLoadBalancer"
                },
                "Port": 80,
                "BackendName": {
                    "Ref": "LBBackendName"
                },
                "Protocol": "Http",
                "HealthCheckSpec": {
                    "Protocol": "Http",
                    "HttpPath": "/"
                }
            }
        },
        "MyLoadBalancerListener": {
            "Type": "JDCLOUD::LoadBalance::Listener",
            "Properties": {
                "BackendId": {
                    "Ref": "MyLoadBalancerBackend"
                },
                "LoadBalancerId": {
                    "Ref": "MyLoadBalancer"
                },
                "Protocol": "Http",
                "Port": 80
            }
        },
        "MyWaitCondition": {
            "Type": "JDCLOUD::ResourceOrchestration::WaitCondition",
            "DependsOn": [
                "MyInstance1",
                "MyInstance2"
            ],
            "Properties": {
                "Count": 2,
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
        "wordpress_cluster_domain": {
            "Value": {
                "Fn::Join": [
                    ":",
                    [
                        {
                            "Fn::GetAtt": [
                                "MyLoadBalancer",
                                "ElasticIpAddress"
                            ]
                        },
                        {
                            "Fn::GetAtt": [
                                "MyLoadBalancerListener",
                                "Port"
                            ]
                        }
                    ]
                ]
            }
        }
    }
}
```
