## SD-JWT

[Selective Disclosure for JWTs (SD-JWT)](https://www.ietf.org/archive/id/draft-ietf-oauth-selective-disclosure-jwt-22.html#name-w3c-verifiable-credentials-) 这是sd-jwt规范里的基于w3c vc2.0的一个例子

https://www.iana.org/assignments/jwt/jwt.xhtml#IESG



### 理解SD-JWT的结构：

​    \- SD-JWT实际上是一个字符串，由两部分组成：

​        a) 一个签名的JWT（包含公开声明和所有声明的哈希承诺，即_sd数组）

​        b) 一组可选的披露字符串（disclosures），每个披露字符串是一个base64url编码的数组，数组形式为：[salt, claim_key, claim_value]（或者对于数组中的元素，形式可能有所不同，但基本结构如此）

```json
// <发行者签名的 JWT>~<披露 1>~<披露 2>~...~<披露 N>~

eyJhbGciOiJFZERTQSJ9.eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy92MSJdLCJjbmYiOnsiandrIjp7ImFsZyI6IkVkRFNBIiwiY3J2IjoiRWQyNTUxOSIsImtpZCI6Imlzc3Vlci1rZXkiLCJrdHkiOiJPS1AiLCJ4IjoiS1gveHI4WGFmRk9GMTB1SjczazErUnNXQmNKRXRna1BnelU3VDFKYmdOST0ifX0sImNyZWRlbnRpYWxTdWJqZWN0Ijp7Il9zZCI6WyJwSDlUSHZicEtfSDBTSUFtYzRBbVF2MXBLa0MwVUNTRDJRa0RfQi1IVWpvIiwiSXh6MHktU29oWTlqRGJldkRadmF6MVlhclRjb1VzS3ZhQWUwRmpQb1Q5MCIsIjJ1emtWdmEwOWlQRW9jZnN4bUVGeF9fTU00WnBFVmJRM2tkMnYwdTNjdmciLCJkYnRxMnhFeXFZMThIdTlEb1pORTFWbTJDelJsNlBHZUFiN3lOeWQ3Rzk4Il19LCJpZCI6Imh0dHBzOi8vZXhhbXBsZS5lZHUvY3JlZGVudGlhbHMvMTg3MiIsImlzc3VhbmNlRGF0ZSI6IjIwMjUtMDYtMjZUMTQ6NDU6MzQrMDg6MDAiLCJpc3N1ZXIiOiJkaWQ6ZXhhbXBsZTppc3N1ZXIiLCJ0eXBlIjpbIlZlcmlmaWFibGVDcmVkZW50aWFsIl19.oFysymZVJpvtY63851LJiMqXHKmSt-eFHynoJHxkH0nI68NtJIrYRuCRPrb68qsZYa5l0W0KZTq90Iz0amFsCw~WyI3dWJ3eDJjUHVNS0xnTWNjZWhQLTRRIiwiYWRkcmVzcyIsIuWMl-S6rOacm-S6rCJd~WyJSMjRBWkV4NDhrSEpOT0w4Snc4Yy1RIiwibmFtZSIsIkFsaWNlIl0~WyIxSy1MUkNqVXpVT2tMb0VRMmhhTVJnIiwiYWdlIiwiMzAiXQ~WyJqb2JFS05USldCMzZHYTRLa2poNGpnIiwiZW1haWwiLCJhbGljZUBleGFtcGxlLmNvbSJd
```



* 当向持有人发行时，发行人会在 SD-JWT 中包含所有相关披露

* 当向验证者呈现时，持有者仅发送 SD-JWT 中选定的一组披露信息
* 

在SD-JWT中，原始VC（由颁发者签发）包含所有声明（claims）的哈希承诺（即_sd数组），而每个声明的原始值、盐值以及声明名称则被单独存储，以便在需要时进行选择性披露。

当持有者（holder）想要向验证者（verifier）出示VC中的部分声明时，需要从原始VC中提取出要披露的声明及其盐值。



### 具体步骤如下：

在颁发者创建SD-JWT时，会生成每个声明的披露描述符（即[salt, claim_key, claim_value]），但不会将它们包含在签名的JWT中。相反，签名的JWT中只包含这些声明的哈希值。然后，颁发者会将签名的JWT和所有披露描述符（以base64url编码的字符串形式）一起发送给持有者。但是，持有者可以选择在出示时只提供部分披露描述符。

1. 持有者如何提取需披露的声明及其盐值？

