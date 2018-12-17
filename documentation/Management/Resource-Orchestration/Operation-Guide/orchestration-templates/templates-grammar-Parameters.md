# 编排模板基本语法说明

## 参数（Parameters）
在创建模板时，使用参数（Parameters）可提高模板的灵活性和可复用性。创建资源栈时，可根据实际情况，替换模板中的某些参数值。

### 在模板中定义参数

以下示例声明名为 InstanceTypeParameter 的参数。利用此参数，您可以为资源栈指定实例类型以在创建或更新资源栈时使用。

请注意，InstanceTypeParameter 在此示例中具有默认值 g.n2.medium。


```
"Parameters" : {
  "InstanceTypeParameter" : {
    "Type" : "String",
    "Default" : "g.n2.medium",
    "AllowedValues" : ["g.n2.medium", "g.n2.large", "g.n2.xlarge"],
    "Description" : "选择实例类型，默认为 g.n2.medium，可选值为g.n2.medium, g.n2.large, g.n2.xlarge"
  }
}
```


### 在模板中引用参数

您使用 Ref 内部函数来引用某个参数，使用该参数的值来预置资源栈。您在模板的 Resources 和 Outputs 部分中引用该参数。

在以下示例中，vm实例资源的 InstanceType 属性引用了 InstanceTypeParameter 参数值：

```
"MyInstance" : {
  "Type" : "JDCLOUD::VM::Instance",
  "Properties" : {
    "InstanceType" : { "Ref" : "InstanceTypeParameter" },
    "ImageId" : "img-wcewkxc5c1"
  }
}
```

## 参数的一般要求

使用参数时存在以下要求：

* 一个 JDRO 模板中最多可包含 60 个参数。

* 必须为每个参数提供一个逻辑名称 (也称为逻辑 ID)，该名称必须是字母数字，并且在模板内的所有逻辑名称中必须是唯一的。

* 必须向每个参数分配一个 JDRO 支持的参数类型。有关更多信息，请参阅类型。

* 必须向每个参数分配一个值，也可以定义默认值。

### 语法

每个参数由参数名称和参数属性组成。

参数名称必须为字母数字，并且在同一个模板中不能与其它参数名称重复。

参数属性列表：

|属性|必须|描述|
|:--|:--|:--|
|Type|是|参数的数据类型。<br>JDRO目前支持以下参数类型：<br><br>**1）String**<br>字符串。如："g.n2.medium"。<br><br>**2）Number**<br>整数或浮点数。如：3.14。<br><br>**3）CommaDelimitedList**<br>一组用逗号分隔的字符串或数字，可通过 Fn::Select 函数索引值。如："80, foo, bar"。<br><br>**4）List\<Number\>**<br>一组用逗号分隔的整数或浮点数。JDRO 将参数值验证为数字，但当您在模板中的其他位置使用该参数时（例如，通过使用 Ref 内部函数），该参数值将变成字符串列表。例如，用户可指定 "80,20"，并且 Ref 将生成 ["80","20"]。<br><br>**5）Boolean**<br>一个布尔值。如：true 或者 false。<br><br>**6）JDRO特定的参数类型**<br>JDRO 的特殊值，例如 vm 镜像。有关更多信息，请参阅 [JDRO 特定的参数类型](#JDROsptype)。|
|Default|否|在创建资源栈时，如果用户没有传入指定值，资源编排服务会检查模板中是否有定义默认值。如果有定义默认值，则使用默认值，否则报错。|
|NoEcho|否|当调用查询资源栈时，是否输出参数值。如果将值设置为 true，则只输出星号 (******)。|
|AllowedValues|否|包含参数允许值的列表。|
|AllowedPattern|否|一个正则表达式，用于检查用户输入的字符串类型的参数是否匹配。如果用户输入的不是字符串类型，则报错。|
|MinLength|否|一个整数值，允许 String 类型使用的字符的最小数目。|
|MaxLength|否|一个整数值，允许 String 类型使用的字符的最大数目。|
|MinValue|否|一个数字值，允许 Number 类型使用的最小数字值。|
|MaxValue|否|一个数字值，允许 Number 类型使用的最大数字值。|
|Description|否|描述参数的字符串。|
|ConstraintDescription|否|违反该参数约束条件时，说明该约束条件的字符串。|

#### JDRO 特定的参数类型
为了方便用户，定义一些特定参数类型，若用户在模板中设置了这些参数类型，则在创建资源栈时，该类型的参数对应的值，以下拉列表框列出用户对应区域下的资源。支持的特定参数类型如下：

| 类型                              | 说明                                                                                        |
| --------------------------------- | ------------------------------------------------------------------------------------------- |
| JDClOUD:::AvailabilityZone::Name  | 列出当前地域下的可用区                                                                      |
| JDClOUD::VM::PublicImage::Name    | 列出当前地域下可用的公共镜像                                                                |
| JDClOUD:::VM::PrivateImage:::Name | 列出当前地域下可用的私有镜像                                                                |
| JDClOUD:::VM::SharedImage:::Name  | 列出当前地域下可用的共享镜像                                                                |
| JDClOUD:::VM::Instance::Id        | 列出当前地域下的云主机，下拉列表中展示格式为“名称(ID)”,例如：VM_test(i-nwyuh3eben)        |
| JDClOUD:::VM::Disk::Id            | 列出当前地域下的云硬盘，下拉列表中展示格式为“名称(ID)”,例如：disk_test(vol-8i4mv87d48)    |
| JDClOUD:::VM::KeyPair::KeyName    | 列出当前地域下的密钥名称                                                                    |
| JDClOUD::SecurityGroup::GroupName | 列出当前地域下的安全组名称                                                                  |
| JDClOUD::VPC::VPC::Id             | 列出当前地域下的子网，下拉列表中展示格式为“名称(ID)”,例如：Subnet_test(subnet-d6x9i2t4i1) |


