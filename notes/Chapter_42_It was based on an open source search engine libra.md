# 《Elasticsearch in Action, Second Edition》- It was based on an open source search engine library called Apache Lucene

## 一、本章概述

**页码范围**：第 42 - 153 页

In this section, we briefly discuss the high-level features of a modern search engine.

Then the following section introduces a couple of search engines available in the mar-

## 二、核心概念

- **CHAPTER**
- **Overview**
- **In**
- **this**
- **section**
- **we**
- **briefly**
- **discuss**
- **the**
- **high**

## 三、关键技术点

### 3.1 重要原理

1.3.2 Popular search engines
1.4 Elasticsearch overview
1.4.3 Elasticsearch use cases
1.4.4 Unsuitable Elasticsearch uses
1.6 Generative AI and modern search

### 3.2 算法伪代码

（本章无代码示例）

## 四、架构图/流程图

> 注意：PDF 中的图表需要手动还原为 Mermaid 格式

### 4.1 图表列表

- figure 1.2. Let’s
- Figure 1.2 Core application areas of Elastic, the company behind Elasticsearch
- figure 1.3, Beats and Logstash bring the
- Figure 1.3 The Elastic Stack: Beats, Logstash, Elasticsearch, and Kibana
- figure 2.1.
- Figure 2.1 A JSON 
- Figure 2.2 shows the syntax of the full URL format for an HTTP PUT
- Figure 2.2 Elasticsearch URL invocation endpoint using an HTTP method
- figure 2.3 (Kibana hides the full URL).
- Figure 2.3 Elasticsearch URL invocation endpoint using cURL
- figure 2.4. As I mentioned,
- Figure 2.4 Transitioning from a cURL command to Kibana’s request command
- figure 2.5. 
- figure 2.6), the code editor opens, showing two
- Figure 2.5 Accessing the Kibana Dev 
- Figure 2.6 Kibana’s Dev Tools code editor
- Figure 2.7 shows the indexing request and a response.
- Figure 2.7 Indexing a document in Kibana (left), and the response from Elasticsearch (right)
- figure 2.6). Kibana sends this request to
- figure
2.7), stores the message, and sends the response back to the client (Kibana). You can
- figure 2.7, the result property indicates
- figure 2.8. 
- Figure 2.8 Elasticsearch’s request and response flow at a high level
- figure 2.6) for
- figure 2.9.
- Figure 2.9 The JSON response for a _count API invocation
- figure 2.10.
- Figure 2.10 Fetching a book document by ID
- figure 2.11, returning
- Figure 2.11 Retrieving documents with a set of IDs using an ids query, which invokes a _search endpoint
- figure 2.12, returns the three documents in our books
- Figure 2.12 Retrieving all documents using the search API
- figure 2.13.
- Figure 2.13 Fetching books authored by Joshua
- Figure 2.14 Fetching an exact match for a title using the and operator 
- Figure 2.15 shows part of the file’s contents. 
- Figure 2.15 Bulk indexing documents using the _bulk endpoint
- figure 2.16 shows an example. The notable point is that aggs is the root-level
- Figure 2.16 Query DSL syntax for finding an average rating aggregation 
- Figure 3.1 shows data being ingested into Elasticsearch via three data sources:
- Figure 3.1 Priming Elasticsearch with data
- figure 3.2).
- Figure 3.2 Search results (as expected) from Google
- figure 3.3 represents a JSON document for a student object.
- Figure 3.3 A student document represented in JSON format
- figure
3.4 may help. Remember, Elasticsearch can be used as a storage server (although I
- figure 3.3 is equivalent to a record in the table of a rela-
- figure 3.4).
- Figure 3.4 A JSON document vs. a relational database table structure
- figure 3.4, the STUDENT and ADDRESS tables are
- figure 3.4) are in the same index as the main fields.
- Figure 3.5 shows a cars index composed of three shards on three nodes (a node
- Figure 3.5 An index designed with three shards and two replicas per shard
- figure 3.6 (one index
- figure 3.6.
- Figure 3.6 Sample Apache web server log file
- Figure 3.7 shows an example data stream for ecommerce order logs generated and
- Figure 3.7 A data stream consists of automatically generated hidden indexes.
- Figure 3.8 shows the inner workings of a Lucene engine
- Figure 3.8 Lucene’s mechanism for indexing documents
- figure
3.9).
- Figure 3.9 The engine is not ready and has a RED status because the shards 
- figure 3.10). Node A joins a newly created single-
- Figure 3.10 A single node with three shards joining a single-node cluster
- figure 3.11
- figure 3.12).
- Figure 3.11 Shards 
- Figure 3.13).
- Figure 3.13 Replicas are lost (or promoted to shards) when a node crashes.
- Figure 3.12 Happy 
- Figure 3.14
- Figure 3.14 A single-node Elasticsearch cluster
- figure 3.15.
- Figure 3.15 Cluster formation from a single node to a multiple-node cluster
- figure 3.16, but doing so depends on an organization’s use cases. 
- Figure 3.16 Varied cluster configurations
- figure 3.17 illustrates.
- Figure 3.17 An inverted index data 
- figure 3.18). The input line <h2>Hello WORLD</
- Figure 3.18 Text analysis procedure where Elasticsearch processes text
- figure 3.19,
- Figure 3.19 Relevant results for Java in a title search
- figure 3.20.
- Figure 3.20 Setting fields with different similarity functions
- Figure 3.21 shows an index with a custom similarity func-
- Figure 3.21 Setting custom parameters on the BM25 similarity function
- figure 3.22.
- Figure 3.22 Creating 
- Figure 4.1 shows the anatomy of the mapping schema for an index. As you can see,
- Figure 4.1 Anatomy of a mapping schema
- figure 4.2.
- Figure 4.2 Movie index 
- figure 4.3. 
- Figure 4.3 Dynamically deriving the indexing schema
- figure 4.4).
- Figure 4.4 A sort operation on a text field results in an error.
- Figure
4.5 demonstrates using both APIs to create a movies index: 
- Figure 4.5 Creating and updating a schema using the indexing and mapping APIs
- figure 4.6).
- Figure 4.6 Creating a mapping definition during index creation
- figure 4.2), deriving types by analyzing field values. We are now at a
- figure 4.7):
- Figure 4.7 Processing a full-text field during indexing with the standard analyzer module

## 五、面试题整理

### 5.1 概念理解类

**Q：** CHAPTER 的定义是什么？

**A：** （待补充）

### 5.2 原理分析类

**Q：** 为什么 search 能解决问题？

**A：** （待补充）

## 六、实践要点

- （根据章节内容补充实践建议）

## 七、扩展阅读

### 7.1 相关章节
### 7.2 推荐资源

## 八、本章小结

---

*Generated by PDF Book Reader*