​    \- 持有者从颁发者那里收到的原始SD-JWT VC通常是一个字符串，其中包含：

​        <签名的JWT>~<披露1>~<披露2>~...~<披露n>（每个披露都是base64url编码的字符串，以波浪符分隔）

​    \- 持有者需要解析这个字符串，将其拆分为：

​        \- 签名的JWT（第一部分）

​        \- 一系列披露字符串（以~分隔的其余部分）

​       \- 每个披露字符串（disclosure）是base64url编码的JSON数组，解码后得到数组，例如：[salt, key, value] 或者 [salt, key]（对于布尔声明）或者对于数组元素可能是不同的结构。

​     \- 持有者可以根据自己的策略，选择要披露的声明，即选择哪些披露字符串（disclosures）包含在将要出示给验证者的VP中。

2. 提取特定声明的盐值和原始值：

​    \- 持有者需要知道每个声明的密钥（claim key）才能选择。例如，持有者可能只想披露"name"声明，那么他需要找到包含"name"的披露字符串。

​    \- 方法：遍历所有的披露字符串，解码每个披露字符串（base64url解码），然后解析为JSON数组。检查数组的第二个元素（索引为1）是否等于要披露的声明键（如"name"）。如果匹配，那么这个披露字符串就包含该声明的盐值（数组第一个元素）和原始值（数组第三个元素）。

​    \- 然后，持有者将这个披露字符串（base64url编码的原始形式）与签名的JWT一起组合成新的VP字符串，格式为：<签名的JWT>~<选定的披露字符串>（多个披露字符串用~连接）

3. 注意：有些声明可能直接在签名的JWT中（公开声明），而不在披露字符串中。这些公开声明（如iss、iat等）总是会被披露。







有两个安全策略：https://www.w3.org/TR/vc-data-model-2.0/#securing-mechanisms

两类 安全机制：使用封装证明的机制(enveloping )和使用嵌入式证明的机制(embedded )  



### [Verifiable Presentations](https://www.w3.org/TR/vc-data-model-2.0/#verifiable-presentations)

