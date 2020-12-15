# [1. ElasticSearch基本概念]()

Elaticsearch，简称为es， es是一个开源的高扩展的分布式全文检索引擎，它可以近乎实时的存储、检索数据；本身扩展性很好，可以扩展到上百台服务器，处理PB级别的数据。es也使用Java开发并使用Lucene作为其核心来实现所有索引和搜索的功能，但是它的目的是通过简单的RESTful API来隐藏Lucene的复杂性，从而让全文搜索变得简单。

**ElasticSearch对比Solr：**

- Solr 利用 Zookeeper 进行分布式管理，而 Elasticsearch 自身带有分布式协调管理功能;
- Solr 支持更多格式的数据，而 Elasticsearch 仅支持json文件格式；
- Solr 官方提供的功能更多，而 Elasticsearch 本身更注重于核心功能，高级功能多有第三方插件提供；
- Solr 在传统的搜索应用中表现好于 Elasticsearch，但在处理实时搜索应用时效率明显低于 Elasticsearch

# 2. ElasticSearch安装与启动

1. 下载ES压缩包，选择windows版本

   ElasticSearch的官方地址： https://www.elastic.co/products/elasticsearch

   > 注：7.9.2版本要求jdk的高版本

2. 解压压缩包

   进入config目录，添加如下内容

   > 此步为允许elasticsearch跨越访问，如果不安装后面的elasticsearch-head是可以不修改，直接启动。

   ```yml
   http.cors.enabled: true
   http.cors.allow-origin: "*"
   ```

   进入bin目录，双击elasticsearch.bat启动

   注意：**9300是tcp通讯端口**，集群间和TCPClient都执行该端口，**9200是http协议的RESTful接口** 。

3. 安装ElasticSearch的head插件

   进入谷歌商店搜ElasticSearch-head安装即可

4. 安装postman

   进入谷歌商店搜postman安装即可

# 3.ES 概述

**Elasticsearch是面向文档**(document oriented)的，这意味着它可以存储整个对象或文档(document)。然而它不仅仅是存储，还会**索引(index)每个文档的内容使之可以被搜索**。在Elasticsearch中，你可以对文档（而非成行成列的数据）进行索引、搜索、排序、过滤。Elasticsearch比传统关系型数据库如下：

```
Relational DB -> Databases -> Tables -> Rows -> Columns
Elasticsearch -> Indices（index的复数） -> Types  -> Documents -> Fields（域）
```

**es核心概念**

1. 索引 index

   **一个索引就是一个拥有几分相似特征的文档的集合。**比如说，你可以有一个客户数据的索引，另一个产品目录的索引，还有一个订单数据的索引。一个索引由一个名字来标识（必须全部是小写字母的），并且当我们要对对应于这个索引中的文档进行索引、搜索、更新和删除的时候，都要使用到这个名字。在一个集群中，可以定义任意多的索引。

2. 类型 type

   在一个索引中，你可以定义一种或多种类型。一个类型是你的索引的一个逻辑上的分类/分区，其语义完全由你来定。通常，会为具有一组共同字段的文档定义一个类型。比如说，我们假设你运营一个博客平台并且将你所有的数据存储到一个索引中。在这个索引中，你可以为用户数据定义一个类型，为博客数据定义另一个类型，当然，也可以为评论数据定义另一个类型。

3. 字段Field

   相当于是数据表的字段，对文档数据根据不同属性进行的分类标识

4.  映射 mapping

   mapping是处理数据的方式和规则方面做一些限制，如某个字段的数据类型、默认值、分析器、是否被索引等等，这些都是映射里面可以设置的，其它就是处理es里面数据的一些使用规则设置也叫做映射，按着最优规则处理数据对性能提高很大，因此才需要建立映射，并且需要思考如何建立映射才能对性能更好。

5.  文档 document

   一个文档是一个可被索引的基础信息单元。比如，你可以拥有某一个客户的文档，某一个产品的一个文档，当然，也可以拥有某个订单的一个文档。文档以JSON（Javascript Object Notation）格式来表示，而JSON是一个到处存在的互联网数据交互格式。

   在一个index/type里面，你可以存储任意多的文档。注意，尽管一个文档，物理上存在于一个索引之中，文档必须被索引/赋予一个索引的type。

6. 接近实时 NRT

   Elasticsearch是一个接近实时的搜索平台。这意味着，从索引一个文档直到这个文档能够被搜索到有一个轻微的延迟（通常是1秒以内）

# 4.ES客户端操作

实际开发中，主要有三种方式可以作为elasticsearch服务的客户端：

- 第一种：elasticsearch-head插件
- 第二种：使用elasticsearch提供的Restful接口直接访问
- 第三种：使用elasticsearch提供的API进行访问

**使用postman插件进行Restful接口访问**

## 4.1 ES接口语法

```
curl -X<VERB> '<PROTOCOL>://<HOST>:<PORT>/<PATH>?<QUERY_STRING>' -d '<BODY>'
```

参数说明：

| 参数           | 解释                                                         |
| -------------- | ------------------------------------------------------------ |
| `VERB`         | 适当的 HTTP *方法* 或 *谓词* : `GET`、 `POST`、 `PUT`、 `HEAD` 或者 `DELETE`。 |
| `PROTOCOL`     | `http` 或者 `https`（如果你在 Elasticsearch 前面有一个 `https` 代理） |
| `HOST`         | Elasticsearch 集群中任意节点的主机名，或者用 `localhost` 代表本地机器上的节点。 |
| `PORT`         | 运行 Elasticsearch HTTP 服务的端口号，默认是 `9200` 。       |
| `PATH`         | API 的终端路径（例如 `_count` 将返回集群中文档数量）。Path 可能包含多个组件，例如：`_cluster/stats` 和 `_nodes/stats/jvm` 。 |
| `QUERY_STRING` | 任意可选的查询字符串参数 (例如 `?pretty` 将格式化地输出 JSON 返回值，使其更容易阅读) |
| `BODY`         | 一个 JSON 格式的请求体 (如果请求需要的话)                    |

## 4.2 同时创建 索引 和 映射 

请求url

```
PUT		localhost:9200/blog
```

请求体：

```json
{
    "mappings": {
        "article": {
            "properties": {
                "id": {
                	"type": "long",
                    "store": true,
                    "index":"not_analyzed"
                },
                "title": {
                	"type": "text",
                    "store": true,
                    "index":"analyzed",
                    "analyzer":"standard"
                },
                "content": {
                	"type": "text",
                    "store": true,
                    "index":"analyzed",
                    "analyzer":"standard"
                }
            }
        }
    }
}
```

