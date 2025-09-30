# JSON-LD



JSON-LD 定义了一些核心关键字（以 `@` 开头），用于描述数据的结构和语义：

| **关键字**   | **作用**                                                     |
| :----------- | :----------------------------------------------------------- |
| `@context`   | 告诉 JSON-LD 使用的标准（例如 Schema.org）。                 |
| `@type`      | 定义数据的类型（如产品、事件、人物等）。                     |
| `@id`        | 提供数据的唯一标识符（可选）。                               |
| `@value`     | 定义属性的具体值。                                           |
| `@language`  | 定义文本的语言（例如 `en` 表示英语）。                       |
| **`@graph`** | 允许你在同一个 JSON-LD 数据块内，声明多个实体（例如组织、产品、评论等）之间的关联。 |
| @nest        | **嵌套属性**（Nested Properties）特性通过`@nest`关键字实现属性的逻辑分组 |
| @container   | JSON-LD 1.1大幅扩展了**容器类型**（Container Type）系统，为复杂数据结构的语义建模提供了更强大的工具。新增的容器类型包括`@id`容器、`@type`容器、`@graph`容器等，每种都针对特定的数据组织模式进行了优化。 |
|              |                                                              |

具体请参考： https://w3c.github.io/json-ld-syntax/#syntax-tokens-and-keywords

https://www.w3.org/TR/json-ld11-api/#remote-document-and-context-retrieval

### 1. JSON-LD 的数据类型和属性

JSON-LD 结构化数据基于 [Schema.org](https://schema.org/) 词汇表，支持大量 `@type` 类型



```
https://schema.org
http://www.w3.org/2001/XMLSchema
https://www.w3.org/2018/credentials
```

### 2. JSON-LD 的嵌套数据结构

JSON-LD 支持嵌套数据结构，可以用于描述复杂的关系。例如，一个 `Product` 可以嵌套 `offers` 属性，而 `offers` 本身又是一个对象。

#### 示例：嵌套结构

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "示例产品",
  "description": "这是一个示例产品的描述。",
  "offers": {
    "@type": "Offer",
    "price": "99.99",
    "priceCurrency": "USD",
    "availability": "https://schema.org/InStock"
  }
}
</script>
```

### 3. JSON-LD 数组的使用

JSON-LD 支持数组，可以用于描述多个值或多个对象。例如，一个 `FAQPage` 可以包含多个问题和答案。

#### 示例：数组

```json
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "示例问题 1",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "这是示例问题 1 的答案。"
      }
    },
    {
      "@type": "Question",
      "name": "示例问题 2",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "这是示例问题 2 的答案。"
      }
    }
  ]
}
</script>
```

### 多重类型声明

这反映了现实世界中实体往往具备多重身份的特点

```json
{
  "@context": "http://schema.org",
  "@type": ["Person", "Employee", "SoftwareEngineer"],
  "name": "王五",
  "jobTitle": "高级软件工程师",
  "worksFor": {
    "@type": "Organization",
    "name": "AI科技公司"
  }
}

```



* 王五同时被声明为人、员工和软件工程师三种类型。这种多重类型声明为不同应用场景下的数据查询和推理提供了灵活性。



### **图容器（Graph Container）**



```json
"verifiableCredential": {
          "@id": "https://www.w3.org/2018/credentials#verifiableCredential",
          "@type": "@id",
          "@container": "@graph",
          "@context": null
        }