可以拥有多个VC: [verifiable credentials](https://www.w3.org/TR/vc-data-model-2.0/#dfn-verifiable-credential).

包含的属性有：

**id**：可选的   `id`的 值*必须*是一个URL [ [RFC2397](https://www.rfc-editor.org/rfc/rfc2397) ]

**type**： *必须存在*

**verifiableCredential**： 可选的

**holder**： 可选的



## 使用封装证明的机制(enveloping )

* 封装VP有两种方式：  **EnvelopedVerifiableCredential** 和 **EnvelopedVerifiablePresentation**

### EnvelopedVerifiableCredential  ==》 

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://www.w3.org/ns/credentials/examples/v2"
  ],
  "exp": 1746804372,
  "iat": 1745594772,
  "type": "VerifiablePresentation",
  "verifiableCredential": [
    {
      "@context": "https://www.w3.org/ns/credentials/v2",
      "id": "data:application/vc+sd-jwt, eyJraWQiOiJFeEhrQk1XOWZtYmt2VjI2Nm1ScHVQMnNVWV9OX0VXSU4xbGFwVXpPOHJvIiwiYWxnIjoiRVMyNTYifQ.eyJfc2RfYWxnIjoic2hhLTI1NiIsIkBjb250ZXh0IjpbImh0dHBzOi8vd3d3LnczLm9yZy9ucy9jcmVkZW50aWFscy92MiIsImh0dHBzOi8vd3d3LnczLm9yZy9ucy9jcmVkZW50aWFscy9leGFtcGxlcy92MiJdLCJpc3N1ZXIiOiJodHRwczovL3VuaXZlcnNpdHkuZXhhbXBsZS9pc3N1ZXJzLzU2NTA0OSIsInZhbGlkRnJvbSI6IjIwMTAtMDEtMDFUMTk6MjM6MjRaIiwiY3JlZGVudGlhbFNjaGVtYSI6eyJfc2QiOlsiNjVFLVZZbmE3UE5mSGVsUDN6THFwcE5ERXhSLWhjWkhSTnlxN2U0ZVdabyIsIjhJbEwtUGx4Ukt3S0hLaTMtTXhXMjM4d0FkTmQ0NHdabC1iY3NBc2JIQjAiXX0sImNyZWRlbnRpYWxTdWJqZWN0Ijp7ImRlZ3JlZSI6eyJuYW1lIjoiQmFjaGVsb3Igb2YgU2NpZW5jZSBhbmQgQXJ0cyIsIl9zZCI6WyJMVXhqcWtsWS1hdDVSVmFoSXpxM3NJZ015dkdwVDlwdlUwdTRyU2ktMXl3Il19LCJfc2QiOlsiVmxZLW50ZklPOUI5RGRsUWp5U2REMldoVWI0bjc3Zl9HWDZ2U1dLQWpCNCJdfSwiX3NkIjpbIi1iREZ4Um94UUVlcEdjZFl6a250aTVGWXBsUTU5N0djaEdUTGVtLVJSY1UiLCJfREFVZ0xrTF9zVkVtLTBvcE8zaWhpeVFhS0ZzT08xUl9ONk1CUmprOWhFIl19.Kc083RKbBxc3Vr5qR3iEEPp3dKxTa6sPaWNsqtkIw8TvMRf9EZL2ajtgkWSBYzyzOzawOrCXryyp4rMTyI9vfA ~WyJiQ1RTaU9HNUo1VXhPY1QwUlNfd01nIiwgImlkIiwgImh0dHA6Ly91bml2ZXJzaXR5LmV4YW1wbGUvY3JlZGVudGlhbHMvMTg3MiJd~WyJTclNWMS01SjR6cWhOU3N3STIwaHdRIiwgInR5cGUiLCBbIlZlcmlmaWFibGVDcmVkZW50aWFsIiwgIkV4YW1wbGVBbHVtbmlDcmVkZW50aWFsIl1d~WyJKX294dDhtUGUtaDl4MkQzc29uT1N3IiwgImlkIiwgImh0dHBzOi8vZXhhbXBsZS5vcmcvZXhhbXBsZXMvZGVncmVlLmpzb24iXQ~WyJDMlpWektmZ185RUh1ajB2S1ExdWJnIiwgInR5cGUiLCAiSnNvblNjaGVtYSJd~WyJ6Szd5QlFPbFhfX2Q0X0VoYUc0Y0pRIiwgImlkIiwgImRpZDpleGFtcGxlOjEyMyJd~WyJ6b1pzRzMzeXBMeVRGMm9aS3ZmMVFnIiwgInR5cGUiLCAiQmFjaGVsb3JEZWdyZWUiXQ~",
      "type": "EnvelopedVerifiableCredential"
    }
  ]
}
```

* verifiableCredential 是一个数组形式，可以有多个vc，每一个EnvelopedVerifiableCredential都是一个vc

#### 将上面的 verifiableCredential.id 内容解码后：

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://www.w3.org/ns/credentials/examples/v2"
  ],
  "credentialSchema": {
    "id": "https://example.org/examples/degree.json",
    "type": "JsonSchema"
  },
  "credentialSubject": {
    "degree": {
      "name": "Bachelor of Science and Arts",
      "type": "BachelorDegree"
    },
    "id": "did:example:123"
  },
  "id": "http://university.example/credentials/1872",
  "issuer": "https://university.example/issuers/565049",
  "type": [
    "VerifiableCredential",
    "ExampleAlumniCredential"
  ],
  "validFrom": "2010-01-01T19:23:24Z"
}
```



### EnvelopedVerifiablePresentation ===》

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://www.w3.org/ns/credentials/examples/v2"
  ],
  "exp": 1746804372,
  "iat": 1745594772,
  "id": "data:application/vp+sd-jwt, eyJhbGciOiJFUzM4NCIsImtpZCI6IlVRTV9fblE0UzZCTzhuUTRuT05YeHB4aHRob3lOeGI1M0xZZ1l6LTJBQnMiLCJ0eXAiOiJ2cCtsZCtqc29uK3NkLWp3dCIsImN0eSI6InZwK2xkK2pzb24ifQ.eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvbnMvY3JlZGVudGlhbHMvdjIiLCJodHRwczovL3d3dy53My5vcmcvbnMvY3JlZGVudGlhbHMvZXhhbXBsZXMvdjIiXSwidmVyaWZpYWJsZUNyZWRlbnRpYWwiOlt7IkBjb250ZXh0IjpbImh0dHBzOi8vd3d3LnczLm9yZy9ucy9jcmVkZW50aWFscy92MiIsImh0dHBzOi8vd3d3LnczLm9yZy9ucy9jcmVkZW50aWFscy9leGFtcGxlcy92MiJdLCJpc3N1ZXIiOiJodHRwczovL3VuaXZlcnNpdHkuZXhhbXBsZS9pc3N1ZXJzLzU2NTA0OSIsInZhbGlkRnJvbSI6IjIwMTAtMDEtMDFUMTk6MjM6MjRaIiwiY3JlZGVudGlhbFN1YmplY3QiOnsiYWx1bW5pT2YiOnsibmFtZSI6IkV4YW1wbGUgVW5pdmVyc2l0eSIsIl9zZCI6WyJoek9LRzU2cDI5c1ByTGFDNUE4RndFdUczVU05dUlZU1p1cU9YczJlVGJBIl19LCJfc2QiOlsiWVdXVmVDRndxQmk4WDBqSF9jV0NWWU16STNhOHBjTEVYRWZicFNSQVlndyJdfSwiX3NkIjpbIjJJZjhhaUs4REZwVWJ4dEc1cGMwel9SaFJzbm1ybGFRMEhzcTk4WFNyYWsiLCJUeDZ4ZWZMVUdUZUpfYWtVUFdGeHNvbUhobGtWVnpfNzVoaVZ6eWpyYmVzIl19XSwiX3NkIjpbIjd2anl0VVN3ZEJ0MXQ5RktlOVFfS3JIRXhFWGxrTEFaTzBKM0Jpd200dlkiXSwiX3NkX2FsZyI6InNoYS0yNTYiLCJpYXQiOjE3MDY1NjI4NDksImV4cCI6MTczODE4NTI0OSwiY25mIjp7Imp3ayI6eyJrdHkiOiJFQyIsImNydiI6IlAtMzg0IiwiYWxnIjoiRVMzODQiLCJ4IjoidWtEd1U2ZzlQUVRFUWhYaEgyckRZNndMQlg3UHFlUjZBcGlhVHBEUXowcl8tdDl6UXNxem54Z0hEcE5oekZlQyIsInkiOiJMQnhVYnBVdFNGMVVKVTVpYnJIdkpINjBUSG5YMk1xa0xHZGltU1l0UGR4RlkxOEdhcldiS3FZV0djUkZHVE9BIn19fQ.kYD63YtBNYnLUTw6Szf1vs_Ug3UBXhPwCyqpNmPnPDa3rXZQhQLdB1BgaoO8zgQ-c3B41fxaXMnLHYV9-B20uboSpJP0B-2Vre917eQt1cSDswDGA_Ytvn4BSqYVBB2J~WyJFMkFsRzhsY2p0QVFrcllIbjlIbnVRIiwgInR5cGUiLCAiVmVyaWZpYWJsZVByZXNlbnRhdGlvbiJd~WyI5NldYMDRneno4cVZzOVZLU2wwYTVnIiwgImlkIiwgImh0dHA6Ly91bml2ZXJzaXR5LmV4YW1wbGUvY3JlZGVudGlhbHMvMTg3MiJd~WyJaekU2VFVaamtHMW1DWXBKMEhnc0l3IiwgInR5cGUiLCBbIlZlcmlmaWFibGVDcmVkZW50aWFsIiwgIkV4YW1wbGVBbHVtbmlDcmVkZW50aWFsIl1d~WyItQ3NsS25GZGFYb2JiQWsyU0JBVGR3IiwgImlkIiwgImRpZDpleGFtcGxlOmViZmViMWY3MTJlYmM2ZjFjMjc2ZTEyZWMyMSJd~WyJuRm1OWl9IczB3WWNoOFdkeTdnQUNRIiwgImlkIiwgImRpZDpleGFtcGxlOmMyNzZlMTJlYzIxZWJmZWIxZjcxMmViYzZmMSJd~",
  "type": "EnvelopedVerifiablePresentation"
}
```

* EnvelopedVerifiablePresentation得id解码后是VP,VP中可包含多个vc

#### 将 id进行解码后得：

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://www.w3.org/ns/credentials/examples/v2"
  ],
  "cnf": {
    "jwk": {
      "alg": "ES384",
      "crv": "P-384",
      "kty": "EC",
      "x": "ukDwU6g9PQTEQhXhH2rDY6wLBX7PqeR6ApiaTpDQz0r_-t9zQsqznxgHDpNhzFeC",
      "y": "LBxUbpUtSF1UJU5ibrHvJH60THnX2MqkLGdimSYtPdxFY18GarWbKqYWGcRFGTOA"
    }
  },
  "exp": 1738185249,
  "iat": 1706562849,
  "type": "VerifiablePresentation",
  "verifiableCredential": [
    {
      "@context": [
        "https://www.w3.org/ns/credentials/v2",
        "https://www.w3.org/ns/credentials/examples/v2"
      ],
      "credentialSubject": {
        "alumniOf": {
          "id": "did:example:c276e12ec21ebfeb1f712ebc6f1",
          "name": "Example University"
        },
        "id": "did:example:ebfeb1f712ebc6f1c276e12ec21"
      },
      "id": "http://university.example/credentials/1872",
      "issuer": "https://university.example/issuers/565049",
      "type": [
        "VerifiableCredential",
        "ExampleAlumniCredential"
      ],
      "validFrom": "2010-01-01T19:23:24Z"
    }
  ]
}
```