响应体

```json
{
    "acknowledged": true,
    "shards_acknowledged": true,
    "index": "blog"
}
```

## 4.3 创建索引后再设置Mapping

我们可以在创建索引时设置mapping信息，当然也可以先创建索引然后再设置mapping。

在上一个步骤中不设置maping信息，直接使用put方法创建一个索引，然后设置mapping信息。

请求的url：

1. 创建无Mapping的索引

   ```
   PUT 	http://127.0.0.1:9200/blog
   ```

2. 创建Mapping的URL

   ```
   POST	http://127.0.0.1:9200/blog/hello/_mapping
   ```

3. 创建Mapping的请求体

   ```json
   {
       "hello": {
               "properties": {
                   "id":{
                   	"type":"long",
                   	"store":true
                   },
                   "title":{
                   	"type":"text",
                   	"store":true,
                   	"index":true,
                   	"analyzer":"standard"
                   },
                   "content":{
                   	"type":"text",
                   	"store":true,
                   	"index":true,
                   	"analyzer":"standard"
                   }
             	  }
           }
     }
   ```

## 4.4 删除索引 index

请求URL

```
DELETE		localhost:9200/blog1
```

## 4.5 创建文档 document

请求url：

```json
POST	localhost:9200/blog/article/1
```

请求体：

```json
{
	"id":1,
	"title":"ElasticSearch是一个基于Lucene的搜索服务器",
	"content":"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。"
}
```

## 4.6 修改文档 document

请求url：

```json
POST	localhost:9200/blog/article/1
```

请求体：

```json
{
	"id":1,
	"title":"【修改】ElasticSearch是一个基于Lucene的搜索服务器",
	"content":"【修改】它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。"
}
```

## 4.7 删除文档 document

请求url：

```json
DELETE	localhost:9200/blog/article/1
```

## 4.8  查询文档 querystring

请求url：

```json
POST	localhost:9200/blog/article/_search
```

请求体：

```json
{
    "query": {
        "query_string": {
            "default_field": "title",
            "query": "搜索服务器"
        }
    }
}
```

响应体：

> 从响应回来的内容可以发现是已经成功了的，而且我们可以将查询的字符串换为 “钢索” 也是可以搜索到的，原因是我们目前使用的是标准分词器，对中文的分词会将每个字符都当作一个词语，所以是可以检索出来的

```json
{
    "took": 14,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": 1,
        "max_score": 1.3143302,
        "hits": [
            {
                "_index": "blog",
                "_type": "article",
                "_id": "1",
                "_score": 1.3143302,
                "_source": {
                    "id": 1,
                    "title": "ElasticSearch是一个基于Lucene的搜索服务器",
                    "content": "它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。"
                }
            }
        ]
    }
}
```

## 4.9 查询文档 trem查询

请求url：

```json
POST	localhost:9200/blog/article/_search
```

请求体：

```json
{
    "query": {
        "term": {
            "title": "搜索"
        }
    }
}
```

响应体

> 这里可以看到响应内容为空，原因还是因为标准分词器下，会将中文逐个分词。所以，只要改为单个词是可以搜索到的。解决方法为使用IK分词器

```json
{
    "took": 1,
    "timed_out": false,
    "_shards": {
        "total": 5,
        "successful": 5,
        "skipped": 0,
        "failed": 0
    },
    "hits": {
        "total": 0,
        "max_score": null,
        "hits": []
    }
}
```

# 5. IK 分词器和ES集成使用

## 5.1 问题分析

在进行字符串查询时，我们发现去搜索"搜索服务器"和"钢索"都可以搜索到数据；

而在进行词条查询时，我们搜索"搜索"却没有搜索到数据；

究其原因是ElasticSearch的标准分词器导致的，当我们创建索引时，字段使用的是标准分词器：

```json
{
    "mappings": {
        "article": {
            "properties": {
                "id": {
                	"type": "long",
                    "store": true,
                    "index":"not_analyzed"
                },
                "title": {
                	"type": "text",
                    "store": true,
                    "index":"analyzed",
                    "analyzer":"standard"	//标准分词器
                },
                "content": {
                	"type": "text",
                    "store": true,
                    "index":"analyzed",
                    "analyzer":"standard"	//标准分词器
                }
            }
        }
    }
}
```

例如对 "我是程序员" 进行分词

标准分词器分词效果测试：

```http
GET http://127.0.0.1:9200/_analyze?analyzer=standard&pretty=true&text=我是程序员
```

分词结果：

```json
{
    "tokens": [
        {
            "token": "我",
            "start_offset": 0,
            "end_offset": 1,
            "type": "<IDEOGRAPHIC>",
            "position": 0
        },
        {
            "token": "是",
            "start_offset": 1,
            "end_offset": 2,
            "type": "<IDEOGRAPHIC>",
            "position": 1
        },
        {
            "token": "程",
            "start_offset": 2,
            "end_offset": 3,
            "type": "<IDEOGRAPHIC>",
            "position": 2
        },
        {
            "token": "序",
            "start_offset": 3,
            "end_offset": 4,
            "type": "<IDEOGRAPHIC>",
            "position": 3
        },
        {
            "token": "员",
            "start_offset": 4,
            "end_offset": 5,
            "type": "<IDEOGRAPHIC>",
            "position": 4
        }
    ]
}
```

而我们需要的分词效果是：我、是、程序、程序员

这样的话就需要对中文支持良好的分析器的支持，支持中文分词的分词器有很多，word分词器、庖丁解牛、盘古分词、Ansj分词等，但我们常用的还是下面要介绍的IK分词器。

## 5.2 安装分词器

下载地址：https://github.com/medcl/elasticsearch-analysis-ik/releases   

1. 下载IK分词器
2. 解压压缩包，将解压后的elasticsearch文件夹拷贝到`elasticsearch-5.6.8\plugins`下，并重命名文件夹为analysis-ik
3. 重启ES

## 5.3 IK分词器测试

IK提供了两个分词算法` ik_smart `和` ik_max_word`

其中 **ik_smart 为最少切分**，**ik_max_word为最细粒度划分**

