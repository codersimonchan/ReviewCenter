## What is MongoDB？

MongoDB is a database, and it is so-called NoSQL database. It contains lots of collections, you can think of a collection as a table of data in MySQL. And each collections contains more documents. In relational database, a document would be a row in a table like MySQL. So, each document contains the data with about one single entity. Now you get the point, right?



## Compass和Mongosh

MongoDB Compass 是 MongoDB 提供的一款图形化管理工具，它可以用于连接和管理 MongoDB 数据库，包括本地部署的 MongoDB 实例以及 MongoDB Atlas 上托管的 MongoDB 数据库。MongoDB Compass 可以非常方便地连接到 MongoDB Atlas，以图形化方式管理和操作 Atlas 上的数据库。

Compass 是一个 GUI 管理工具，适用于所有类型的 MongoDB 部署，而 Atlas 是一个托管的云数据库服务。通过使用 Compass 连接到 Atlas，用户可以方便地管理和操作云端的 MongoDB 数据库，从而提升开发和运维效率。



定义索引配置。例如，对于一个包含 `title` 和 `description` 字段的集合，索引配置可能如下：

```
{
  "mappings": {
    "dynamic": false,
    "fields": {
      "title": {
        "type": "string",
        "analyzer": "lucene.standard"
      },
      "description": {
        "type": "string",
        "analyzer": "lucene.standard"
      }
    }
  }
}
```

使用 MongoDB 聚合框架来执行搜索查询。例如：

```
db.products.aggregate([
  {
    $search: {
      "text": {
        "query": "apple",
        "path": ["title", "description"]
      }
    }
  }
]);
```





MongoDB Atlas Search 和 MongoDB 内置的全文搜索（Full-Text Search）都有相似的功能，但它们有一些关键的区别。以下是对两者的详细比较：

## MongoDB Full-Text Search

2. - **MongoDB 的全文搜索**并不是模糊搜索，它基于**精确匹配**进行工作。在全文搜索中，MongoDB 会对文本字段进行分词，然后对分词后的词语进行精确匹配。所以，它更类似于在**多个字段中查找与搜索词精确匹配**的内容，而不是像模糊搜索那样查找部分匹配或近似匹配的内容。

     ### **全文搜索的工作方式：**

     1. **分词处理**：MongoDB 会根据语言特性，将文本字段的内容分解为独立的词。例如，`"This is an order"` 会被拆分为 `This`、`is`、`an`、`order` 四个词。
     2. **精确匹配**：在搜索时，MongoDB 会查找与你的搜索词完全匹配的分词结果。比如，搜索 `order` 会匹配到包含这个词的文档，但搜索 `orde` 或 `orders` 可能就不会匹配。
   
     ### **不是模糊搜索：**
   
     MongoDB 的全文搜索并不会进行以下类型的模糊匹配：
   
     - **部分匹配**：如果你搜索 `12345`，全文搜索不会匹配 `123456` 或 `12345678`。
     - **近似匹配**：搜索 `dog` 不会匹配到 `dogs`、`doggy` 等类似词语，除非这些形式也作为独立的分词存在。
   
     ### 具体流程是这样的：
   
     1. **创建全文索引**：首先你需要为 `title` 和 `description` 字段创建全文索引。MongoDB 的全文搜索依赖于这些索引。
   
        ```
        db.collection.createIndex({ title: "text", description: "text" })
        ```
   
     2 .**执行全文搜索**：接着，当你执行搜索时，MongoDB 会在**带有全文索引的字段中**查找匹配的文本。假设你搜索的是 `order`，MongoDB 会在 `title` 和 `description` 两个字段中查找。
   
     3.当你使用 `$text: { $search: "order" }` 搜索时，MongoDB 会检查这两个文档的 `title` 和 `description` 字段。无论是 `title` 还是 `description` 中有 "order"，这两个文档都会被搜索结果返回。

### 注意事项：

- **多个字段**：全文搜索在所有带有全文索引的字段中并行进行搜索，只要有任意一个字段匹配关键词，文档就会被返回。
- **相关性评分**：MongoDB 会根据匹配的字段和词的出现频率为文档生成一个相关性评分，默认情况下，返回的文档会按相关性排序。如果你想更精确地控制哪些字段权重更高，可以为不同字段设置权重。



## MongoDB Atlas Search

**特点**：

1. **基于 Lucene**：Atlas Search 基于 Apache Lucene [luːˈsiːn] 构建，Lucene 是一个强大的开源全文搜索引擎库，提供了丰富的搜索功能和性能优化。

2. **全面集成**：作为 MongoDB Atlas (是基于 MongoDB 数据库技术的云服务，提供了一种托管和管理 MongoDB 数据库的解决方案)的一部分，Atlas Search 提供了一个全面托管的搜索解决方案。

3. 高级功能：

   - 提供复杂的搜索查询，如模糊搜索、地理搜索、自动完成、多语言支持等。
   - 支持细粒度的索引管理和配置，包括字段映射和分析器。
   - 支持聚合查询和多条件组合搜索。

