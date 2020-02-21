# Basic Concepts
- [Official Doc Link](https://www.elastic.co/guide/en/elasticsearch/reference/current/_basic_concepts.html)
- **Cluster** is collection of one or more nodes (servers) that together holds entire data and provides federated indexing and search capabilities accross all nodes.
- **Node** is single server that is part of cluster, stores data, and participates in indexing and search capabilities. 
- **Index** is a collection of documents that have similar characteristics. 
- **Document** is a basic unit of information that can be indexed. E.g., single customer, single product, single order, etc. It is expressed in `JSON`. 
- **Shards** is to subdivide index into multiple pieces. When creating index, simply define the number of shards you want. Each shard is in itself a fully-functional and independent "index" that can be hosted on any node in the cluster. 
- **Replicas** is making one or more copies of index's shards. Replicas are never allocated on the same node as the original/primary shard that it was copied. 
- Define shards and replicas when creating index. By defautl, 1 index is allocated 5 primary shards, and 1 replica each. After created, you can only change the number of replicas dynamically, but cannot change the number of shards. 

# Cluster Health
- Green - everything is good
- Yellow - all data is available, but some replicas are not yet allocated (cluster is fully functional)
- Red - some data is not available (cluster is partially functional)
```
GET /_cat/health?v
GET /_cat/nodes?v
```

# Indices
- List all indices
```
GET /_cat/indices?v
```
- Create, delete, modify an Index
```
PUT /customer
PUT /customer/_doc/1
{
  "name": "John Doe"
}
GET /customer/_doc/1
DELETE /customer
```
- Add document without specifying ID, Elasticsearch will generate a random ID and use it to index the document. The generated ID is returned as part of the index API call.
```
POST /customer/_doc?pretty
{
  "name": "Jane Doe"
}
```
- To update previous document, use `POST`:
```
POST /customer/_doc/1/_update?pretty
{
  "doc": {"name": "Jane Doe"}
}
```
- Or using scripts to update document, `ctx._source` refers to the current source document that is about to be updated.
```
POST /customer/_doc/1/_update?pretty
{
  "script" : "ctx._source.age += 5"
}
```

# Search API
- Two APi: REST request URI, and REST request body. The request body is more expressive and more readable.
- REST request URI example: 
```
GET /bank/_search?q=*&sort=account_number:asc&pretty
```
- REST request body example: 
```
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```
- `size` specifies how many documents to return, starting from `from` parameter. By default, `size` is 10, and `from` is 0.
- `_source` parameter specifies which fields we want to return:
```
GET /bank/_search
{
  "query": {"match_all": {} },
  "sort": [
    {"account_number": "asc"}
  ],
  "_source": ["account_number", "balance"]
}
```
- `match` parameter is for condition search:
```
GET /bank/_search
{
  "query": { "match": { "account_number": 20 } }
}
```
This example returns account numbered 20.
```
GET /bank/_search
{
  "query": { "match": { "address": "mill lane" } }
}
```
This returns accounts containing the term "mill lane" in the address.

- `bool` query allows to compose smaller queries into bigger queries using boolean logic. 
```
GET /bank/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "age": "40" } }
      ],
      "must_not": [
        { "match": { "state": "ID" } }
      ]
    }
  }
}
```
- `_score` field in results shows how relevant the document matches the query. It is only used for filtering the document set. 
- `range` query allows filter docuemnt by a range of cvalues, generally used for numeric or date filtering: 
```
GET /bank/_search
{
  "query": {
    "bool": {
      "filter": {
        "range": {
          "balance": {
            "gte": 20000,
            "lte": 30000
          }
        }
      }
    }
  }
}
```
- `aggs` procides grouping and extracting statistics from data.
```
GET /bank/_search
{
  "size": 0,
  "aggs": {
    "group_by_age": {
      "range": {
        "field": "age",
        "ranges": [
          {
            "from": 20,
            "to": 30
          },
          {
            "from": 30,
            "to": 40
          },
          {
            "from": 40,
            "to": 50
          }
        ]
      },
      "aggs": {
        "group_by_gender": {
          "terms": {
            "field": "gender.keyword"
          },
          "aggs": {
            "average_balance": {
              "avg": {
                "field": "balance"
              }
            }
          }
        }
      }
    }
  }
}

```