1. 最小切分测试

   ```http
   PUT http://127.0.0.1:9200/_analyze?analyzer=ik_smart&pretty=true&text=我是程序员
   ```

   输出的结果为：

   ```
   {
       "tokens": [
           {
               "token": "我",
               "start_offset": 0,
               "end_offset": 1,
               "type": "CN_CHAR",
               "position": 0
           },
           {
               "token": "是",
               "start_offset": 1,
               "end_offset": 2,
               "type": "CN_CHAR",
               "position": 1
           },
           {
               "token": "程序员",
               "start_offset": 2,
               "end_offset": 5,
               "type": "CN_WORD",
               "position": 2
           }
       ]
   }
   ```

   2）最细切分：在浏览器地址栏输入地址

   ```http
   GET http://127.0.0.1:9200/_analyze?analyzer=ik_max_word&pretty=true&text=我是程序员
   ```

   输出的结果为：

   ```json
   {
       "tokens": [
           {
               "token": "我",
               "start_offset": 0,
               "end_offset": 1,
               "type": "CN_CHAR",
               "position": 0
           },
           {
               "token": "是",
               "start_offset": 1,
               "end_offset": 2,
               "type": "CN_CHAR",
               "position": 1
           },
           {
               "token": "程序员",
               "start_offset": 2,
               "end_offset": 5,
               "type": "CN_WORD",
               "position": 2
           },
           {
               "token": "程序",
               "start_offset": 2,
               "end_offset": 4,
               "type": "CN_WORD",
               "position": 3
           },
           {
               "token": "员",
               "start_offset": 4,
               "end_offset": 5,
               "type": "CN_CHAR",
               "position": 4
           }
       ]
   }
   ```


## 5.4 修改索引映射mapping

1. 重建索引

   删除原有blog索引

   ```http
   DELETE		localhost:9200/blog
   ```

   创建blog索引，此时分词器使用ik_max_word

   ```http
   PUT		localhost:9200/blog
   ```

   ```json
   {
       "mappings": {
           "article": {
               "properties": {
                   "id": {
                   	"type": "long",
                       "store": true,
                       "index":"not_analyzed"
                   },
                   "title": {
                   	"type": "text",
                       "store": true,
                       "index":"analyzed",
                       "analyzer":"ik_max_word"
                   },
                   "content": {
                   	"type": "text",
                       "store": true,
                       "index":"analyzed",
                       "analyzer":"ik_max_word"
                   }
               }
           }
       }
   }
   ```

   创建文档

   ```http
   POST	localhost:9200/blog/article/1
   ```

   ```json
   {
   	"id":1,
   	"title":"ElasticSearch是一个基于Lucene的搜索服务器",
   	"content":"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。"
   }
   ```

2. 再次测试queryString查询

   请求url：

   ```http
   POST	localhost:9200/blog/article/_search
   ```

   请求体：

   ```json
   {
       "query": {
           "query_string": {
               "default_field": "title",
               "query": "搜索服务器"
           }
       }
   }
   ```

   响应体

   ```json
   {
       "took": 1,
       "timed_out": false,
       "_shards": {
           "total": 5,
           "successful": 5,
           "skipped": 0,
           "failed": 0
       },
       "hits": {
           "total": 1,
           "max_score": 1,
           "hits": [
               {
                   "_index": "blog",
                   "_type": "article",
                   "_id": "1",
                   "_score": 1,
                   "_source": {
                       "id": 1,
                       "title": "ElasticSearch是一个基于Lucene的搜索服务器",
                       "content": "它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。"
                   }
               }
           ]
       }
   }
   ```

   将请求体搜索字符串修改为"钢索"，再次查询：

   ```json
   {
       "query": {
           "query_string": {
               "default_field": "title",
               "query": "钢索"
           }
       }
   }
   ```

   此时响应体为空

   ```json
   {
       "took": 0,
       "timed_out": false,
       "_shards": {
           "total": 5,
           "successful": 5,
           "skipped": 0,
           "failed": 0
       },
       "hits": {
           "total": 0,
           "max_score": null,
           "hits": []
       }
   }
   ```

3. 测试term

   请求url：

   ```http
   POST	localhost:9200/blog/article/_search
   ```

   请求体：

   ```json
   {
       "query": {
           "term": {
               "title": "搜索"
           }
       }
   }
   ```

   响应体：

   ```json
   {
       "took": 1,
       "timed_out": false,
       "_shards": {
           "total": 5,
           "successful": 5,
           "skipped": 0,
           "failed": 0
       },
       "hits": {
           "total": 1,
           "max_score": 0.25316024,
           "hits": [
               {
                   "_index": "blog",
                   "_type": "article",
                   "_id": "1",
                   "_score": 0.25316024,
                   "_source": {
                       "id": 1,
                       "title": "ElasticSearch是一个基于Lucene的搜索服务器",
                       "content": "它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。"
                   }
               }
           ]
       }
   }
   ```

# 6. 搭建ES集群

​	ES集群是一个 P2P类型(使用 gossip 协议)的分布式系统，除了集群状态管理以外，其他所有的请求都可以发送到集群内任意一台节点上，这个节点可以自己找到需要转发给哪些节点，并且直接跟这些节点通信。所以，从网络架构及服务配置上来说，构建集群所需要的配置极其简单。在 Elasticsearch 2.0 之前，无阻碍的网络下，所有配置了相同 cluster.name 的节点都自动归属到一个集群中。2.0 版本之后，基于安全的考虑避免开发环境过于随便造成的麻烦，从 2.0 版本开始，默认的自动发现方式改为了单播(unicast)方式。配置里提供几台节点的地址，ES 将其视作 gossip router 角色，借以完成集群的发现。由于这只是 ES 内一个很小的功能，所以 gossip router 角色并不需要单独配置，每个 ES 节点都可以担任。所以，采用单播方式的集群，各节点都配置相同的几个节点列表作为 router 即可。

​	集群中节点数量没有限制，一般大于等于2个节点就可以看做是集群了。一般处于高性能及高可用方面来考虑一般集群中的节点数量都是3个及3个以上。

## 6.1 集群基本概念

### 6.1.1 集群 cluster

一个集群就是由一个或多个节点组织在一起，它们共同持有整个的数据，并一起提供索引和搜索功能。一个集群由一个唯一的名字标识，这个名字默认就是“elasticsearch”。这个名字是重要的，因为一个节点只能通过指定某个集群的名字，来加入这个集群

### 6.1.2 节点 node

一个节点是集群中的一个服务器，作为集群的一部分，它存储数据，参与集群的索引和搜索功能。和集群类似，一个节点也是由一个名字来标识的，默认情况下，这个名字是一个随机的漫威漫画角色的名字，这个名字会在启动的时候赋予节点。这个名字对于管理工作来说挺重要的，因为在这个管理过程中，你会去确定网络中的哪些服务器对应于Elasticsearch集群中的哪些节点。

