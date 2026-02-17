# 《Elasticsearch in Action, Second Edition》- Fetch the

## 一、本章概述

**页码范围**：第 205 - 593 页

If you think we can make the same updates by calling the GET, UPDATE, and POST meth-

ods on the document individually, you are absolutely right. In reality, this is what Elas-

## 二、核心概念

- **Updating**
- **documents**
- **If**
- **you**
- **think**
- **we**
- **can**
- **make**
- **the**
- **same**

## 三、关键技术点

### 3.1 重要原理

3.5 and 4.5, flights canceled between two dates, and so on. We can use a range query
-XPOST localhost:9200/_bulk
--data-binary "@movie_bulk_data.json"
- _create-This API throws an error if the document already exists.
- _mget-This API lets us retrieve multiple documents at once, given their IDs.

### 3.2 算法伪代码

```java
public void createIndexUsingClient(String indexName) throws IOException {
 ElasticsearchIndicesClient elasticsearchIndicesClient =
 this.elasticsearchClient.indices();
 CreateIndexRequest createIndexRequest =
new CreateIndexRequest.Builder().index(indexName).build();
CreateIndexResponse createIndexResponse =
 elasticsearchIndicesClient.create(createIndexRequest);
System.out.println("Index created successfully: "+createIndexResponse);
}
```

```java
public void createIndexUsingBuilder(String indexName) throws IOException {
CreateIndexResponse createIndexResponse = this.elasticsearchClient
 .indices().create(new CreateIndexRequest.Builder()
 .index(indexName)
 .build());
System.out.println("Index created successfully using 
 Builder"+createIndexResponse);
}
```

```java
public void createIndexUsingLambda(String indexName) throws IOException {
CreateIndexResponse createIndexResponse = 
this.elasticsearchClient.indices().create(
 request -> request.index(indexName)
 );
System.out.println("Index created successfully using Lambda"
 +createIndexResponse);
}
```

```java
public void indexDocument(String indexName, Flight flight) throws IOException 
{
 IndexResponse indexResponse = this.elasticsearchClient.index(
 i -> i.index(indexName)
 .document(flight)
 );
 System.out.println("Document indexed successfully"+indexResponse);
}
```

```java
public void search(String indexName, String field, String searchText) throws 
IOException {
SearchResponse searchResponse =
 this.elasticsearchClient.search(searchRequest -> searchRequest
 .index(indexName)
 .query(queryBuilder -> queryBuilder
 .match(matchQueryBuilder ->
 matchQueryBuilder
 .field("route")
 .query(searchText)))
 ,Flight.class
 );
List<Flight> flights = 
 (List<Flight>) searchResponse.hits().hits()
 .stream().collect(Collectors.toList());
searchResponse.hits().hits()
 .stream().
```

## 四、架构图/流程图

> 注意：PDF 中的图表需要手动还原为 Mermaid 格式

### 4.1 图表列表

