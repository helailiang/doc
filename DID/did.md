DID Registry服务 uPort，uPort可以帮助以太坊上的用户完成DID的注册

# 深入理解去中心化身份DID (Decentralized ID)

https://cloud.tencent.com/developer/article/2404252

https://www.cnblogs.com/studyzy/tag/DID

# 相关did文档：

去中心化标识符解析 (DID解析)：https://studyzy.github.io/did-resolution/

去中心化标识符 (DID)：https://w3c.github.io/did/#did-url-syntax

Verifiable Credentials API: https://w3c-ccg.github.io/vc-api/

vc : https://www.w3.org/TR/vc-data-model/

w3c 相关标准： https://www.w3.org/groups/wg/did/publications/

#### W3C ：https://github.com/Instun?q=&type=all&language=&sort=stargazers

Identifier标识符识别符识别码标示符
Foundation基础基金会地基根据创建，创办基本原理粉底霜房基
Universal普遍的共同的全世界的普遍存在的全体的广泛适用的一般性一般概念全称命题
Driver驾驶员司机驱动程序驱动因素驾车者球杆
Credential凭据证书凭证证件资格证书
issuer发行人；发布人证券发行人信用证开证人
presents目前现在礼物礼品提出展现，显示，表现提交颁发授予把…交给present的第三人称单





- DID标识符（Decentralized Identifier）
- DID文档（DID Document）
- DID解析器（DID resolver）
- 可验证声明（Verifiable Credential）
- 身份存储库（Identity Hub）



## DID标识符 （https://w3c.github.io/did/#did-url-syntax）

* did:{method-name}:{method-specific-id} [ "?" query ] [ "#" fragment ]

  DID方法需要在w3c进行注册，DID 已存在的方法：https://www.w3.org/TR/did-extensions-methods/#did-methods

```javascript
例如：
did:ethr:0xE6Fe788d8ca214A080b0f6aC7F48480b2AEfa9a6
did:web:veramo.dev

# path 
did:example:123456/path

# Query
did:example:123456?versionId=1

#Fragment：
did:example:123#public-key-0

```

## DID文档

DID标识符只是表示一个身份的标识符，不包含身份的信息。而DID文档就是用于描述身份详细信息的文档，一个DID标识符关联到一个DID文档。

DID文档一般包含以下内容：