一个节点可以通过配置集群名称的方式来加入一个指定的集群。默认情况下，每个节点都会被安排加入到一个叫做“elasticsearch”的集群中，这意味着，如果你在你的网络中启动了若干个节点，并假定它们能够相互发现彼此，它们将会自动地形成并加入到一个叫做“elasticsearch”的集群中。

在一个集群里，只要你想，可以拥有任意多个节点。而且，如果当前你的网络中没有运行任何Elasticsearch节点，这时启动一个节点，会默认创建并加入一个叫做“elasticsearch”的集群。

### 6.1.3 分片和复制 shards&replicas

一个索引可以存储超出单个结点硬件限制的大量数据。比如，一个具有10亿文档的索引占据1TB的磁盘空间，而任一节点都没有这样大的磁盘空间；或者单个节点处理搜索请求，响应太慢。为了解决这个问题，Elasticsearch提供了将索引划分成多份的能力，这些份就叫做分片。当你创建一个索引的时候，你可以指定你想要的分片的数量。每个分片本身也是一个功能完善并且独立的“索引”，这个“索引”可以被放置到集群中的任何节点上。分片很重要，主要有两方面的原因： 

1. 允许你水平分割/扩展你的内容容量。 
2. 允许你在分片（潜在地，位于多个节点上）之上进行分布式的、并行的操作，进而提高性能/吞吐量。

至于一个分片怎样分布，它的文档怎样聚合回搜索请求，是完全由Elasticsearch管理的，对于作为用户的你来说，这些都是透明的。

在一个网络/云的环境里，失败随时都可能发生，在某个分片/节点不知怎么的就处于离线状态，或者由于任何原因消失了，这种情况下，有一个故障转移机制是非常有用并且是强烈推荐的。为此目的，Elasticsearch允许你创建分片的一份或多份拷贝，这些拷贝叫做复制分片，或者直接叫复制。

复制之所以重要，有两个主要原因： 在分片/节点失败的情况下，提供了高可用性。因为这个原因，注意到复制分片从不与原/主要（original/primary）分片置于同一节点上是非常重要的。扩展你的搜索量/吞吐量，因为搜索可以在所有的复制上并行运行。总之，每个索引可以被分成多个分片。一个索引也可以被复制0次（意思是没有复制）或多次。一旦复制了，每个索引就有了主分片（作为复制源的原来的分片）和复制分片（主分片的拷贝）之别。分片和复制的数量可以在索引创建的时候指定。在索引创建之后，你可以在任何时候动态地改变复制的数量，但是你事后不能改变分片的数量。

默认情况下，Elasticsearch中的每个索引被分片5个主分片和1个复制，这意味着，如果你的集群中至少有两个节点，你的索引将会有5个主分片和另外5个复制分片（1个完全拷贝），这样的话每个索引总共就有10个分片。

## 6.2  搭建

1. 准备三台elasticsearch服务器

   创建elasticsearch-cluster文件夹，在内部复制三个elasticsearch服务

   > 注意：删除每个服务器下的data文件夹下所有数据

2. 修改每台服务器配置

   修改`ES-node*\config\elasticsearch.yml`配置文件

   - node1节点

     ```yml
     #节点1的配置信息：
     #集群名称，保证唯一
     cluster.name: my-elasticsearch
     #节点名称，必须不一样
     node.name: node-1
     #必须为本机的ip地址
     network.host: 127.0.0.1
     #服务端口号，在同一机器下必须不一样
     http.port: 9200
     #集群间通信端口号，在同一机器下必须不一样
     transport.tcp.port: 9300
     #设置集群自动发现机器ip集合
     discovery.zen.ping.unicast.hosts: ["127.0.0.1:9300","127.0.0.1:9301","127.0.0.1:9302"]
     cluster.initial_master_nodes: ["node-1"]
     #允许跨域访问，以支持head插件
     http.cors.enabled: true 
     http.cors.allow-origin: "*"
     ```

   - node2节点

     ```yml
     #节点2的配置信息：
     #集群名称，保证唯一
     cluster.name: my-elasticsearch
     #节点名称，必须不一样
     node.name: node-2
     #必须为本机的ip地址
     network.host: 127.0.0.1
     #服务端口号，在同一机器下必须不一样
     http.port: 9201
     #集群间通信端口号，在同一机器下必须不一样
     transport.tcp.port: 9301
     #设置集群自动发现机器ip集合
     discovery.zen.ping.unicast.hosts: ["127.0.0.1:9300","127.0.0.1:9301","127.0.0.1:9302"]
     cluster.initial_master_nodes: ["node-1"]
     #允许跨域访问，以支持head插件
     http.cors.enabled: true 
     http.cors.allow-origin: "*"
     ```

   - node3节点

     ```yml
     #节点3的配置信息：
     #集群名称，保证唯一
     cluster.name: my-elasticsearch
     #节点名称，必须不一样
     node.name: node-3
     #必须为本机的ip地址
     network.host: 127.0.0.1
     #服务端口号，在同一机器下必须不一样
     http.port: 9202
     #集群间通信端口号，在同一机器下必须不一样
     transport.tcp.port: 9302
     #设置集群自动发现机器ip集合
     discovery.zen.ping.unicast.hosts: ["127.0.0.1:9300","127.0.0.1:9301","127.0.0.1:9302"]
     cluster.initial_master_nodes: ["node-1"]
     
     
     #允许跨域访问，以支持head插件
     http.cors.enabled: true 
     http.cors.allow-origin: "*"
     ```

3. 启动各节点服务器

4. 集群测试

   添加索引和映射

   ```http
   PUT		localhost:9200/blog
   ```

   ```json
   {
       "mappings": {
           "article": {
               "properties": {
                   "id": {
                   	"type": "long",
                       "store": true,
                       "index":"not_analyzed"
                   },
                   "title": {
                   	"type": "text",
                       "store": true,
                       "index":"analyzed",
                       "analyzer":"standard"
                   },
                   "content": {
                   	"type": "text",
                       "store": true,
                       "index":"analyzed",
                       "analyzer":"standard"
                   }
               }
           }
       }
   }
   ```

   添加文档

   ```http
   POST	localhost:9200/blog/article/1
   ```

   ```json
   {
   	"id":1,
   	"title":"ElasticSearch是一个基于Lucene的搜索服务器",
   	"content":"它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。"
   }
   ```

5. 使用elasticsearch-header查看集群情况

# 7. ES Java操作

