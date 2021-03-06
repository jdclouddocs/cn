# 创建实例

您可以通过控制台快速创建区块链数据服务实例

## 前提条件

* 已开通区块链数据服务权限

## 操作步骤

1.  登录[区块链数据服务控制台](https://bds-console.jdcloud.com/block/list)。 
2.  在实例里列表页，点击点击 **创建** 按钮，进入创建页。
3.  计费方式只有包年包月。
4.  实例配置的参数说明如下：
    * 地域：选择实例所在的地域，不同地域资源的内网不互通，创建后不能更改，关于地域的详细说明，请参考 [地域与可用区](https://www.jdcloud.com/help/detail/1844/isCatalog/1)。
    * 数据库类型：目前仅支持SQL Server 2016 企业版。
    * 规格：目前仅支持56核224GB配置。
    * 存储空间：提供的存储规格是6000GB，该存储空间包括数据包括数据文件，系统文件等
    * 私有网络：如果当前地域下没有私有网络或子网，请预先进行相关创建，界面上增加了超链接地址，可以跳转到相应的页面创建 [私有网络](https://console.jdcloud.com/host/vpc/list)，[子网](https://console.jdcloud.com/host/subnet/list)；在选择私有网络的时候，请确保需要连接数据库实例的云主机和区块链数据服务实例在同一个私有网络内。
    * 基本信息
        *   实例名称：允许重复，名称的长度和字符有一定限制，具体以控制台为准。
    * 购买时长：可选择1个月至2年；购买的时长越长，折扣力度越大，具体以控制台为准。
       ![创建实例](Pic/创建实例.png)
5. 点击 **立即购买** 进入订单确认页。
6. 阅读完区块链云服务服务条款，按照提示完成后续操作 
    * 包年包月实例：点击立即支付，进入到支付确认页，可以选择多种支付方式，如可以使用代金券，余额，银行卡等等。
    
  