```

* 当属性被定义为`@container: @graph`时, 其值可以直接写数组

* 上面中 @graph 对应的类型是上面@type类型，即为@id 指定的verifiableCredential

## JSON-LD 实战示例

**BlogPosting schema 示例**（更多详见 BlogPosting - Schema.org Type[3]）：

```json
{
  "@context":"https://schema.org",
"@type":"BlogPosting",
"@id":"https://example.com/blog/my-first-post",
"url":"https://example.com/blog/my-first-post",
"headline":"My First Blog Post",
"image":"https://example.com/images/first-post.jpg",
"datePublished":"2025-05-01T14:30:00Z",
"dateModified":"2025-05-02T10:00:00Z",
"author":{
    "@type":"Person",
    "name":"Jane Doe",
    "url":"https://example.com/author/jane-doe"
},
"publisher":{
    "@type":"Organization",
    "name":"Example Blog",
    "logo":{
      "@type":"ImageObject",
      "url":"https://example.com/images/logo.png"
    }
},
"description":"This is a short summary or excerpt of my first blog post.",
"mainEntityOfPage":{
    "@type":"WebPage",
    "@id":"https://example.com/blog/my-first-post"
}
}
```

•  **`@context`** – 该字段定义了 JSON-LD 的上下文或词汇表。我们将其设置为 `"https://schema.org"`，表示所使用的术语（如 *headline*、*publisher* 等）均来自 Schema.org 的词汇表。 `@context` 实际上是告诉解析器：“请根据 Schema.org 的定义来解释这些键。”

•  **`@type`** – 该字段指明我们正在描述*事物的类型*。此处为 `"BlogPosting"`，这是博客文章的 schema 类型（一种更具体的 Article 子类型）。这一点至关重要：`@type` 就像告诉搜索引擎：“*这个 JSON 对象描述的是一个 BlogPosting（博客文章）。* ” 类似地，在我们的数据中，作者有 `@type: "Person"`，发布者有 `@type: "Organization"` 等。每个 `@type` 值都对应 Schema.org 中该实体的定义。

•  **`@id`** – 该字段为此特定项目提供唯一标识符。在 JSON-LD 中，`@id` 通常是一个 URL (或 URI)，用以唯一标识所描述的实体。在我们的示例中，我们使用博客文章的 URL 作为其 `@id`。这可能看起来与 `url` 字段有些重复（确实，我们在这里将两者设置为了相同的 URL），但 `@id` 有其特殊用途：它允许其他 schema 项目明确地引用此对象。可以将 `@id` 视为数据的永久链接或锚点。例如，如果同一位作者出现在多篇文章中，我们可以为该作者指定一个 `@id`，并在其他地方引用该 `@id` 以将数据链接起来。 *“@id 创建了唯一标识符，而 url 属性则告诉搜索引擎在哪里找到该实体的主页。两者都是必需的……”* 实际上，许多简单的 schema（如上例）会将 `@id` 设置为页面 URL，并且也包含一个 `url` 字段——这同时满足了标识和链接的需求。

* **属性（如 `name`、`price`、`address` 等）**：根据数据类型填写的具体内容。



**BreadcrumbList schema 示例**（更多详见：BreadcrumbList - Schema.org Type[4]）：

```json
{
  "@context":"https://schema.org",
"@type":"BreadcrumbList",
"itemListElement":[
    {
      "@type":"ListItem",
      "position":1,
      "name":"Home",
      "item":"https://example.com/"
    },
    {
      "@type":"ListItem",
      "position":2,
      "name":"Blog",
      "item":"https://example.com/blog/"
    },
    {
      "@type":"ListItem",
      "position":3,
      "name":"My First Blog Post",
      "item":"https://example.com/blog/my-first-post"
    }
]
}
```



### **语义映射**

 `{ "id": "@id", "type": "@type" }` 的结构是 **`@context` 定义中的关键字别名映射**

此结构将普通键名（如 `id`/`type`）映射到 JSON-LD 的保留关键词（如 `@id`/`@type`），**赋予普通 JSON 数据以机器可理解的语义**。
示例：

```json
{
  "@context": {
    "id": "@id",      // 将普通字段 "id" 映射为语义标识符 "@id"
    "type": "@type"   // 将普通字段 "type" 映射为类型声明符 "@type"
  },
  "id": "https://example.com/person/1",
  "type": "Person",
  "name": "Alice"
}
```

* "id": "@id"  声明该字段的值是一个**全局唯一资源标识符** (URI/IRI)  例如： "id": "https://example.com/person/1"

* "type": "@type"	 声明该字段的值表示资源的语义类型（通常来自 Schema.org 等本体）	示例值	"type": "Person"	





## 处理算法与数据转换

### 四大核心算法详解

1. **Context Processing Algorithms（上下文处理算法）**

   ```mermaid
   graph LR
   A[输入JSON数据] --> |检测@context| B:[检测@context]
   B -->|存在| C[加载外部上下文]
   B -->|不存在| D[使用空上下文]
   C --> E[术语解析]
   E --> F[创建语义映射表]
   ```

   

2. **扩展算法**（Expansion Algorithm）是所有其他算法的基础。该算法将带有上下文的紧凑JSON-LD文档转换为完全展开的形式，其中所有术语都被替换为完整的IRI，所有语法糖都被解除

   将简洁的JSON-LD转换为**完全显式化的RDF友好结构**

   ```json
   // 输入（压缩后）
   {
     "@context": {"ex": "http://example.com/"},
     "ex:name": "Alice"
   }
   
   // 输出（扩展后）
   [{
     "@id": "_:b0",  // 自动生成空白节点
     "http://example.com/name": [{"@value": "Alice"}]
   }]
   ```

   - 标准化输出值对象（`"value" → {"@value": "value"}`）

     

3. **压缩算法**（Compaction Algorithm）执行与扩展算法相反的操作，它将展开的JSON-LD文档根据给定的上下文压缩为更简洁的形式

   将扩展后的数据**还原为开发者友好的简洁格式**

   ```json
   // 输入（扩展后）
   [{
     "@id": "http://example.com/person/1",
     "http://xmlns.com/foaf/name": [{"@value": "Alice"}]
   }]
   
   // 输出（压缩后）
   {
     "@context": {"foaf": "http://xmlns.com/foaf/"},
     "@id": "http://example.com/person/1",
     "foaf:name": "Alice"
   }
   ```

   * 替换长链接为短名字，让结构更简洁。

4. **Flattening Algorithm**（扁平化算法   `Flattening` 就像把这个复杂的树或者嵌套结构，**“拍扁”成一个非常简单、清晰的“列表”**。**拆解**嵌套结构，变成一个简单的列表（数组）。

   ```json
   {
     "@context": {
       "name": "http://schema.org/name",
       "spouse": "http://schema.org/spouse",
       "worksFor": "http://schema.org/worksFor",
       "address": "http://schema.org/address",
       "street": "http://schema.org/streetAddress",
       "city": "http://schema.org/addressLocality"
     },
     "@id": "http://example.com/person/123",
     "name": "Alice",
     "spouse": {
       "@id": "http://example.com/person/456",
       "name": "Bob"
     },
     "worksFor": {
       "name": "Tech Corp",
       "address": {
         "street": "123 Main St",
         "city": "Beijing"
       }
     }
   }
   把这个复杂的结构，拆解成一条条独立的、简单的“事实”
   
   [
       {
           "@id": "_:b0",
           "http://schema.org/address": [
               {
                   "@id": "_:b1"
               }
           ],
           "http://schema.org/name": [
               {
                   "@value": "Tech Corp"
               }
           ]
       },
       {
           "@id": "_:b1",
           "http://schema.org/addressLocality": [
               {
                   "@value": "Beijing"
               }
           ],
           "http://schema.org/streetAddress": [
               {
                   "@value": "123 Main St"
               }
           ]
       },
       {
           "@id": "http://example.com/person/123",
           "http://schema.org/name": [
               {
                   "@value": "Alice"
               }
           ],
           "http://schema.org/spouse": [
               {
                   "@id": "http://example.com/person/456"
               }
           ],
           "http://schema.org/worksFor": [
               {
                   "@id": "_:b0"
               }
           ]
       },
       {
           "@id": "http://example.com/person/456",
           "http://schema.org/name": [
               {
                   "@value": "Bob"
               }
           ]
       }
   ]
   ```

   

3. **框架化算法**（Framing Algorithm）是JSON-LD独有的强大特性，它允许开发者定义数据的输出结构模板，实现复杂的数据查询和重组。根据你提供的一个 **模板 (Frame)**，从 JSON-LD 数据中**筛选、提取并重新组织**信息，生成一个**结构完全符合模板要求**的新 JSON 文档。

4. **规范化算法**（Canonicalization Algorithm）基于RDF数据集规范化标准，为JSON-LD文档生成规范化的N-Quads表示，这对于数字签名和数据完整性验证至关重要：

5. 

   

