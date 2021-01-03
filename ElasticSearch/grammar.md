[TOC]

# 导读

ElasticSearch，全文检索工具。以web服务运行的程序，无需安装。本质上是分布式数据库。

ElasticSearch的专业术语在含义上与传统数据库存在对应关系。

| Mysql    | ElasticSearch     |
| -------- | ----------------- |
| Database | Index             |
| Table    | Type(7.0之后废弃) |
| Row      | Document          |
| Column   | Field             |
| Schema   | Mapping           |

# 一、启动ES

```shell
# 启动第一个Node
cd elasticsearch-7.6.2/bin
./elasticsearch

# 启动第二、三个Node
./elasticsearch -Epath.data=data2 -Epath.logs=log2
./elasticsearch -Epath.data=data3 -Epath.logs=log3
```

# 二、查看ES cluster状态

```shell
curl -X GET "localhost:9200/_cat/health?v&pretty"
```

# 三、Index

## 3.1.概念

Index(索引)是单个数据库的同义词，是ES数据管理的顶层单位。ES会对所有Field索引，当存入Field值后会经过处理生成反向索引，查找数据时便直接查找改索引。

## 3.2.列出所有index

```shell
curl -X GET "localhost:9200/_cat/indices?v"
```

## 3.3.创建index

创建名为“twitter”的索引，**index的名字必须小写**。

```shell
curl -X PUT "localhost:9200/twitter?pretty"
```

## 3.4.删除index

删除名为“twitter“的索引

```shell
curl -X DELETE "localhost:9200/twitter?pretty"
```

## 3.5.为index增加字段

```shell
curl -X PUT "localhost:9200/twitter/_mapping?pretty" -H 'Content-Type: application/json' -d'
{
  "properties": {
    "email": {
      "type": "keyword"
    }
  }
}
'
```

**摘要1**

Request body说明：

**`properties`**

(Required, [mapping object](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/mapping.html)) Mapping for a field. For new fields, this mapping can include:

- Field name
- [Field datatype](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/mapping.html#field-datatypes)
- [Mapping parameters](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/mapping-params.html)

For existing fields, see [Change the mapping of an existing field](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/indices-put-mapping.html#updating-field-mappings).

**摘要2**

Field的常用datatype：[`text`](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/text.html), [`keyword`](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/keyword.html), [`date`](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/date.html), [`long`](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/number.html), [`double`](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/number.html), [`boolean`](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/boolean.html) or [`ip`](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/ip.html).

引申问题：text与keyword的区别？

text可用于全文索引，该类型会被`analyzed`，值被传入`analyzer`从而转化成一系列子串，然后被反向索引(如分词后建立索引，能够支持模糊搜索)。text类型不被用于排序和聚合。

keyword则相反，用于排序和聚合，且只能精确匹配。

## 3.6.清空index

```shell
curl -X POST "localhost:9200/twitter/_refresh?pretty"
```

# 四、Document

## 4.1.概念

对应mysql的row，表示一行记录。

## 4.2.查看所有Document

查看一条记录

```shell
curl -X GET "localhost:9200/twitter/_doc/0?pretty"
```

查看多条记录

```shell
curl -X GET "localhost:9200/_mget?pretty" -H 'Content-Type: application/json' -d'
{
    "docs" : [
        {
            "_index" : "twitter",
            "_id" : "1"
        },
        {
            "_index" : "twitter",
            "_id" : "2"
        }
    ]
}
'
```

查看所有记录

```shell
curl -X GET "localhost:9200/twitter/_search?pretty"
```

## 4.3.批量操作Document

```shell
curl -X POST "localhost:9200/_bulk?pretty" -H 'Content-Type: application/json' -d'
{ "index" : { "_index" : "twitter", "_id" : "1" } }
{ "field1" : "value1" }
{ "delete" : { "_index" : "twitter", "_id" : "2" } }
{ "create" : { "_index" : "twitter", "_id" : "3" } }
{ "field1" : "value3" }
{ "update" : {"_id" : "1", "_index" : "twitter"} }
{ "doc" : {"field2" : "value2"} }
'
```

**摘要**

**Request body**

The request body contains a newline-delimited list of `create`, `delete`, `index`, and `update` actions and their associated source data.

- **`create`**

  (Optional, string) Indexes the specified document if it does not already exist. The following line must contain the source data to be indexed.**`_index`**(Optional, string) The name of the target index. Required if not specified as a path parameter.**`_id`**(Optional, string) The document ID. If no ID is specified, a document ID is automatically generated.

- **`delete`**

  (Optional, string) Removes the specified document from the index.**`_index`**(Optional, string) The name of the target index. Required if not specified as a path parameter.**`_id`**(Required, string) The document ID.

- **`index`**

  (Optional, string) Indexes the specified document. If the document exists, replaces the document and increments the version. The following line must contain the source data to be indexed.**`_index`**(Optional, string) The name of the target index. Required if not specified as a path parameter.**`_id`**(Optional, string) The document ID. If no ID is specified, a document ID is automatically generated.

- **`update`**

  (Optional, string) Performs a partial document update. The following line must contain the partial document and update options.**`_index`**(Optional, string) The name of the target index. Required if not specified as a path parameter.**`_id`**(Optional, string) The document ID. If no ID is specified, a document ID is automatically generated.

- **`doc`**

  (Optional, object) The partial document to index. Required for `update` operations.

- **`<fields>`**

  (Optional, object) The document source to index. Required for `create` and `index` operations.

## 4.4.模糊搜索Document

```shell
curl -X POST -H 'Content-Type: application/json' 'localhost:9200/twitter/_search?pretty' -d'
{
    "query": {
        "fuzzy": {"conent": {"value": "美好的一天", "fuzziness": "2"}}
    }
}
'
```

模糊搜索原理：Levenshtein编辑距离

**摘要**

**Request body**

 **Top-level parameters for `fuzzy`**

- **`<field>`**

  (Required, object) Field you wish to search.

**Parameters for `<field>`**

- **`value`**

  (Required, string) Term you wish to find in the provided ``.

- **`fuzziness`**

  (Optional, string) Maximum edit distance allowed for matching. See [Fuzziness](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/common-options.html#fuzziness) for valid values and more information.

- **`max_expansions`**

  (Optional, integer) Maximum number of variations created. Defaults to `50`.Avoid using a high value in the `max_expansions` parameter, especially if the `prefix_length` parameter value is `0`. High values in the `max_expansions` parameter can cause poor performance due to the high number of variations examined.

- **`prefix_length`**

  (Optional, integer) Number of beginning characters left unchanged when creating expansions. Defaults to `0`.

- **`transpositions`**

  (Optional, boolean) Indicates whether edits include transpositions of two adjacent characters (ab → ba). Defaults to `true`.

- **`rewrite`**

  (Optional, string) Method used to rewrite the query. For valid values and more information, see the [`rewrite` parameter](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/query-dsl-multi-term-rewrite.html).

## 4.5.批量搜索

一次请求执行多次搜索

```shell
curl -X GET "localhost:9200/twitter/_msearch?pretty" -H 'Content-Type: application/json' -d'
{ }
{"query" : {"match" : { "message": "this is a test"}}}
{"index": "twitter2"}
{"query" : {"match_all" : {}}}
'
```

**Request**

GET /`<index>`/_msearch

`<index>`显式制定index，也可以不填并在Request body中指明。

**Request body**

**`<header>`**

(Required, object) Contains parameters used to limit or change the subsequent search body request.

This object is required for each search body but can be empty (`{}`) or a blank line.

**`<body>`**

(Optional, object) Contains parameters for a search request:

**`aggregations`**

(Optional, [aggregation object](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/search-aggregations.html#_structuring_aggregations)) Aggregations you wish to run during the search. See [Aggregations](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/search-aggregations.html).

**`query`**

(Optional, [query DSL object](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/query-dsl.html)) Query you wish to run during the search. Hits matching this query are returned in the response.

**`from`**

(Optional, integer) Starting offset for returned hits. Defaults to `0`.

**`size`**

(Optional, integer) Number of hits to return. Defaults to `10`.

# 参考链接

1.[官方API文档](https://www.elastic.co/guide/en/elasticsearch/reference/7.6/rest-apis.html)

