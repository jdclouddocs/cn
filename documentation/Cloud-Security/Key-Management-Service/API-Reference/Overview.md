# 使用说明

**环境准备**

在您开始调用京东云open API之前，需提前在京东云用户中心账户管理下的AccessKey管理页面申请accesskey与secretKey密钥对（简称AK/SK）。AK/SK信息请妥善保管，如果遗失可能会造成非法用户使用此信息操作您在云上的资源，给你造成数据和财产损失。

KMS秘钥管理服务是以RESTful API的形式对外提供服务，当访问不同的Region的时候，需要不同的域名。通过内网和外网访问同一个Region所需要的Endpoint也是不同的，其中外网指的是互联网；内网指的是京东云云主机访问云存储服务所使用的域名，通过内网访问可以获得更好的性能和更低的网络延时。

例如华北Region的外网Endpoint是XXX.cn-north-1.jcloudcs.com，内网Endpoint是XXX-internal.cn-north-1.jcloudcs.com。

各个Region对应的Endpoint如下表：

|区域|Endpoint|
| - | - |
|华北|xxx
|华东上海|xxx
|华东宿迁|xxx
|华南广州|xxx

**请求方法**

|方法|说明|
| - | - |
|GET|请求服务器返回指定资源
|PUT|请求服务器更新指定资源
|POST|请求服务器新增资源
|DELETE|请求服务器删除指定资源
|PATCH|请求服务器更新指定资源

**请求结构**

https :// {Endpoint} / {resource-path} ? {query-string}

**请求体/响应体**

请求体必须以json的格式进行数据传输，由服务端返回的响应体也是json的数据格式。

请求参数：

公共返回参数：

用户发送的每次接口调用请求，无论成功与否，系统都会返回一个唯一识别码requestId给用户。
响应体分为调用成功与失败两种。格式分别为：
成功返回：

{

"result": {  …  },

"requestId": "xxx"

}

错误返回：

{

"requestId": "333333333333333",
  "error": {
    "message": "XXXXX",
    "status": "",
    "code": XXXX
  }
}

