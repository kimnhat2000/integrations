# integrations## Introduction
- Elastic Stack is a free and Opensource project to community.
- Elastic Stack is a collection of different products.
  1. Beats.
  2. Logstash.
  3. Elastic Search.
  4. Kibana.
- We will talk more about the Elastic products latter.

## ElasticSearch
  ### Introduction.
  ElasticSearch is known as the heart of Elastic Stack where we can use to **search, store and analize data**
    - ***Work flow:*** 
      - *From user perspective*: When a user send a search request to a server, the server will send the query to ElasticSearch, ElasticSearch does it job to get the most relevant returns and send it back to the server and server will send it to the user.
      `Users' queries => Server => ElasticSearch => Server => User.`
      - *From app perspective*: When an app send a query to server, the server reaches out to ElasticSearch and ElasticSearch delivers required info back to server and the app. However, an application usually gets huge influx of data from ElasticSearch and to help visualize things easier, we can use`Kibana`. With `Kibana`, we can make data looks easy to understand. It has different visual tools for every kind of data. 

  ### ElasticSearch architechture.
  1. After installing ElasticSearch, we have an instance of ElasticSearch running called a `Node` with a unique ID and name that belong to a single cluster. We can have as many nodes as we want. All nodes, of course run on the same cluster.
  2. Node might have different roles but the main role we focus on our job is the holding data role.
  3. When creating data, we store data in an `index`. For example, we can create a `group index` and `user indexes` links to `group index`
  4. Indexes does not hold data. It just helps pointing to data.
  5. When adding an `Index` to our node, it will be assiged with a `shard` because we cannot find Indexes on disk. We need to use `shard`. by default one `shard` comes with a new index and we can configure for one index to hold multiple `shard` across different nodes.
  6. Since a `shard` is where documents are hold and sorted, the number of `shard` in a node can have depends on the node resource. For example, we want to create an index for 600k documents about a product in your cluster but a node in our cluster can only hold 200k documents. In this case, we will need to create two more notes and assign two new `shards` for our index. 
  7. With `shard` comes a term `sharding`.
    - `Sharding` helps scaling our nodes in case our index grows and our current notes reach their capacity. 
    - `Sharding` also help with queried. For example, we create a product index that holds 500k documents. If we put all 500k documents in one note, it will take longer to process so `sharding` our index through multiple nodes drastically reduce the search time. In our case, if we have ten nodes instead of one, each node will only hold 50k documents and we can reduce the search data ten times faster.
  8. If we store our `shard` in multiple nodes and lost one node. Data in that node will be gone forever so we should have a replica `shard`. So we have `Primary Shards` and `Replica Shard` that automatically scale our cluster horizontally by adding more nodes as out data grows

  ### Setup ElasticSearch and Kibana.
  1. Download [ElasticSearch](https://www.elastic.co/downloads/elasticsearch).
  2. Download [Kibana](https://www.elastic.co/downloads/kibana). 
  3. Extrac the ElasticSearch zip file and run `bin/elasticsearch` or `bin\elasticsearch.bat` on Windows.
  4. Extrac 
  3. Run `curl hhtp://localhost:9200/` or `Invoke-RestMethod http://localhost:9200` with Powershell.
  4. 