* 解析结果 这是一个 VP对象，包括多个vc



## 使用嵌入式证明的机制(embedded )  ：

```json
{
    "@context": [
        "https://www.w3.org/2018/credentials/v1"
    ],
    "holder": "did:peer:1zQmQuC3q1WiFfPNhsfpbmU5mhi9CkQzWpWc64v11qdzeoDF",
    "proof": {
        "capabilityChain": [
            {
                "curve": "BN254",
                "hashAlgorithm": "MiMC",
                "proofSystem": "groth16"
            }
        ],
        "created": "2025-07-04T09:50:49.6518741+08:00",
        "proofPurpose": "authentication",
        "proofValue": "VdJQ3ecGdOtbsENGwxRCUqDFWyVyvzH1Z7AHhUWegesSf-JQrzQJuLAOxhbtIRyLy2yger64Y5cuL9wGgfUwBA",
        "type": "Ed25519Signature2018",
        "verificationMethod": "#key-1"
    },
    "type": "VerifiablePresentation",
    "verifiableCredential": [
        {
            "@context": [
                "https://www.w3.org/2018/credentials/v1"
            ],
            "credentialSubject": {
                "address": "北京市朝阳区",
                "age": 30,
                "birthDate": "1995-01-01",
                "gender": "男",
                "id": "did:peer:1zQmQuC3q1WiFfPNhsfpbmU5mhi9CkQzWpWc64v11qdzeoDF",
                "name": "张三"
            },
            "id": "urn:uuid:123",
            "issuanceDate": "2025-07-04T09:50:48.677054+08:00",
            "issuer": "did:peer:1zQmNNopsgeVXcVQ7gjD6oHNt8WeTFy88VygmTJUjqYRTZJu",
            "proof": {
                "created": "2025-07-04T09:50:48.677054+08:00",
                "proofPurpose": "assertionMethod",
                "proofValue": "-XcCQVf8JsHRGxIK0VsVlrjceWXvdDrvGV6m55ulaf2Cf5HCLv2zyCjLpEg8EE18rTkSLMjd1zRTrB4tDaF5Dw",
                "type": "Ed25519Signature2018",
                "verificationMethod": "#key-1"
            },
            "type": "VerifiableCredential"
        }
    ]
}
```





