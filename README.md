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
    #### Setup ElasticSearch
      1. Download [ElasticSearch](https://www.elastic.co/downloads/elasticsearch).
      3. Extrac the `ElasticSearch` zip file and run `bin/elasticsearch` or `bin\elasticsearch.bat` on Windows.
      4. On Windows, we can run the `elasticsearch-service.bat` file to make it run in the background without having our terminal open. After running the command, we can start `elasticsearch` using terminal with `elasticsearch-service start` and we can now close the terminal if we want. `ElasticSearch` is running
      5. I get `ERROR: Failed to determine the health of the cluster. , with exit code 69` when running `elasticsearch.bat` file directly in the `bin` folder so make sure to cd out of the `bin` folder and run `bin/elasticsearch-reset-password -u elastic`
      6. If encounter `received plaintext http traffic on an https channel, closing connection Netty4HttpChannel` error. We can set `xpack` security enable to `false` in config file or run this command when starting `ElasticSearch`
      > bin/elasticsearch -E xpack.security.enabled=false
      7. Open browser, go to `http://localhost:9200` or run `curl hhtp://localhost:9200/` or `Invoke-RestMethod http://localhost:9200` with Powershell. If it returns JSON file with our cluster information, everything is working correctly.
      8. So far, so good. The problem with this is when searching for Integrations after installing Elastic Agent, we will encounter this error `Kibana cannot connect to the Elastic Package Registry, which provides Elastic Agent integrations`. To fix it we will need to try these steps.
      9. configure x-pack to true in elasticsearc.yml, restart both elasticsearch and kibana
      10. You will need the following commands on terminal.
      
      ```
      bin\elasticsearch-create-enrollment-token --scope kibana
      bin\kibana-verification-code // ***do not clear the terminal before copying the code. We will need it for Kibana***
      bin\elasticsearch-reset-password -u elastic -i
      ```

      After this, we can access our cluster on a browser by going to `https://localhost:9200` instead of http and enter username and password.

    #### Setup Kibana
      1. Download and extract [Kibana](https://www.elastic.co/downloads/kibana). 
      2. Learn your lesson. Do not cd to the `bin` folder. Just go to Kibana folder and run `bin\kibana.bat`. It will take a while to run ... maybe 15 minutes or more... no kidding. When it finishes we can connect to Kibana from its port `5601`

  ### first steps.
    With ElasticSearch and Kibana setup, we can now start working on our cluster.
    1. Firstly, we can start with the config file. That file will have default value we can change.
      - Change cluster name: Uncommend the `#cluster.name: my-application` line and edit `my-application`.
      - Change node name: Uncommend `#node.name: node-1` line and edit `node-1`.
      - To apply changes, we need to stop and restart `ElasticSearch` and `kibana`. Unfortunately.

    2. We can test our cluster from kibana by going to our local port 5601, select hamburger icon at top left, `Dev tools`, make `GET _cluster/health` request to get our cluster status.
    3. Get info about nodes in a cluster: 
    > GET _nodes/stats
    4. ***WE KNOW THAT ELASTIC USES INDEX FOR OUR DATA SO WE CAN START OUR FIRST STEPS WITH CREATING A NEW INDEX***
      - > PUT Name-of-the-Index
      - More details on working with `indexes` we can go [here](https://github.com/LisaHJung/Part-1-Intro-to-Elasticsearch-and-Kibana?tab=readme-ov-file#c---create)

## Elastic Agent.
  1. Elastic Agent helps running ELK.
  2. Download Elastic agent [here](https://www.elastic.co/downloads/elastic-agent).
  3. Unzip, open and edit the elastic-agent.yml file to our ElasticSearch.
  4. Run `elastic-agent.exe install`.
  5. Check if agent is running by go to Kibana => Stack Management => Index management => Data Stream. If we have data on the list, then our agent is running.
  
## Integrate with 1Password.
  1. To integrate with 1Password. We need to setup agent. If we currently have a running agent, we can restart it by uninstalling and reinstalling it.
  ```
   C:\"Program Files"\Elastic\Agent\elastic-agent.exe uninstall
   elastic-agent.exe install
  ```