> 说明：此javaAPI基于ElasticSearch 5.6.8版本，此版本较低，和高版本不兼容。

## 7.1 入门案例

1. 创建工程，导入坐标

   ```xml
   <dependencies>
       <dependency>
           <groupId>org.elasticsearch</groupId>
           <artifactId>elasticsearch</artifactId>
           <version>5.6.8</version>
       </dependency>
       <dependency>
           <groupId>org.elasticsearch.client</groupId>
           <artifactId>transport</artifactId>
           <version>5.6.8</version>
       </dependency>
       <dependency>
           <groupId>org.apache.logging.log4j</groupId>
           <artifactId>log4j-to-slf4j</artifactId>
           <version>2.9.1</version>
       </dependency>
       <dependency>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-api</artifactId>
           <version>1.7.24</version>
       </dependency>
       <dependency>
           <groupId>org.slf4j</groupId>
           <artifactId>slf4j-simple</artifactId>
           <version>1.7.21</version>
       </dependency>
       <dependency>
           <groupId>log4j</groupId>
           <artifactId>log4j</artifactId>
           <version>1.2.12</version>
       </dependency>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
       </dependency>
   </dependencies>
   ```

2. 创建索引

   ```java
   @Test
       //创建索引
       public void test1() throws Exception{
           // 创建Client连接对象
           Settings settings = Settings.builder().put("cluster.name", "my-elasticsearch").build();
           TransportClient client = new PreBuiltTransportClient(settings)
                   .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9300));
           //创建名称为blog2的索引
           client.admin().indices().prepareCreate("blog").get();
           //释放资源
           client.close();
       }
   ```

## 7.2 建立文档

1. 建立文档（通过XContentBuilder）

   ```java
   @Test
   //创建文档(通过XContentBuilder)
   public void test4() throws Exception{
       // 创建Client连接对象
       Settings settings = Settings.builder().put("cluster.name", "my-elasticsearch").build();
       TransportClient client = new PreBuiltTransportClient(settings)
           .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9300));
   
       //创建文档信息
       XContentBuilder builder = XContentFactory.jsonBuilder()
           .startObject()
           .field("id", 1)
           .field("title", "ElasticSearch是一个基于Lucene的搜索服务器")
           .field("content",
                  "它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口。Elasticsearch是用Java开发的，并作为Apache许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。")
           .endObject();
   
       // 建立文档对象
       /**
            * 参数一blog1：表示索引对象
            * 参数二article：类型
            * 参数三1：建立id
            */
       client.prepareIndex("blog2", "article", "1").setSource(builder).get();
   
       //释放资源
       client.close();
   }
   ```

2. 建立文档（使用Jackson转换实体）

   - 建立实体Article

     ```java
     public class Article {
     	private Integer id;
     	private String title;
     	private String content;
         
         getter/setter...
     }
     ```

   - 添加jackson坐标

     ```xml
     <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-core</artifactId>
         <version>2.8.1</version>
     </dependency>
     <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-databind</artifactId>
         <version>2.8.1</version>
     </dependency>
     <dependency>
         <groupId>com.fasterxml.jackson.core</groupId>
         <artifactId>jackson-annotations</artifactId>
         <version>2.8.1</version>
     </dependency>
     ```

   - 代码实现

     ```java
     @Test
     //创建文档(通过实体转json)
     public void test5() throws Exception{
         // 创建Client连接对象
         Settings settings = Settings.builder().put("cluster.name", "my-elasticsearch").build();
         TransportClient client = new PreBuiltTransportClient(settings)
             .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9300));
     
         // 描述json 数据
         //{id:xxx, title:xxx, content:xxx}
         Article article = new Article();
         article.setId(2);
         article.setTitle("搜索工作其实很快乐");
         article.setContent("我们希望我们的搜索解决方案要快，我们希望有一个零配置和一个完全免费的搜索模式，我们希望能够简单地使用JSON通过HTTP的索引数据，我们希望我们的搜索服务器始终可用，我们希望能够一台开始并扩展到数百，我们要实时搜索，我们要简单的多租户，我们希望建立一个云的解决方案。Elasticsearch旨在解决所有这些问题和更多的问题。");
     
         ObjectMapper objectMapper = new ObjectMapper();
     
         // 建立文档
         client.prepareIndex("blog2", "article", article.getId().toString())
             //.setSource(objectMapper.writeValueAsString(article)).get();
             .setSource(objectMapper.writeValueAsString(article).getBytes(), XContentType.JSON).get();
     
         //释放资源
         client.close();
     }
     ```

## 7.3 查询文档

1. 关键字查询

   ```java
   @Test
   // 关键词查询
   public void testTermQuery() throws Exception{
       //1、创建es客户端连接对象
       Settings settings = Settings.builder().put("cluster.name", "my-elasticsearch").build();
       TransportClient client = new PreBuiltTransportClient(settings)
           .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9300));
   
       //2、设置搜索条件
       SearchResponse searchResponse = client.prepareSearch("blog")
           .setTypes("article")
           .setQuery(QueryBuilders.termQuery("content", "搜索")).get();
   
       //3、遍历搜索结果数据
       SearchHits hits = searchResponse.getHits(); // 获取命中次数，查询结果有多少对象
       System.out.println("查询结果有：" + hits.getTotalHits() + "条");
       Iterator<SearchHit> iterator = hits.iterator();
       while (iterator.hasNext()) {
           SearchHit searchHit = iterator.next(); // 每个查询对象
           System.out.println(searchHit.getSourceAsString()); // 获取字符串格式打印
           System.out.println("title:" + searchHit.getSource().get("title"));
       }
   
       //4、释放资源
       client.close();
   
   }
   ```

2. 字符串查询

   ```java
   @Test
   // 字符串查询
   public void testStringQuery() throws Exception{
       //1、创建es客户端连接对象
       Settings settings = Settings.builder().put("cluster.name", "my-elasticsearch").build();
       TransportClient client = new PreBuiltTransportClient(settings)
           .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9300));
   
       //2、设置搜索条件
       SearchResponse searchResponse = client.prepareSearch("blog")
           .setTypes("article")
           .setQuery(QueryBuilders.queryStringQuery("搜索")).get();
   
       //3、遍历搜索结果数据
       SearchHits hits = searchResponse.getHits(); // 获取命中次数，查询结果有多少对象
       System.out.println("查询结果有：" + hits.getTotalHits() + "条");
       Iterator<SearchHit> iterator = hits.iterator();
       while (iterator.hasNext()) {
           SearchHit searchHit = iterator.next(); // 每个查询对象
           System.out.println(searchHit.getSourceAsString()); // 获取字符串格式打印
           System.out.println("title:" + searchHit.getSource().get("title"));
       }
   
       //4、释放资源
       client.close();
   
   }
   ```