## 将上面封装证明的机制(enveloping )和使用嵌入式证明的机制(embedded ) 融合

#### 第一种： 将embedded 加入到enveloping , 主体是embedded 

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://www.w3.org/ns/credentials/examples/v2"
  ],
  "holder": "did:example:12345678",
  "id": "urn:uuid:313801ba-24b7-11ee-be02-ff560265cf9b",
  "proof": {
    "type": "Ed25519Signature2020",
    "created": "2025-07-03T07:53:33Z",
    "verificationMethod": "did:example:12345678#key-1",
    "proofPurpose": "authentication",
    "signatureValue": "G5eCuuXYCyEG8t4O70-s479lqkmG_ILJrLorvcI1WCSWAYJgbfzMwzBRQHEJrpc5IvOCu0yzbuWEQWRTxA45Cw"
  },
  "type": [
    "VerifiablePresentation",
    "ExamplePresentation"
  ],
  "verifiableCredential": [
    {
      "@context": "https://www.w3.org/ns/credentials/v2",
      "type": "EnvelopedVerifiableCredential",
      "id": "data:application/vc+sd-jwt,eyJhbGciOiJFUzI1NiIsInR5cCI6ImFwcGxpY2F0aW9uL2pzb24rc2Qtand0In0.eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvMjAxOC9jcmVkZW50aWFscy92MSJdLCJpZCI6Imh0dHBzOi8vc2Nob29sLmV4YW1wbGUuZWR1L2NyZWRlbnRpYWxzLzIwMjMwMDEiLCJ0eXBlIjpbIlZlcmlmaWFibGVDcmVkZW50aWFsIiwiU2Nob29sQ3JlZGVudGlhbCJdLCJpc3N1ZXIiOiJkaWQ6ZXhhbXBsZTpzY2hvb2wiLCJpc3N1YW5jZURhdGUiOiIyMDI1LTA3LTAzVDE1OjUzOjMzKzA4OjAwIiwiY3JlZGVudGlhbFN1YmplY3QiOnsiaWQiOiJkaWQ6ZXhhbXBsZTpzdHVkZW50OjAwMSJ9LCJfc2QiOlsiV2tGYVlpMXlWV1Z1ZEhoamJHZFFTMlJDYzFCdlNGTmthV2xYVTFkS1gzRnhPVGR6UzB3M01tMVRWUSIsIldWUkdZVkJZVDJsS2JEY3lUWFZ3UVVWblZFZFNUM1pETFdwblIyMWlkek5NZDNneFJtVk5jbXA1TUEiLCJhQzA0Y21aaExVUk1lRlJ6ZUZwbGMxOVZibWxMYlMwNGMwcHVVa3d5WWtFdGRFZGhaemh2UzBoWlp3IiwiV0RaRWIycEtYemxqWWt4dFZuSndNVTR6UzFOV1RHTnRXRk5hVEcxTlNYaDRNbTVOUW5SS1IyNVFSUSIsIlNVRkNTMWRCYW5sSGFVWnhXSFJ5TWtjdGQyOVNTRzF4YzJacWNVbERiMEptYVVsSGFrbGpTakpWTUEiXSwiY25mIjp7Imp3ayI6eyJjcnYiOiJQLTI1NiIsImtpZCI6IldGM3plWEFHQms2OUNPVkN2RzhrYTNMWmxuOXcxY2hDWWw4MnZYbzk2MXMiLCJrdHkiOiJFQyIsIngiOiJhS3U3RndYclhRRWlXdHlFSE1tZ0h4WHI3LXhkVGNEUTFUVUYtbXlseFJNIiwieSI6IkVOSWZ0OVFvMzlYbmxXcEc4T0RrUTVJSnNuQk5hYU40cElTRzlwWDdZejgifX0sIm5vbmNlIjoiNDFUR0Q5UnFKbks1ZFFEdjBFMWlOQSIsIl9zZF9hbGciOiJzaGEtMjU2In0.9bnGX21Dv8rTydJNq9K3n9kYLkEG56nykow2KOwE2GcfGdXixlJeqsJoyGhnNQDDbpv0gFsa6magSkoiWT0lUA~WyJiQVM0QVVXQjMzY3NvQUphaDluMEd3Iiwic3R1ZGVudF9pZCIsIjIwMjMwMDEiXQ~WyJnRGEwc3Mwd21KTTJCOFkyX19nNVRRIiwibWFqb3IiLCLorqHnrpfmnLrnp5HlrabkuI7mioDmnK8iXQ~"
    },
    {
      "@context": [
        "https://www.w3.org/ns/credentials/v2",
        "https://www.w3.org/ns/credentials/examples/v2"
      ],
      "credentialSubject": {
        "assertion": "This VP is submitted by the subject as evidence of a legal right to drive",
        "id": "urn:uuid:313801ba-24b7-11ee-be02-ff560265cf9b"
      },
      "issuer": "did:example:12345678",
      "proof": {
        "type": "Ed25519Signature2020",
        "created": "2025-07-03T07:53:33Z",
        "verificationMethod": "did:example:12345678#key-1",
        "proofPurpose": "assertionMethod",
        "signatureValue": "_wlH3zeWIRtc7q7VXEdUYLyLPKrqO5VALfjyrNKUEywX8sgaLwlMvgp_S0zk-e-lclb-hIptb-Et7XeB_t9wAA"
      },
      "type": [
        "VerifiableCredential",
        "ExampleAssertCredential"
      ]
    }
  ]
}
```



#### 第二种： 将enveloping 加入到embedded , 主体是enveloping 

```json
{
  "@context": [
    "https://www.w3.org/ns/credentials/v2",
    "https://www.w3.org/ns/credentials/examples/v2"
  ],
  "exp": 1746804372,
  "iat": 1745594772,
  "id": "data:application/vp+sd-jwt, eyJhbGciOiJFUzM4NCIsImtpZCI6IlVRTV9fblE0UzZCTzhuUTRuT05YeHB4aHRob3lOeGI1M0xZZ1l6LTJBQnMiLCJ0eXAiOiJ2cCtsZCtqc29uK3NkLWp3dCIsImN0eSI6InZwK2xkK2pzb24ifQ.eyJAY29udGV4dCI6WyJodHRwczovL3d3dy53My5vcmcvbnMvY3JlZGVudGlhbHMvdjIiLCJodHRwczovL3d3dy53My5vcmcvbnMvY3JlZGVudGlhbHMvZXhhbXBsZXMvdjIiXSwidmVyaWZpYWJsZUNyZWRlbnRpYWwiOlt7IkBjb250ZXh0IjpbImh0dHBzOi8vd3d3LnczLm9yZy9ucy9jcmVkZW50aWFscy92MiIsImh0dHBzOi8vd3d3LnczLm9yZy9ucy9jcmVkZW50aWFscy9leGFtcGxlcy92MiJdLCJpc3N1ZXIiOiJodHRwczovL3VuaXZlcnNpdHkuZXhhbXBsZS9pc3N1ZXJzLzU2NTA0OSIsInZhbGlkRnJvbSI6IjIwMTAtMDEtMDFUMTk6MjM6MjRaIiwiY3JlZGVudGlhbFN1YmplY3QiOnsiYWx1bW5pT2YiOnsibmFtZSI6IkV4YW1wbGUgVW5pdmVyc2l0eSIsIl9zZCI6WyJoek9LRzU2cDI5c1ByTGFDNUE4RndFdUczVU05dUlZU1p1cU9YczJlVGJBIl19LCJfc2QiOlsiWVdXVmVDRndxQmk4WDBqSF9jV0NWWU16STNhOHBjTEVYRWZicFNSQVlndyJdfSwiX3NkIjpbIjJJZjhhaUs4REZwVWJ4dEc1cGMwel9SaFJzbm1ybGFRMEhzcTk4WFNyYWsiLCJUeDZ4ZWZMVUdUZUpfYWtVUFdGeHNvbUhobGtWVnpfNzVoaVZ6eWpyYmVzIl19XSwiX3NkIjpbIjd2anl0VVN3ZEJ0MXQ5RktlOVFfS3JIRXhFWGxrTEFaTzBKM0Jpd200dlkiXSwiX3NkX2FsZyI6InNoYS0yNTYiLCJpYXQiOjE3MDY1NjI4NDksImV4cCI6MTczODE4NTI0OSwiY25mIjp7Imp3ayI6eyJrdHkiOiJFQyIsImNydiI6IlAtMzg0IiwiYWxnIjoiRVMzODQiLCJ4IjoidWtEd1U2ZzlQUVRFUWhYaEgyckRZNndMQlg3UHFlUjZBcGlhVHBEUXowcl8tdDl6UXNxem54Z0hEcE5oekZlQyIsInkiOiJMQnhVYnBVdFNGMVVKVTVpYnJIdkpINjBUSG5YMk1xa0xHZGltU1l0UGR4RlkxOEdhcldiS3FZV0djUkZHVE9BIn19fQ.kYD63YtBNYnLUTw6Szf1vs_Ug3UBXhPwCyqpNmPnPDa3rXZQhQLdB1BgaoO8zgQ-c3B41fxaXMnLHYV9-B20uboSpJP0B-2Vre917eQt1cSDswDGA_Ytvn4BSqYVBB2J~WyJFMkFsRzhsY2p0QVFrcllIbjlIbnVRIiwgInR5cGUiLCAiVmVyaWZpYWJsZVByZXNlbnRhdGlvbiJd~WyI5NldYMDRneno4cVZzOVZLU2wwYTVnIiwgImlkIiwgImh0dHA6Ly91bml2ZXJzaXR5LmV4YW1wbGUvY3JlZGVudGlhbHMvMTg3MiJd~WyJaekU2VFVaamtHMW1DWXBKMEhnc0l3IiwgInR5cGUiLCBbIlZlcmlmaWFibGVDcmVkZW50aWFsIiwgIkV4YW1wbGVBbHVtbmlDcmVkZW50aWFsIl1d~WyItQ3NsS25GZGFYb2JiQWsyU0JBVGR3IiwgImlkIiwgImRpZDpleGFtcGxlOmViZmViMWY3MTJlYmM2ZjFjMjc2ZTEyZWMyMSJd~WyJuRm1OWl9IczB3WWNoOFdkeTdnQUNRIiwgImlkIiwgImRpZDpleGFtcGxlOmMyNzZlMTJlYzIxZWJmZWIxZjcxMmViYzZmMSJd~",
  "type": "EnvelopedVerifiablePresentation"
}