- Figure 5.12 Updating or 
- figure 5.13.
- Figure 5.13 The anatomy of a script
- Figure 5.14 demonstrates this operation.
- Figure 5.14 Upsert operation workflow
- figure 5.15). The request body consists of two lines for every document that
- Figure 5.15 The _bulk API’s generic format
- figure 5.15 is the document’s source, which is what we want to
- figure 5.16 with annotations.
- Figure 5.16 Indexing the new movie Mission Impossible using the _bulk API
- figure 6.1 shows.
- Figure 6.1 Fetching the details of the cars index
- figure 6.2, my_cars_alias is the alternate (second) name of the cars_
- Figure 6.2 Creating an alias for an existing index
- figure 6.3), includ-
- Figure 6.3 Creating an alias pointing to multiple indexes
- Figure 6.4 Achieving migration with zero downtime
- figure 6.5 shows the result of issuing a GET _all command: it returns the entire
- Figure 6.5 Fetching a list of all public and hidden indexes
- figure 6.6.
- Figure 6.6 Composable (index) templates are composed of component templates.
- Figure 6.7 shows the statistics returned by this call.
- Figure 6.7 Statistics for a cars index
- figure 6.8) that we can find in the response primaries and total buckets. Table 6.1
- Figure 6.8 Multiple statistics for an index
- figure 6.9). We can
- Figure 6.9 Detailed information about every segment in a shard
- figure 6.10
- Figure 6.10 Rolling over to a new index when the 
- Figure 6.11 Lifecycle of an index
- figure 7.1 shows, an analyzer module consists of a set of filters and a
- Figure 7.1 Anatomy of an analyzer module
- Figure 7.2 shows an example of the analysis process. We saw this figure in chapter
- Figure 7.2 An example of text analysis in action
- figure 7.3. The output of the
- Figure 7.3 Tokens produced by invoking the _analyze endpoint 
- Figure 7.4 shows the workings of this example input text when
- Figure 7.4 The standard (default) analyzer in action
- Figure 7.5 shows a condensed output of this command in Dev Tools.
- Figure 7.5 The output tokens from a standard analyzer
- figure 7.5. We can replace the value of the
- Figure 7.6 illustrates the standard analyzer with its internal components and anat-
- Figure 7.6 Anatomy of a standard analyzer
- figure 7.6), the filter is disabled by default. We can switch it on by configuring its
- figure 7.7).
- Figure 7.7 Anatomy of a simple analyzer 
- figure 7.8). The following listing shows a script using the
- Figure 7.8 Anatomy of the whitespace analyzer
- figure 7.9 shows, the keyword analyzer is composed of
- Figure 7.9 Anatomy of the keyword analyzer
- figure 7.10).
- Figure 7.10 Anatomy of the fingerprint analyzer
- figure 7.11 shows, the pattern
- Figure 7.11
- figure 7.12).
- Figure 7.12 Anatomy of a custom analyzer
- Figure 7.13 shows these n-grams and edge n-grams.
- Figure 7.13 Pictorial representation of n-grams and edge n-grams
- Figure 8.1 shows how the engine carries
- Figure 8.1 A typical search request, and the mechanics of how a search works
- figure 8.1, Node A is the coordinator node that receives the client’s request. It was
- Figure 8.2 shows the anatomy of a search request.
- Figure 8.2 Components of a search request 
- Figure 8.3 Constituents of a search response 
- figure 8.4. Highlighting keywords in results for our clients is engaging and visually
- Figure 8.4 An example of highlighted text
- figure 8.5 shows.
- Figure 8.5 The explanation of how Elasticsearch calculates relevancy scores
- figure 8.5, three
- figure 8.5 shows 25 documents in our
- Figure 9.1 illustrates index-
- Figure 9.1 Indexing and term-level searching for the movie The Godfather
- figure 9.1). This is because the
- figure 10.1), and a couple are projectors, because LG also pro-
- Figure 10.1 A precision example in action, returning search results for a 4K television
- figure 10.2).
- Figure 10.2 The recall measure in action for our 4K television example search
- Figure 10.3 shows the inverse relationship between these two factors.
- Figure 10.4 summarizes precision and recall.
- Figure 10.3 Precision and recall 
- Figure 10.4 The formulas for precision 
- Figure 11.1 shows the syntax for a com-
- Figure 11.1 Compound query syntax
- Figure 11.2 illustrates the basic structure of a bool query with empty
- Figure 11.2 Syntax of a sample Boolean ( bool) query with four conditional clauses 
- Figure 11.3 shows an example (from a
- Figure 11.3 Hiding out-of-stock 
- Figure 12.1 A georectangle constructed with a set of longitude and latitude coordinates
- figure 12.2, we have a central location (shown as the dropped pin
- Figure 12.3 shows a hexagonal
- Figure 12.3 Finding addresses in a polygonal shape with a geo_shape query
- Figure 12.2 Addresses 
- figure 12.4 shows, we can construct a rectangle using
- Figure 12.4 Georectangle from latitude and longitude coordinates
- figure 12.4. The user can pro-
- figure 12.5 shows, we may want to find
- Figure 12.5 Returning schools with a geo_distance query
- figure 12.6 shows some plots of land on our London map.
- Figure 12.6 Plots of farmland in an area of London
- figure 12.6. We construct this hexagonal shape
- figure 12.6, if we specifically set relation=intersects, the expected plots are A, B,
- Figure 12.7 shows the two shapes: the line and the point. 
- Figure 12.7 Searching for 2D shapes in a bounded envelope
- figure 12.7 to
- Figure 12.8 shows this relationship: Plato is at posi-
- Figure 12.8
- figure 12.9).
- Figure 12.9 Searching for a document with a term in the first n number of tokens 
- figure 12.9). Because Aristotle’s position is fifth, the document consisting of
- figure 12.10
- Figure 12.10 Words expected to 
- figure 12.11.
- Figure 12.11 Finding a word if it exists between other words 
- figure 12.12 shows the approximate distances of these universities from
- Figure 12.12 Map of London showing universities near London Bridge
- figure 12.13
- Figure 12.13 Viewing More Like This 
- Figure 12.14 shows the differences between a normal query
- Figure 12.14 Normal vs. percolate queries
- figure 13.1. The query calculates a lot of advanced statisti-
- Figure 13.1 The extended statistics on 
- Figure 13.2
- figure 13.2 shows that the buckets have two fields:
- Figure 13.2 Book rating 
- figure 13.2.
- Figure 13.3 shows the
- Figure 13.3 Finding the 
- figure 13.4 shows, the cappuccino_sales aggregation is created as a child of the
- Figure 13.5 shows
- Figure 13.4 Parent 
- Figure 13.5 New buckets created inside existing buckets
- figure 13.6.
- figure
13.7 with newly created buckets for each sibling aggregator.
- Figure 13.6 Sibling aggregations 
- Figure 13.7 Sibling queries output 
- Figure 13.8 shows the parent aggregation cappuccino_sales. The pipeline aggre-
- figure 13.9 is a sibling
- Figure 13.8 Parent pipeline 
- Figure 13.9
- Figure 14.1 shows a single-node cluster.
- Figure 14.1 Single-node cluster
- Figure 14.2 shows this (a shard is created, but a replica isn’t).
- Figure 14.2 Single-node cluster with no replica created
- Figure
14 . 3 i s r e p e a t e d f r o m c h a p t e r 3 a n d
- Figure 14.3 Representing the health of a cluster 
- figure
14.4 shows, Elasticsearch instantly creates the replica 1 shard, which is an exact copy
- Figure 14.5 illustrates how a shard (shard 2) is moved to a
- Figure 14.4 The replica shard is 
- Figure 14.5 A newly joined node gets new shards moved from the single-node cluster.
- Figure 14.6 shows this configuration.
- Figure 14.6 Shards and replicas spread across a multi-node cluster
- Figure 14.7 shows these calculations.
- Figure 14.7 Memory sizing of a single index with 10 shards and 2 replicas
- figure 14.8 indi-
- figure 14.7), we com-
- figure 14.8), we need a massive cluster that can handle about
- Figure 14.8
- figure 14.9 shows, administra-
- Figure 14.9 Mechanics of snapshot and restore on a cluster
- figure 14.10). Click the link to navigate to the Stack Management page. Then,
- Figure 14.10 Accessing the 
- figure 14.11. 
- Figure 14.11 Naming the repository and choosing its type
- Figure 14.12 shows this
- Figure 14.12 Snapshot lifecycle, from creation to deletion
- figure 14.13.
- Figure 14.13 Creating an SLM policy using the Kibana Console
- Figure
14.14 shows the Snapshot Settings configurations.
- Figure 14.14 Configuring the snapshot’s settings
- figure 14.15, we ask the snapshot manager to delete this snapshot after a week
- Figure 14.15 Configuring the snapshot’s retention settings
- figure 14.16
- figure 14.17).
- Figure 14.16
- Figure 14.17
- figure 15.1.
- Figure 15.1 shows that the programming_books2 index is unassigned (look at the
- Figure 15.1 Reasons for unassigned shards

## 五、面试题整理

### 5.1 概念理解类

**Q：** Updating 的定义是什么？

**A：** （待补充）

### 5.2 原理分析类

**Q：** 为什么 UPDATE 能解决问题？

**A：** （待补充）

## 六、实践要点

- （根据章节内容补充实践建议）

## 七、扩展阅读

### 7.1 相关章节
### 7.2 推荐资源

## 八、本章小结

---

*Generated by PDF Book Reader*