3.  使用文档ID查询文档

   ```java
   @Test
   // 使用文档ID查询文档
   public void testIdQuery() throws Exception {
        //1、创建es客户端连接对象
       Settings settings = Settings.builder().put("cluster.name", "my-elasticsearch").build();
       TransportClient client = new PreBuiltTransportClient(settings)
           .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9300));
   
       
       //2、client对象为TransportClient对象
       SearchResponse response = client.prepareSearch("blog")
           .setTypes("article")
           //设置要查询的id
           .setQuery(QueryBuilders.idsQuery().addIds("2"))
           //执行查询
           .get();
       //取查询结果
       SearchHits searchHits = response.getHits();
       //取查询结果总记录数
       System.out.println(searchHits.getTotalHits());
       Iterator<SearchHit> hitIterator = searchHits.iterator();
       while(hitIterator.hasNext()) {
           SearchHit searchHit = hitIterator.next();
           //打印整行数据
           System.out.println(searchHit.getSourceAsString());
       }
   
       client.close();
   }
   ```

## 7.4 批量插入数据

```java
@Test
//批量插入100条数据
public void test9() throws Exception{
    // 创建Client连接对象
    Settings settings = Settings.builder().put("cluster.name", "my-elasticsearch").build();
    TransportClient client = new PreBuiltTransportClient(settings)
        .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9300));

    ObjectMapper objectMapper = new ObjectMapper();

    for (int i = 1; i <= 100; i++) {
        // 描述json 数据
        Article article = new Article();
        article.setId(i);
        article.setTitle(i + "搜索工作其实很快乐");
        article.setContent(i
                           + "我们希望我们的搜索解决方案要快，我们希望有一个零配置和一个完全免费的搜索模式，我们希望能够简单地使用JSON通过HTTP的索引数据，我们希望我们的搜索服务器始终可用，我们希望能够一台开始并扩展到数百，我们要实时搜索，我们要简单的多租户，我们希望建立一个云的解决方案。Elasticsearch旨在解决所有这些问题和更多的问题。");

        // 建立文档
        client.prepareIndex("blog", "article", article.getId().toString())
            //.setSource(objectMapper.writeValueAsString(article)).get();
            .setSource(objectMapper.writeValueAsString(article).getBytes(),XContentType.JSON).get();
    }

    //释放资源
    client.close();
}

```

## 7.5 分页查询

```java
@Test
//分页查询
public void test10() throws Exception{
    // 创建Client连接对象
    Settings settings = Settings.builder().put("cluster.name", "my-elasticsearch").build();
    TransportClient client = new PreBuiltTransportClient(settings)
        .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9300));

    // 搜索数据
    SearchRequestBuilder searchRequestBuilder = client.prepareSearch("blog2").setTypes("article")
        .setQuery(QueryBuilders.matchAllQuery());//默认每页10条记录

    // 查询第2页数据，每页20条
    //setFrom()：从第几条开始检索，默认是0。
    //setSize():每页最多显示的记录数。
    searchRequestBuilder.setFrom(0).setSize(5);
    SearchResponse searchResponse = searchRequestBuilder.get();

    SearchHits hits = searchResponse.getHits(); // 获取命中次数，查询结果有多少对象
    System.out.println("查询结果有：" + hits.getTotalHits() + "条");
    Iterator<SearchHit> iterator = hits.iterator();
    while (iterator.hasNext()) {
        SearchHit searchHit = iterator.next(); // 每个查询对象
        System.out.println(searchHit.getSourceAsString()); // 获取字符串格式打印
        System.out.println("id:" + searchHit.getSource().get("id"));
        System.out.println("title:" + searchHit.getSource().get("title"));
        System.out.println("content:" + searchHit.getSource().get("content"));
        System.out.println("-----------------------------------------");
    }

    //释放资源
    client.close();
}
```

## 7.6  高亮显示

```java
@Test
//高亮查询
public void test11() throws Exception{
    // 创建Client连接对象
    Settings settings = Settings.builder().put("cluster.name", "my-elasticsearch").build();
    TransportClient client = new PreBuiltTransportClient(settings)
        .addTransportAddress(new InetSocketTransportAddress(InetAddress.getByName("127.0.0.1"), 9300));

    // 搜索数据
    SearchRequestBuilder searchRequestBuilder = client
        .prepareSearch("blog").setTypes("article")
        .setQuery(QueryBuilders.termQuery("title", "搜索"));

    //设置高亮数据
    HighlightBuilder hiBuilder=new HighlightBuilder();
    hiBuilder.preTags("<font style='color:red'>");
    hiBuilder.postTags("</font>");
    hiBuilder.field("title");
    searchRequestBuilder.highlighter(hiBuilder);

    //获得查询结果数据
    SearchResponse searchResponse = searchRequestBuilder.get();

    //获取查询结果集
    SearchHits searchHits = searchResponse.getHits();
    System.out.println("共搜到:"+searchHits.getTotalHits()+"条结果!");
    //遍历结果
    for(SearchHit hit:searchHits){
        System.out.println("String方式打印文档搜索内容:");
        System.out.println(hit.getSourceAsString());
        System.out.println("Map方式打印高亮内容");
        System.out.println(hit.getHighlightFields());

        System.out.println("遍历高亮集合，打印高亮片段:");
        Text[] text = hit.getHighlightFields().get("title").getFragments();
        for (Text str : text) {
            System.out.println(str);
        }
    }

    //释放资源
    client.close();
}
```

# 8. Spring Data ElasticSearch 使用

Spring Data是一个用于简化数据库访问，并支持云服务的开源框架。其主要目标是使得对数据的访问变得方便快捷，并支持map-reduce框架和云计算数据服务。 Spring Data可以极大的简化JPA的写法，可以在几乎不用写实现的情况下，实现对数据的访问和操作。除了CRUD外，还包括如分页、排序等一些常用的功能。

Spring Data的官网：http://projects.spring.io/spring-data/

> Spring Data ElasticSearch 基于 spring data API 简化 elasticSearch操作，将原始操作elasticSearch的客户端API 进行封装 。Spring Data为Elasticsearch项目提供集成搜索引擎。Spring Data Elasticsearch POJO的关键功能区域为中心的模型与Elastichsearch交互文档和轻松地编写一个存储库数据访问层。
>
> 官方网站：<http://projects.spring.io/spring-data-elasticsearch/> 