```



| 字段名             | 字段                       | 类型       | 必传 | 备注                                                                 |
|--------------------|----------------------------|------------|------|----------------------------------------------------------------------|
| 上下文             | `@context`                 | 数组       | 是   | 定义了凭证使用的上下文（命名空间）。                                 |
| ID                 | `id`                       | 字符串     | 是   | 凭证的唯一标识符。                                                   |
| 类型               | `type`                     | 数组       | 是   | 凭证类型，包括 `VerifiableCredential` 和 `DIDInformationCredential`。|
| 签发者             | `issuer`                   | 字符串     | 是   | 签发者的去中心化标识符（DID）。                                      |
| 凭证主体           | `credentialSubject`        | 对象       | 是   | 被签发凭证的实体信息。                                               |
| 凭证主体 ID        | `credentialSubject.id`     | 字符串     | 是   | 凭证主体的去中心化标识符（DID）。                                    |
| 用户类型           | `credentialSubject.info.userType` | 字符串 | 否   | 用户类型（例如 Corporate）。                                         |
| 公司名称           | `credentialSubject.info.companyName` | 字符串 | 否   | 公司名称（例如 ASL）。                                               |
| 地址               | `credentialSubject.info.address` | 字符串 | 否   | 公司地址（例如 16/F Shek Mun）。                                     |
| 签名信息           | `proof`                    | 对象       | 是   | 签名信息用于验证凭证的真实性。                                       |
| 签名类型           | `proof.type`               | 字符串     | 是   | 签名类型（例如 `DataIntegrityProof`）。                              |
| 创建时间           | `proof.created`            | 字符串     | 是   | 签名创建时间，采用 ISO 8601 格式（例如 `2025-04-27T17:58:33Z`）。     |
| 验证方法           | `proof.verificationMethod` | 字符串     | 是   | 签发者使用的验证方法（例如 DID 公钥 ID）。                           |
| 加密套件           | `proof.cryptosuite`        | 字符串     | 是   | 使用的加密算法（例如 `ecdsa-rdfc-2019`）。                           |
| 签名用途           | `proof.proofPurpose`       | 字符串     | 是   | 签名用途（例如 `assertionMethod`）。                                 |
| 签名值             | `proof.proofValue`         | 字符串     | 是   | 实际的签名值。                                                       |

### 凭证主体 (credentialSubject)

| 字段名             | 字段                       | 类型       | 必传 | 备注                                                                 |
|--------------------|----------------------------|------------|------|----------------------------------------------------------------------|
| ID                 | `id`                       | 字符串     | 是   | 凭证主体的去中心化标识符（DID）。                                    |
| 用户类型           | `info.userType`            | 字符串     | 否   | 用户类型（例如 Corporate）。                                         |
| 公司名称           | `info.companyName`         | 字符串     | 否   | 公司名称（例如 ASL）。                                               |
| 地址               | `info.address`             | 字符串     | 否   | 公司地址（例如 16/F Shek Mun）。                                     |

### 签名信息 (proof)

| 字段名             | 字段                       | 类型       | 必传 | 备注                                                                 |
|--------------------|----------------------------|------------|------|----------------------------------------------------------------------|
| 签名类型           | `type`                     | 字符串     | 是   | 签名类型（例如 `DataIntegrityProof`）。                              |
| 创建时间           | `created`                  | 字符串     | 是   | 签名创建时间，采用 ISO 8601 格式（例如 `2025-04-27T17:58:33Z`）。     |
| 验证方法           | `verificationMethod`       | 字符串     | 是   | 签发者使用的验证方法（例如 DID 公钥 ID）。                           |
| 加密套件           | `cryptosuite`              | 字符串     | 是   | 使用的加密算法（例如 `ecdsa-rdfc-2019`）。                           |
| 签名用途           | `proofPurpose`             | 字符串     | 是   | 签名用途（例如 `assertionMethod`）。                                 |
| 签名值             | `proofValue`               | 字符串     | 是   | 实际的签名值。                                                       |

