---
layout: guides.cn
section: concept
toc: article
title: 基本概念
---

微搜索致力于将搜索尽可能地简化。然而，要明白微搜索是怎么工作的，这里还是有几个相关的概念需要了解。

## 搜索流程

搜索的流程简单来说分为两部分：索引和搜索。

在索引环节，需要将待搜索的`资源`提交到搜索引擎中，搜索引擎对其做相应的处理。例如要对博客里的文章提供搜索功能，需要将博客里的文章提交到搜索引擎中。我们把 **"将待搜索的`资源`提交到搜索引擎中"** 的过程称为 `索引资源`，简称`索引`(`indexing `)

在搜索环节，向搜索引擎提交要搜索的内容，比如一些关键词，搜索引擎会在前一步中索引的资源里进行搜索，返回符合搜索要求的`资源`，比如返回一系列文章。值得注意的是，`自动补全`是一种特殊的`搜索`，在不引起歧义的情况下，我们所说的 `搜索` 包含 `自动补全`。

微搜索将`资源`按如下结构组织：

![资源组织结构][resource_structure]

## 资源

### engine

`engine`是资源组织结构中最顶层的概念，类似于 mysql 中的 database，mongodb 中的 db, s3中的 bucket。

每个`engine` 有1个`name`，为全局唯一，可由字母，数字和 ‘-’ 构成。另外每个`engine`有一个`engine_key`，可用于调用 [搜索 API][search_api] 时指定 `engine`。

### collection

`collection` 定义了 `document` 的 ‘schema’，包含了拥有类似 ‘schema’ 的一系列 `document`，类似于 mysql 中的 table, mongodb 中的 collection。

每个`collection` 有1个`name`，在同一个`engine`中唯一，可由字母，数字和 ‘-’ 构成。另外每个`collection`还有一个`field_types`，描述了所包含`document`的 ‘schema’。例如：

```json
{
  "title": "string",
  "tags": "string",
  "published_date": "date",
  "url": "enum",
  "body": "text"
}
```

表示 `document` 由 ‘title’, ‘tags’, ‘published_date’, ‘url’, ‘body’ 这些 `field` 组成，每个 `field` 的类型(`type`) 分别是：‘string’, ‘string’, ‘date’, ‘enum’, ‘text’。

`field` `type` 的详细列表，参见下面 [Field Types][field_types] 小节。

### document

微搜索中， `document` 对应于具体的一个个资源，类似于 mysql 中的 record, mongodb 中的 document。每个 `document`拥有一个 id，以及符合所属 `collection` 中规定的 ‘schema’ 内容。例如：

```json
{
  "title": "微搜索上线内测",
  "tags": ["公告"],
  "published_date": "2014-08-16T00:00:00Z",
  "url": "http://blog.tinysou.com/cn/2014/08/09/%E5%BE%AE%E6%90%9C%E7%B4%A2%E4%B8%8A%E7%BA%BF%E5%86%85%E6%B5%8B.html",
  "body": "我们很高兴地在这里宣布，今天，微搜索正式开始上线内测了！"
}
```

## 爬虫

微搜索提供两种方式进行`索引资源`：API 和 爬虫。

当使用 API 时，用户可以自己定义 `collection`，例如 `name` 和 `field_types`(schema)。这样可以更加可控地进行`索引`。

如果你想利用微搜索进行网页的搜索，并且不想通过 API 的方式进行`索引`，可以使用爬虫方式。

要使用爬虫，只需要提交待搜索网站的网址，微搜索的爬虫就会自动爬取该网站的页面，并进行`索引`。添加方式如下图：

![添加域名][add-domain]

[resource_structure]:resource_structure.png
[add-domain]:add-domain.png
[field_types]:/v1/overview.html#3-Field-Types
[search_api]:/v1/searching.html