## 8.1 入门案例

1. 创建模块并添加坐标

   注：junit测试坐标必须在4.12以上，不然会报初始化错误

   ```xml
   <dependencies>
       <dependency>
           <groupId>org.elasticsearch</groupId>
           <artifactId>elasticsearch</artifactId>
           <version>5.6.8</version>
       </dependency>
       <dependency>
           <groupId>org.elasticsearch.client</groupId>
           <artifactId>transport</artifactId>
           <version>5.6.8</version>
       </dependency>
       <dependency>
           <groupId>junit</groupId>
           <artifactId>junit</artifactId>
           <version>4.12</version>
           <scope>compile</scope>
       </dependency>
       <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>fastjson</artifactId>
           <version>1.2.47</version>
       </dependency>
       <dependency>
           <groupId>org.apache.logging.log4j</groupId>
           <artifactId>log4j-core</artifactId>
           <version>2.9.1</version>
       </dependency>
       <dependency>
           <groupId>com.fasterxml.jackson.core</groupId>
           <artifactId>jackson-core</artifactId>
           <version>2.8.1</version>
       </dependency>
       <dependency>
           <groupId>com.fasterxml.jackson.core</groupId>
           <artifactId>jackson-databind</artifactId>
           <version>2.8.1</version>
       </dependency>
       <dependency>
           <groupId>com.fasterxml.jackson.core</groupId>
           <artifactId>jackson-annotations</artifactId>
           <version>2.8.1</version>
       </dependency>
   
       <dependency>
           <groupId>org.springframework.data</groupId>
           <artifactId>spring-data-elasticsearch</artifactId>
           <version>3.0.5.RELEASE</version>
           <exclusions>
               <exclusion>
                   <groupId>org.elasticsearch.plugin</groupId>
                   <artifactId>transport-netty4-client</artifactId>
               </exclusion>
           </exclusions>
       </dependency>
   
       <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-test</artifactId>
           <version>5.0.4.RELEASE</version>
       </dependency>
   
   </dependencies>
   ```

2. 配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   
   <beans xmlns="http://www.springframework.org/schema/beans"
   	   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   	   xmlns:context="http://www.springframework.org/schema/context"
   	   xmlns:elasticsearch="http://www.springframework.org/schema/data/elasticsearch"
   	   xsi:schemaLocation="
   		http://www.springframework.org/schema/beans
   		http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
   		http://www.springframework.org/schema/context
   		http://www.springframework.org/schema/context/spring-context-4.0.xsd
           http://www.springframework.org/schema/data/elasticsearch
           http://www.springframework.org/schema/data/elasticsearch/spring-elasticsearch-1.0.xsd ">
   
   	<!-- 扫描Dao包，自动创建实例 -->
   	<elasticsearch:repositories base-package="dao"></elasticsearch:repositories>
   
   	<!-- 扫描Service包，创建Service的实体 -->
   	<context:component-scan base-package="service"/>
   
   	<!-- 配置elasticSearch的连接 -->
   	<elasticsearch:transport-client id="client" cluster-nodes="localhost:9300" cluster-name="my-elasticsearch"/>
   
   
   	<!-- ElasticSearch模版对象 -->
   	<bean id="elasticsearchTemplate" class="org.springframework.data.elasticsearch.core.ElasticsearchTemplate">
   		<constructor-arg name="client" ref="client"></constructor-arg>
   	</bean>
   
   </beans>
   ```

   ```
   其中，注解解释如下：
   @Document(indexName="blob3",type="article")：
       indexName：索引的名称（必填项）
       type：索引的类型
   @Id：主键的唯一标识
   @Field(index=true,analyzer="ik_smart",store=true,searchAnalyzer="ik_smart",type = FieldType.text)
       index：是否设置分词
       analyzer：存储时使用的分词器
       searchAnalyze：搜索时使用的分词器
       store：是否存储
       type: 数据类型
   ```

   

3. dao层

   ```java
   @Repository
   public interface ArticleRepository extends ElasticsearchRepository<Article,Integer> {
   
   }
   ```

4. 实体类

   ```java
   //@Document 文档对象 （索引信息、文档类型 ）
   @Document(indexName="spirng_es",type="article")
   public class Article {
   
       //@Id 文档主键 唯一标识
       @Id
       //@Field 每个文档的字段配置（类型、是否分词、是否存储、分词器 ）
       @Field(store=true, index = false,type = FieldType.Integer)
       private Integer id;
       @Field(index=true,analyzer="ik_smart",store=true,searchAnalyzer="ik_smart",type = FieldType.text)
       private String title;
       @Field(index=true,analyzer="ik_smart",store=true,searchAnalyzer="ik_smart",type = FieldType.text)
       private String content;
   
       public Integer getId() {
           return id;
       }
       public void setId(Integer id) {
           this.id = id;
       }
       public String getTitle() {
           return title;
       }
       public void setTitle(String title) {
           this.title = title;
       }
       public String getContent() {
           return content;
       }
       public void setContent(String content) {
           this.content = content;
       }
   
       @Override
       public String toString() {
           return "Article [id=" + id + ", title=" + title + ", content=" + content + "]";
       }
   
   }
   ```

5. service层

   ```java
   public interface ArticleService {
   
       void save(Article article);
   
   }
   ```

   ```java
   @Service
   public class ArticleServiceImpl implements ArticleService {
   
       @Autowired
       private ArticleRepository articleRepository;
   
       @Override
       public void save(Article article) {
           articleRepository.save(article);
       }
   
   }
   
   ```

6. 测试

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations="classpath:applicationConfig.xml")
   public class SpringDataESTest {
   
       @Autowired
       private ArticleService articleService;
   
       @Autowired
       private ElasticsearchTemplate elasticsearchTemplate;
   
       /**创建索引和映射*/
       @Test
       public void createIndex(){
           // 创建索引
           elasticsearchTemplate.createIndex(Article.class);
           // 配置映射
           elasticsearchTemplate.putMapping(Article.class);
       }
   
       /**测试保存文档*/
       @Test
       public void saveArticle(){
           Article article = new Article();
           article.setId(100);
           article.setTitle("测试SpringData ElasticSearch");
           article.setContent("Spring Data ElasticSearch 基于 spring data API 简化 elasticSearch操作，将原始操作elasticSearch的客户端API 进行封装 \n" +
                   " Spring Data为Elasticsearch Elasticsearch项目提供集成搜索引擎");
           articleService.save(article);
       }
   
   }
   
   ```

