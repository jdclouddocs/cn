# 编排模板基本语法说明

## 函数（Functions）

JDRO 提供多个内置函数帮助您管理您的堆栈。可以在模板中资源（Resources) 和输出 (Outputs) 时使用。

[Fn::Base64](#fnbase64)

[Fn::FindInMap](#fnfindinmap)

[Fn::GetAtt](#fngetatt)

[Fn::Join](#fnjoin)

[Fn::Select](#fnselect)

[Fn::Split](#fnsplit)

[Fn::Sub](#fnsub)

[Ref](#ref)

[Fn::And](#fnand)

[Fn::Equals](#fnequals)

[Fn::If](#fnif)

[Fn::Not](#fnnot)

[Fn::Or](#fnor)

---

## Fn::Base64
内部函数 Fn::Base64 返回输入字符串的 Base64 编码结果。

### 声明
```
{ "Fn::Base64" : valueToEncode }
```

### 参数

**valueToEncode**：转换成 Base64 的字符串。

### 返回值
用 Base64 表示方法的原始字符串。

### 示例
```
{ "Fn::Base64" : "hello world" }
```

---

## Fn::FindInMap
内部函数 Fn::FindInMap 返回与 Mappings 声明的双层映射中的键对应的值。

### 声明
```
{ "Fn::FindInMap" : [ "MapName", "TopLevelKey", "SecondLevelKey"] }
```

### 参数

**MapName**：Mappings 部分中所声明映射的逻辑名称，包含密钥和值。

**TopLevelKey**：顶层密钥名称。其值是一个键/值对列表。

**SecondLevelKey**：第二层密钥的名称，设置为分配给 TopLevelKey 的列表中的其中一个密钥。

### 返回值
分配给 SecondLevelKey 的值。

### 示例
```
{
  ...
  "Mappings" : {
    "RegionMap" : {
      "cn-north-1" : { "32" : "img-wcewkxc5c1", "64" : "img-wcewkxc5c2" },
      "cn-west-1" : { "32" : "img-wcewkxc5c1", "64" : "img-wcewkxc5c2" },
      "cn-west-2" : { "32" : "img-wcewkxc5c1", "64" : "img-wcewkxc5c2" },
      "cn-south-1" : { "32" : "img-wcewkxc5c1", "64" : "img-wcewkxc5c2" }
    }
  },

  "Resources" : {
     "MyInstance" : {
        "Type" : "JDCLOUD::VM::Instance",
        "Properties" : {
           "ImageId" : { "Fn::FindInMap" : [ "RegionMap", { "Ref" : "JDCLOUD::Region" }, "32"]},
           "InstanceType" : "g.n2.medium"
        }
     }
 }
}
```

---

## Fn::GetAtt
内部函数 Fn::GetAtt 返回模板中的资源的属性值。

### 声明
```
{ "Fn::GetAtt" : [ "logicalNameOfResource", "attributeName" ] }
```

### 参数
**logicalNameOfResource**：资源的逻辑ID。

**attributeName**：您想要获得其值的资源特定属性名称。有关该资源类型之可用属性的详细信息，请参阅资源的引用页面。

### 返回值
属性值。

## 示例
```
{
    "Fn::GetAtt" : [ "MyInstance" , "PrivateIpAddress" ]
}
```

---

## Fn::Join
内部函数 Fn::Join 将一组值连接起来，用特定分隔符隔开。

### 声明
```
{ "Fn::Join" : [ "delimiter", [ "string1", "string2", ... ]] }
```

### 参数
**delimiter**：分隔符。分隔符可以为空，这样就将所有的值直接连接起来。

**[ "string1", "string2", ... ]**：被连接起来的值列表示例。

### 返回值
被连接起来的字符串。

### 示例
```
{
    "Fn::Join" : [ ",", [ "a", "b", "c" ] ]
}
```
返回："a,b,c"。

---

## Fn::Select
内部函数 Fn::Select 通过索引返回数据元列表中的单个数据元。

### 声明
```
{"Fn::Select" : [ "index", [ "value1", "value2", ... ] ]}
```

### 参数
index：待检索数据元的索引。0 到 N-1 之间的某个值（其中 N 代表阵列中元素的数量）

### 返回值
选定的数据元。

### 示例
```
{ "Fn::Select" : [ "1", [ "apples", "grapes", "oranges", "mangoes" ] ] }
```
返回："grapes"。

---

## Fn::Split
内部函数 Fn::Split 通过指定分隔符对字符串进行切片，并返回所有切片组成的列表。

### 声明
```
{"Fn::Split" : [ "delim",  "original_string" ]}
```

### 参数
**delim**：分隔符, 例如: ‘,’，’;’，’\n’，’\t’ 等。

**original_string**：将要被切片的字符串。

### 返回值
返回切片后所有字符串组成的列表。


### 示例
```
{"Fn::Split": [";", "foo; bar; achoo"]}
```
返回：["foo", " bar", "achoo "]。

---

## Fn::Sub
内部函数 Fn::Sub 将输入字符串中的变量替换为您指定的值。

### 声明
```
{ "Fn::Sub" : [ "", { Var1Name: Var1Value, Var2Name: Var2Value } ] }
```

### 参数

**String**：一个带变量的字符串，在运行时会将这些变量替换为其关联的值。以 ${MyVarName} 形式编写变量。变量可以是模板参数名、资源逻辑 ID、资源属性或键值映射中的变量。如果您仅指定模板参数名、资源逻辑 ID 和资源属性，则不要指定键值映射。

如果您指定模板参数名或资源逻辑 ID（例如 ${InstanceTypeParameter}），则返回的值与您使用 Ref 内部函数时返回的值相同。如果您指定资源属性（例如 ${MyInstance.PublicIp}），则返回的值与您使用 Fn::GetAtt 内部函数时返回的值相同。


**VarName**：String 参数中包含的变量的名称。

**VarValue**：在运行时要将关联的变量名称替换为的值。


### 返回值
返回原始字符串，并替换所有变量的值。

### 示例

### 带映射的 Fn::Sub
```
{ "Fn::Sub": [ "www.${Domain}", { "Domain": {"Ref" : "RootDomainName" }} ]}
```

### 不带映射的 Fn::Sub

```
{ "Fn::Sub": "${JDCLOUD::Region}:${JDCLOUD::StackName}:vpc/${vpc}" }
```

---

## Ref
内部函数 Ref 返回指定参数或资源的值。

如果指定参数是资源逻辑ID，则返回资源的值。否则系统将认为指定参数是参数，将尝试返回参数的值。

### 声明
```
{
    "Ref" : "LogicalID"
}
```

### 参数
**LogicalID**：要引用的资源或参数的逻辑ID。

### 返回值
资源的值或者参数的值。

* 它在您指定参数逻辑名称时返回参数值。

* 在您指定资源的逻辑名称时，它会返回一个您一般用于引用该资源的值，如物理 ID。

### 示例
```
{
        "JDCLOUDTemplateFormatVersion": "2018-10-01",
        "Parameters": {
            "AddressPrefix": {
                "ConstraintDescription": "Need give an exact CIDR.",
                "Default": "10.0.0.0/16",
                "Description": "Need give an exact CIDR",
                "Type": "String"
            },
            "SubnetName": {
                "ConstraintDescription": "No overwritten Info.",
                "Default": "MySubnet",
                "Description": "My Subnet Name",
                "MaxLength": "32",
                "MinLength": "1",
                "Type": "String"
            },
            "VPCName": {
                "ConstraintDescription": "No overwritten Info.",
                "Default": "MyVPC",
                "Description": "My VPC Name",
                "MaxLength": "32",
                "MinLength": "1",
                "Type": "String"
            }
        },
        "Resources": {
            "MySubnet": {
                "Properties": {
                    "AddressPrefix": {
                        "Ref": "AddressPrefix"
                    },
                    "SubnetName": {
                        "Ref": "SubnetName"
                    },
                    "VpcId": {
                        "Ref": "MyVPC"
                    }
                },
                "Type": "JDCLOUD::VPC::Subnet"
            },
            "MyVPC": {
                "Properties": {
                    "VpcName": {
                        "Ref": "VPCName"
                    }
                },
                "Type": "JDCLOUD::VPC::VPC"
            }
        }
    }
```

---

## Fn::And
代表 AND 运算符，最少包含两个条件。如果所有指定条件计算为 true，则返回 true；如果任意条件计算为 false，则返回 false。

### 声明
```
{"Fn::And": ["condition", {...}]}
```

### 参数

**condition**：计算为 true 或 false 的条件。

### 返回值
true 或 false。

### 示例
```
{
  "JDCLOUDTemplateFormatVersion" : "2018-10-01",
  "Parameters":{
    "EnvType":{
      "Default":"pre",
      "Type":"String"
    }
  },
  "Conditions": {
    "TestEqualsCond": {"Fn::Equals": ["prod", {"Ref": "EnvType"}]},
    "TestAndCond": {"Fn::And": ["TestEqualsCond", {"Fn::Equals": ["pre", {"Ref": "EnvType"}]}]}
  }
}
```


---


## Fn::Equals
比较两个值是否相等。如果两个值相等，则返回 true；如果不相等，则返回 false。

### 声明
```
{"Fn::Equals": ["value_1", "value_2"]}
```

### 参数
**value**：要比较的任意类型的值。

### 返回值
true 或 false。

### 示例
```
{
  "JDCLOUDTemplateFormatVersion" : "2018-10-01",
  "Parameters":{
    "EnvType":{
      "Default":"pre",
      "Type":"String"
    }
  },
  "Conditions": {
    "TestEqualsCond": {"Fn::Equals": ["prod", {"Ref": "EnvType"}]}
  }
}
```


---

## Fn::If
如果指定的条件计算为 true，则返回一个值；如果指定的条件计算为 false，则返回另一个值。在模板 Resources 和 Outputs 属性值中支持 Fn::If 内部函数。


### 声明
```
{"Fn::If": ["condition_name", "value_if_true", "value_if_false"]}
```

### 参数

**condition_name**：Conditions 中条件对应的条件名称。通过条件名称引用条件。

**value_if_true**：当指定的条件计算为 true 时，返回此值。

**value_if_false**：当指定的条件计算为 false 时，返回此值。

### 返回值

### 示例
```
```


---

## Fn::Not
代表 NOT 运算符。对计算为 false 的条件，返回 true；对计算为 true 的条件，返回 false。

### 声明
```
{"Fn::Not": "condition"}
```

### 参数
**condition**：计算为 true 或 false 的条件。

### 返回值
true 或 false。

### 示例
```
{
  "JDCLOUDTemplateFormatVersion" : "2018-10-01",
  "Parameters":{
    "EnvType":{
      "Default":"pre",
      "Type":"String"
    }
  },
  "Conditions": {
    "TestNotCond": {"Fn::Not": {"Fn::Equals": ["pre", {"Ref": "EnvType"}]}}
  }
}
```

---

## Fn::Or
代表 OR 运算符，最少包含两个条件。如果任意一个指定条件计算为 true，则返回 true；如果所有条件都计算为 false，则返回 false。

### 声明
```
{"Fn::Or": ["condition", {...}]}
```

### 参数
**condition**：计算为 true 或 false 的条件。


### 返回值
true 或 false。

### 示例
```
{
  "JDCLOUDTemplateFormatVersion" : "2018-10-01",
  "Parameters":{
    "EnvType":{
      "Default":"pre",
      "Type":"String"
    }
  },
  "Conditions": {
    "TestEqualsCond": {"Fn::Equals": ["prod", {"Ref": "EnvType"}]},
    "TestOrCond": {"Fn::And": ["TestEqualsCond", {"Fn::Equals": ["pre", {"Ref": "EnvType"}]}]}
  }
}
```


---



