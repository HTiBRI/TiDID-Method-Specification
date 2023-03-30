## 介绍
[天河分布式身份](https://tianhecloud.com/product/identityAuth)是基于区块链的公共身份认证基础服务，提供分布式实体身份标识及管理功能，致力成为智慧社会基础服务，通过可信认证身份实现价值交换。

## 本文档的状态
本草案文档遵循[W3C DIDs](https://www.w3.org/TR/2022/REC-did-core-20220719/)标准，并且将在未来一到两年内持续完善和改进。

## 天河分布式身份方法规范
天河分布式身份的DID标识定义为：`ti`。 `did:ti`身份识别系统的DID方法格式由以下格式组成：

```
# TiDID
did:ti:<method-specific-id>

# method-specific-id 
method-specific-id = base58(ripemd160(sha256(<Public Key>)))
```

*（参考比特币的设计）*

为了提高互操作性，标识符是从公钥计算出来的，然后在去中心化的区块链中发布。 DID 的一个例子是 `did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W`。

## DID文档
`did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4Wis`的DID文档示例：
```
{
  "@context": "https://w3id.org/did/v1",
  "id": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W",
  "created": 1676454887,
  "updated": null,
  "verificationMethod": [
    {
      "id": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W",
      "type": "Secp256k1",
      "controller": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W",
      "publicKeyHex": "305c300d06092a864886f70d0101010500034b003048024100ca8724f210c33858"
    }
  ],
  "authentication": [
       "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W"
  ],
  "service": []
}
```

## TiDID 操作
`did:ti` 方法的操作由[天河分布式身份](https://tianhecloud.com/product/identityAuth)（TiDID）服务实现，该服务生成标识符、创建 DID 文档并保存在 [TiChain](https://tianhecloud.com/safeServices/safeTHChain) 中。 用户在进行 TiDID 操作之前，需要为 TiDID 服务创建一个账号。 公共请求参数参见天河服务统一API文档。
### 创建
```
# endpoint
the TiDID service api url

# Input
{
    AK/SK, PUBICKEY
}

# Output
{TiDID}
```

示例：
```
POST / HTTP/1.1
Host: did.tianhecloudapi.com
Content-Type: application/json
X-TC-Action: CreateTiDid
<public request parameters>

{
    "PublicKey": "7430488790716037455843285301209475352340487000630103610741044889217493578632422416688836099438173061252937088504002356479929639821721792771207629422355422",
}
```
响应：
```
{
    "Response": {
        "Did": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W",
        "sequenceNo": "TIDID-BV8pb-1yUYw-timestamp-servicename-23"
    }
}
```
### 读取
```
# endpoint
the TiDID service api url

# Input
{
    AK/SK, TiDID
}

# Output
{TDID Document}
```

示例：
```
POST / HTTP/1.1
Host: did.tianhecloudapi.com
Content-Type: application/json
X-TC-Action: GetDidDocument
<public request parameters>

{
    "Did": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W"
}
```
响应：
```
{
    "Response": {
        "Name": "test",
        "Document": "...",
        "sequenceNo": "TIDID-DAUqv-TO6sB-timestamp-servicename-40"
    }
}
```

### 升级
DID文档暂不支持升级.
  
### 撤销
```
# endpoint
the TiDID service api url

# Input
{
    AK/SK, TiDID
}

# Output
{sequence No.}
```
示例：
```
POST / HTTP/1.1
Host: did.tianhecloudapi.com
Content-Type: application/json
X-TC-Action: DeactivateTiDid
<public request parameters>

{
    "Did": "did:ti:3K9wuWcR9R3ZL1EaYjkHojBHTD7BCDVCPxTMr2QMvH1u711a3N9eS4W"
}
```
响应：
```
{
    "Response": {
        "sequenceNo": "TIDID-wu5o5-w1Kgc-timestamp-servicename-73"
    }
}
```
  
## 安全考量
1. TiDID方法的相关操作都需要首先通过天河云的统一API平台进行数据的交互。用户在使用TiDID的操作之前需要使用AK/SK认证。AK/SK认证有助于进一步提高TDID业务的安全性。
2. 天河链是天河国云自主研发的基于安全软硬件并融合了数据可信计算和流转的底层机制的区块链。调用 TiDID 的操作方法需要有 TiChain 节点的云账号。
3. 和比特币相似，TiDID的标识是由公钥通过地址转换算法计算出来的。
  
## 隐私考量
1. TiDID服务以及其DID文档不包含用户的隐私数据，只保存了用户的公钥数据。 通过DID 文档不能识别出DID Subject。
2. 用户通过TiDID签发可验证的凭证，达成使用目的同时保护自己的隐私信息。 VC方案保障用户的隐私安全。
3. 用户的可验证凭证通过对称加密密钥如SK或其他协商密码加密存储在云存储中，只有拥有密钥的实体才能解密和查看。
4. 用户可验证凭证存储在云账号下，只能通过云端AK/SK认证读取。