## 8.2 增删改查方法测试 

1. 修改service

   ```java
   public interface ArticleService {
   
       //保存
       public void save(Article article);
       //删除
       public void delete(Article article);
       //查询全部
       public Iterable<Article> findAll();
       //分页查询
       public Page<Article> findAll(Pageable pageable);
   
   }
   ```

   ```java
   @Service
   public class ArticleServiceImpl implements ArticleService {
   
       @Autowired
       private ArticleRepository articleRepository;
   
       @Override
       public void save(Article article) {
           articleRepository.save(article);
       }
       @Override
       public void delete(Article article) {
           articleRepository.delete(article);
       }
       @Override
       public Iterable<Article> findAll() {
           Iterable<Article> iter = articleRepository.findAll();
           return iter;
       }
       @Override
       public Page<Article> findAll(Pageable pageable) {
           return articleRepository.findAll(pageable);
       }
   }
   
   ```

2. 测试

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations="classpath:applicationConfig.xml")
   public class SpringDataESTest {
   
       @Autowired
       private ArticleService articleService;
   
       @Autowired
       private ElasticsearchTemplate elasticsearchTemplate;
   
       /**创建索引和映射*/
       @Test
       public void createIndex(){
           // 创建索引
           elasticsearchTemplate.createIndex(Article.class);
           // 配置映射
           elasticsearchTemplate.putMapping(Article.class);
       }
   
       /**测试保存文档*/
       @Test
       public void saveArticle(){
           Article article = new Article();
           article.setId(100);
           article.setTitle("测试SpringData ElasticSearch");
           article.setContent("Spring Data ElasticSearch 基于 spring data API 简化 elasticSearch操作，将原始操作elasticSearch的客户端API 进行封装 \n" +
                   " Spring Data为Elasticsearch Elasticsearch项目提供集成搜索引擎");
           articleService.save(article);
       }
       /**测试保存*/
       @Test
       public void save(){
           Article article = new Article();
           article.setId(1001);
           article.setTitle("elasticSearch 3.0版本发布");
           article.setContent("ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口");
           articleService.save(article);
       }
   
       /**测试更新*/
       @Test
       public void update(){
           Article article = new Article();
           article.setId(1001);
           article.setTitle("elasticSearch 3.0版本发布...更新");
           article.setContent("ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口");
           articleService.save(article);
       }
   
       /**测试删除*/
       @Test
       public void delete(){
           Article article = new Article();
           article.setId(1001);
           articleService.delete(article);
       }
   
       /**批量插入*/
       @Test
       public void save100(){
           for(int i=1;i<=100;i++){
               Article article = new Article();
               article.setId(i);
               article.setTitle(i+"elasticSearch 3.0版本发布..，更新");
               article.setContent(i+"ElasticSearch是一个基于Lucene的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于RESTful web接口");
               articleService.save(article);
           }
       }
   
       /**分页查询*/
       @Test
       public void findAllPage(){
           Pageable pageable = PageRequest.of(1,10);
           Page<Article> page = articleService.findAll(pageable);
           for(Article article:page.getContent()){
               System.out.println(article);
           }
       }
   }
   ```

   

## 8.3 常用查询命名规则

| 关键字        | 命名规则              | 解释                       | 示例                  |
| ------------- | --------------------- | -------------------------- | --------------------- |
| and           | findByField1AndField2 | 根据Field1和Field2获得数据 | findByTitleAndContent |
| or            | findByField1OrField2  | 根据Field1或Field2获得数据 | findByTitleOrContent  |
| is            | findByField           | 根据Field获得数据          | findByTitle           |
| not           | findByFieldNot        | 根据Field获得补集数据      | findByTitleNot        |
| between       | findByFieldBetween    | 获得指定范围的数据         | findByPriceBetween    |
| lessThanEqual | findByFieldLessThan   | 获得小于等于指定值的数据   | findByPriceLessThan   |

## 8.4 自定义查询

1. dao层

   ```java
   @Repository
   public interface ArticleRepository extends ElasticsearchRepository<Article,Integer> {
       //根据标题查询
       List<Article> findByTitle(String condition);
       //根据标题查询(含分页)
       Page<Article> findByTitle(String condition, Pageable pageable);
   }
   ```

2. service层

   ```java
   public interface ArticleService {
   
       //根据标题查询
       List<Article> findByTitle(String condition);
       //根据标题查询(含分页)
       Page<Article> findByTitle(String condition, Pageable pageable);
   }
   ```

   ```java
   @Service
   public class ArticleServiceImpl implements ArticleService {
   
       @Autowired
       private ArticleRepository articleRepository;
   
   
       @Override
       public List<Article> findByTitle(String condition) {
           return articleRepository.findByTitle(condition);
       }
       
       @Override
       public Page<Article> findByTitle(String condition, Pageable pageable) {
           return articleRepository.findByTitle(condition,pageable);
       }
   
   }
   
   ```

3. 测试

   ```java
   @RunWith(SpringJUnit4ClassRunner.class)
   @ContextConfiguration(locations="classpath:applicationConfig.xml")
   public class SpringDataESTest {
   
       @Autowired
       private ArticleService articleService;
   
       @Autowired
       private ElasticsearchTemplate elasticsearchTemplate;
   
     
       /**条件查询*/
       @Test
       public void findByTitle(){
           String condition = "版本";
           List<Article> articleList = articleService.findByTitle(condition);
           for(Article article:articleList){
               System.out.println(article);
           }
       }
   
       /**条件分页查询*/
       @Test
       public void findByTitlePage(){
           String condition = "版本";
           Pageable pageable = PageRequest.of(2,10);
           Page<Article> page = articleService.findByTitle(condition,pageable);
           for(Article article:page.getContent()){
               System.out.println(article);
           }
       }
   
       // 使用Elasticsearch的原生查询对象进行查询
       @Test
       public void findByNativeQuery() {
           //创建一个SearchQuery对象
           SearchQuery searchQuery = new NativeSearchQueryBuilder()
                   //设置查询条件，此处可以使用QueryBuilders创建多种查询
                   .withQuery(QueryBuilders.queryStringQuery("备份节点上没有数据").defaultField("title"))
                   //还可以设置分页信息
                   .withPageable(PageRequest.of(1, 5))
                   //创建SearchQuery对象
                   .build();
   
   
           //使用模板对象执行查询
           elasticsearchTemplate.queryForList(searchQuery, Article.class)
                   .forEach(a-> System.out.println(a));
       }
   
   }
   ```

   