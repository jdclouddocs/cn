# 使用说明

**环境准备**

​	在您开始调用京东云open API之前，需提前在京东云用户中心账户管理下的AccessKey管理页面申请accesskey与secretKey密钥对（简称AK/SK）。AK/SK信息请妥善保管，如果遗失可能会造成非法用户使用此信息操作您在云上的资源，给你造成数据和财产损失。

​	KMS秘钥管理服务是以openAPI的形式对外提供服务，当访问不同的Region的时候，需要不同的域名。通过内网和外网访问同一个Region所需要的Endpoint也是不同的，其中外网指的是通过互联网访问KMS服务的域名，该类访问会消耗一些流量；内网EndPoint指的是京东云VPC内部访问所使用的域名。

各个Region对应的Endpoint如下表：

| 区域      | 公网Endpoint                   | 内网EndPoint                            |
| --------- | ------------------------------ | --------------------------------------- |
| 华北-北京 | kms.cn-north-1.jdcloud-api.com | kms.internal.cn-north-1.jdcloud-api.com |
| 华东-上海 | kms.cn-east-2.jdcloud-api.com  | kms.internal.cn-east-2.jdcloud-api.com  |
| 华东-宿迁 | kms.cn-east-1.jdcloud-api.com  | kms.internal.cn-east-1.jdcloud-api.com  |
| 华南-广州 | kms.cn-south-1.jdcloud-api.com | kms.internal.cn-south-1.jdcloud-api.com |

**使用京东云openAPI**

京东云openAPI的地址如下表：

| 开发语言 | Github地址                                        |
| -------- | ------------------------------------------------- |
| Golang   | https://github.com/jdcloud-api/jdcloud-sdk-go     |
| Java     | https://github.com/jdcloud-api/jdcloud-sdk-java   |
| .net     | https://github.com/jdcloud-api/jdcloud-sdk-net    |
| python   | https://github.com/jdcloud-api/jdcloud-sdk-python |
| php      | https://github.com/jdcloud-api/jdcloud-sdk-php    |
| nodejs   | https://github.com/jdcloud-api/jdcloud-sdk-nodejs |


**代码示例**

```
package main

import (
    "encoding/base64"
    "fmt"
    core "github.com/jdcloud-api/jdcloud-sdk-go/core"
    kms "github.com/jdcloud-api/jdcloud-sdk-go/services/kms/apis"
    client "github.com/jdcloud-api/jdcloud-sdk-go/services/kms/client"
    models "github.com/jdcloud-api/jdcloud-sdk-go/services/kms/models"
    "time"
)

func main() {
    /** 设置Credentials对象 **/
    accessKey := "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    secretKey := "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    credentials := core.NewCredentials(accessKey, secretKey)

    /** 设置Config对象 **/
    config := core.NewConfig()
    config.SetEndpoint("apigw-test.openapi.jdcloud.com")
    config.SetScheme("http")

    /** 设置Client对象 **/
    client := client.NewKmsClient(credentials)
    client.SetConfig(config)

    /** 将待加密的数据进行base64编码 **/
    data := base64.StdEncoding.EncodeToString("Hello World.")

    /** 设置加密所用密钥ID **/
    keyId = "aabbccddeeffgghh"

    /** 创建加密请求 **/
    reqEnc := kms.NewEncryptRequest(keyId)
    reqEnc.SetPlaintext(reqEnc)

    /** 发送加密请求 **/
    if resp, err := client.Encrypt(reqEnc); err != nil {
        /** TODO: error **/
    } else {
        /** we get resp.Result.CiphertextBlob **/
        fmt.Println("Cipher: ", resp.Result.CiphertextBlob)
    }
}

```