DID标识符（必需）；
一个加密材料的集合，比如公钥；
验证方法集合；
一个服务端点的集合；
时间，包括创建时间和更新时间。
DID文档的示例： [更多例子](https://w3c.github.io/did/#did-documents)

```json
// 20201110152830
// https://veramo.dev/.well-known/did.json

{
  "@context": "https://w3id.org/did/v1",
  "id": "did:web:veramo.dev",
  "publicKey": [
    {
      "id": "did:web:veramo.dev#0405aa19bb98a5fd29c15a730cb5064ca128dea19247b896b1a7bdad0b4bccccda9b47366cd1359e740d938e5a47d7bed0501150e8a1623805ac47c489421b1506",
      "type": "Secp256k1VerificationKey2018",
      "controller": "did:web:veramo.dev",
      "publicKeyHex": "0405aa19bb98a5fd29c15a730cb5064ca128dea19247b896b1a7bdad0b4bccccda9b47366cd1359e740d938e5a47d7bed0501150e8a1623805ac47c489421b1506"
    }
  ],
  "authentication": [
    {
      "type": "Secp256k1SignatureAuthentication2018",
      "publicKey": "did:web:veramo.dev#0405aa19bb98a5fd29c15a730cb5064ca128dea19247b896b1a7bdad0b4bccccda9b47366cd1359e740d938e5a47d7bed0501150e8a1623805ac47c489421b1506"
    }
  ],
  "service": [
    {
      "id": "did:web:veramo.dev#msg",
      "type": "Messaging",
      "serviceEndpoint": "https://veramo.dev/messaging",
      "description": "Handles incoming POST messages"
    }
  ]
}
```


其中:

* @context字段指明了该文档的版本；
* id字段指明了该文档关联到的DID；
* publicKey字段指明了相关的公钥；
* authentication字段则引用了publicKey字段里存储的公钥，并组成了验证该DID用户身份的方式。

DID文档其实和传统PKI系统里的证书有点类似。

DID文档实际格式可以是JSON，也可以是JSON-LD或者YAML、XML等。其存储需要上链，或者至少哈希上链。



## DID解析器

解析器的作用是通过DID标识符来获取DID文档，这样，当DID用户登录某个服务时，该服务提供商调用解析器来获取DID文档，从而知道了如何来验证DID用户。

DID解析器的规范主要是**DIF**主导的。

DIF Universal Resolver的架构如下图。其先通过DID标识得到该DID标识的method，然后去调用该method对应的Driver来完成最终的解析，这些Driver的具体实现不做限定，但是要遵循接口的规范。

## 可验证声明VC： Verifiable Claims

VC的目的前面说过，就是数据授权，而且是尽可能细粒度的授权，从而尽量降低隐私数据的泄露

VC运行需要有一套机制，需要有很多角色:

发行者（Issuer） ：能开具VC（能访问用户数据），如政府、银行、大学等机构和组织。
验证者（Verifier）：能验证VC，由此可以提供给出示VC者某种类型的服务，如游戏网站、香烟店。
持有者（Holder） ：即用户，能向Issuer请求&接收、持有VC，向Verifier出示VC，开具的VC可以存放在钱包里，方便以后再次证明时使用。
标识符注册机构（Registry） ：维护DID标识符及密钥（DID文档）的数据库，如区块链、可信数据库、分布式账本等。

在VC的内部的结构，通常包括三个部分：

- VC元数据（Credential Metadata），主要就是发行人、发行日期、声明的类型等信息。
- 声明（Claims），一个或者多个关于主体的说明。比如身份证作为公安机关颁发给我的VC，在声明中会包含：姓名、性别、出生日期、民族、住址等信息。
- 证明（Proofs），通常就是颁发者的数字签名，保证了本VC能够被验证，防止VC内容被篡改以及验证VC的颁发者。

```json
{
  // VC内容所遵循的JSON-LD标准
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://www.w3.org/2018/credentials/examples/v1"
  ],
  // 本VC的唯一标识，也就是证书ID
  "id": "http://example.edu/credentials/1872",
  // VC内容的格式
  "type": ["VerifiableCredential", "AlumniCredential"],
  // 本VC的发行人
  "issuer": "https://example.edu/issuers/565049",
  // 本VC的发行时间
  "issuanceDate": "2010-01-01T19:73:24Z",
  // VC声明的具体内容
  "credentialSubject": {
    // 被声明的人的DID
    "id": "did:example:ebfeb1f712ebc6f1c276e12ec21",
      // 声明内容:姓名、性别、生日、民族、住址等
    "name":"小明",
    "gender":"男",
    "birthdate":"2000-01-01",
    "nation":"汉",
    "address":"A省B市C区D街道xxx号",
    //接下来是种子数、默克尔根、公安的签名
    "seed":"23523865082340324",
    "merkleRoot":"ea59a369466be42d1a4783f09ae0721a5a157d6dba9c4b053d407b5a4b9af145",
    "rootSignature":"3066022051757c2de7032a0c887c3fcef02ca3812fede7ca748254771b9513d8e266",
    "signer":"did:公安部门ID#keys-1"
    // 声明的断言内容
    "alumniOf": {
      "id": "did:example:c276e12ec21ebfeb1f712ebc6f1",
      "name": [{
        "value": "Example University",
        "lang": "en"
      }, {
        "value": "Exemple d'Université",
        "lang": "fr"
      }]
    }
  },
  // 对本VC的证明
  "proof": {
    // 签名算法
    "type": "RsaSignature2018",
    // 签名创建时间
    "created": "2017-06-18T21:19:10Z",
    // 本证明的目的
    "proofPurpose": "assertionMethod",
    // 验证本签名的公钥的ID
    "verificationMethod": "https://example.edu/issuers/keys/1",
    // 数字签名的内容
    "jws": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..TCYt5X
      sITJX1CxPCT8yAV-TVkIEq_PbChOMqsLfRoPsnsgw5WEuts01mq-pQy7UJiN5mgRxD-WUc
      X16dUEMGlv50aqzpqh4Qktb3rk-BuQy72IFLOqV0G_zS245-kronKb78cPN25DGlcTwLtj
      PAYuNzVBAh4vGHSrQyHUdBBPM"
  }
}
```

这里最重要的内容当然是credentialSubject和proof。



### 可验证表达（Verifiable presentation简称VP）是VC持有者向验证者表名自己身份的数据。

VP就可以实现选择性身份信息验证。VP的选择性披露，一般来说基于默克尔树（Merkle Tree）特性来实现。

 https://www.cnblogs.com/studyzy/p/14202037.html

在VP的内部的结构，通常包括三个部分：

- VP元数据（Presentation Metadata）：包含了版本和本JSON对象的类型等信息
- VC列表（Verifiable Credentials）：VC内容列表，如果是选择性披露或者隐私保护的情况，可能包含部分或者不包含任何VC。
- 证明（Proofs）：持有者对本VP的签名信息

```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://studyzyexamples.com/identity/v1"
  ],
  "type": "VerifiablePresentation",
  // 本VP包含的VC的内容
  "verifiableCredential": [{
    "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://studyzyexamples.com/identity/v1"
  ],
  "id": "vc511112200001010015",
  "type": ["VerifiableCredential", "Identity"],
  "issuer": "did:公安部门ID",
  "issuanceDate": "2010-07-01T19:73:24Z",
  "credentialSubject": {
    "id": "did:cid:511112200001010015",
    //以下是要选择性披露的内容
    "birthdate":"2000-01-01",
    //以下是验证披露字段有效性的数据
    //数据在默克尔树中的索引
    "dataIndex":2,
    //本数据加盐的值
    "salt":"6b264354ed367ced527a86d38f75f9c3888bd3939f548cc48d93af435890b84a",
    //默克尔验证路径
    "merklesibling":"34b64151443c3124620bf4ff69a05e97d580f0878b374b8343c6a5c3d8223435 9d2b5b35ccb5bf18747c1f5dc05771c68ce613e6eb0c5f5ef77cec8ba3e9da67 bb82c63d4e21525125bf66a6724fbb4dcbded26aae2baa2633235dc12730016e",
    //默克尔根哈希
    "merkleRoot":"ea59a369466be42d1a4783f09ae0721a5a157d6dba9c4b053d407b5a4b9af145",
    //公安机关对默克尔根的签名
    "rootSignature":"3066022051757c2de7032a0c887c3fcef02ca3812fede7ca748254771b9513d8e266",
    //用的公安机关哪个Key进行的签名
    "signer":"did:公安部门ID#keys-1"
  },
  
  }],
  // Holder小明对本VP的签名信息
  "proof": {
    "type": "Secp256k1",
    "created": "2010-07-02T21:19:10Z",
    "proofPurpose": "authentication",
    "verificationMethod": "did:cid:511112200001010015#keys-1",
    // challenge和domain是为了防止重放攻击而设计的
    "challenge": "1f44d55f-f161-4938-a659-f8026467f126",
    "domain": "4jt78h47fh47",
    "jws": "eyJhbGciOiJSUzI1NiIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..kTCYt5
      XsITJX1CxPCT8yAV-TVIw5WEuts01mq-pQy7UJiN5mgREEMGlv50aqzpqh4Qq_PbChOMqs
      LfRoPsnsgxD-WUcX16dUOqV0G_zS245-kronKb78cPktb3rk-BuQy72IFLN25DYuNzVBAh
      4vGHSrQyHUGlcTwLtjPAnKb78"
  }
}
```








## 身份存储库 Identity Hub

DID文档里只存储和身份相关的数据；而Identity Hub就是用来存储用户的隐私数据的。Identity Hub，

虽然是身份的Hub，但是存储的是数据，可以理解为数据银行。

- Identity Hub是去中心化的、链下的个人数据存储，可将对个人数据的控制权交给用户。
  它们允许用户以安全而隐私的方式存储其敏感数据，无用户的显式授权就无法获取用户数据。
- Identity Hub实际在哪由用户决定，可以是本地（手机、PC），也可以是云端。



一个简例:假设小明有一个以太坊上的账户0x96f…3d4，小明想使用DID来登录支持DID的游戏网站A。

```tex
步骤 1: 注册DID
-------------------------------------
小明 -> DID Registry服务(uPort): 请求注册DID
DID Registry服务 -> 以太坊区块链: 将DID文档(含公钥等)存储于链上
小明 <- DID Registry服务: 获取did:eth:0x96f...3d4

步骤 2: 使用DID登录游戏网站A
-------------------------------------
小明 -> 游戏网站A: 使用did:eth:0x96f...3d4尝试登录
游戏网站A -> DID解析器: 查询did:eth:0x96f...3d4对应的DID文档
游戏网站A <- DID解析器: 接收DID文档，了解验证方式

步骤 3: 验证年龄>16岁
-------------------------------------
小明 -> 政府机构G(Issuer): 请求开具年龄>16岁的VC
政府机构G -> 身份存储库(Identity Hub): 查询小明相关隐私数据
政府机构G <- 身份存储库: 确认小明确实>16岁
小明 <- 政府机构G: 收到带有G签名的VC

步骤 4: 向游戏网站A出示VC
-------------------------------------
小明 -> 游戏网站A: 出示VC证明年龄>16岁
游戏网站A -> 验证逻辑: 验证VC上的签名是否来自政府机构G
游戏网站A <- 验证逻辑: 确认签名有效，信任该VC
游戏网站A -> 小明: 提供游戏时间

步骤 5: 当游戏网站A倒闭后
-------------------------------------
小明 -> 游戏网站B: 使用did:eth:0x96f...3d4尝试登录
游戏网站B -> DID解析器: 查询DID文档
游戏网站B <- DID解析器: 成功获取DID文档信息
小明 <- 游戏网站B: 登录成功
```



## 3. [Cryptographic Suites](https://www.w3.org/TR/vc-data-integrity/#cryptographic-suites)

```json
"proof": {
              "type": "DataIntegrityProof",
              "created": "2025-04-27T17:58:33Z",
              "verificationMethod": "did:example:8adf89f92b354d73acdfa91e619204ee#key-1",
              "cryptosuite": "ecdsa-rdfc-2019",
              "proofPurpose": "assertionMethod",
              "proofValue": "z2LeuoNi3yR1b6c3fkRsEvXJ5ex8X4RdutyK7L6HAo2bJQwr21w85Y5KWy3DptXR8ke52Assqik6wKTy9DKqkEZ2r"
    }
```

对 DataIntegrityProof 介绍：https://www.w3.org/TR/vc-data-integrity/#dataintegrityproof

对 cryptosuite 介绍：https://w3c.github.io/vc-extensions/#securing-mechanisms



```json
{
  "proof": {
      "type": "JsonWebSignature2020",
      "created": "2019-12-11T03:50:55Z",
      "jws": "eyJhbGciOiJQUzI1NiJ9..Qrm5ulEAIrGFoGJX7lxq61vVSE-z5y-s8vuUoNilkAqMQYu-v2z3FbRX9LHc5T18pozKtRkN4-mon22juXCIkXNuGF41D_amQuU_XtbDwrZmDELPhb_itEGTeeygQyfvo0eAM9dTv7GuH5XHYB0SjfkMHRUHfikfi9wCRQUrMZyABY6057LU5lkaYO-hWGpfBLQVVnDbKAXKycN_Lx-gwH1x5eZdQnUticixtwIqJaYSF54hJ8cp8us-OqQCG015dauEYdoI75BjX_GEo4t7PvP85BtuSLXE3ziSBqEnQ2wHwuZT2oJE-a_wJ9uv2iCGJFwcRkujuSZ52gITHxFByg",
      "proofPurpose": "assertionMethod",
      “verificationMethod”：“https://example.com/issuer/123#ovsDKYBjFemIy8DVhc-w2LSi8CvXMw2AYDzHj04yxkc”
    }
}
```

