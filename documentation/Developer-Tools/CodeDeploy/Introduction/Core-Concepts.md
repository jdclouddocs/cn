# 核心概念

以下是KMS密钥管理服务中使用到的概念及其解释，请参考。
- KMS（Key Management Service）： 密钥管理服务。
- CMK（Custom Master Key）：用户主密钥，通过它来加解密业务使用到的密码、证书、数据密钥等敏感数据，可以通过控制台和API来创建和管理主密钥。
- DEK（Data Encryption Keys）：数据加密密钥，用于信封加密业务数据的密钥，受主密钥保护，也可以通过KMS的API来创建新的数据密钥。
- 服务密钥：由KMS创建的专门为云上服务（如OSS、EBS、RDS）加密使用的密钥。
- OSS（Object Storage Service）：对象存储服务，是面向企业和个人开发者提供高可用、低成本、强安全的云端存储服务。
- EBS（Elastic Block Store）：云硬盘是京东云为云主机提供的低时延、持久性、高可靠的数据块级存储。
- RDS（Relational Database Service）：关系型数据库服务。
- Envelope Encryption：信封加密，一种加密手段，将加密数据的数据密钥封入信封中存储、传递和使用，不再使用主密钥直接加解密数据。
