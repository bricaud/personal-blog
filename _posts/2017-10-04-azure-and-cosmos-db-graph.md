---
layout: post
comments: true
title: Testing the Cosmos DB graph database
categories: [graph, coding, cloud]
thumbnail: /images/cosmosgraph
published: false
---

Graph databases raise more and more interest as alternatives to standard SQL databases. The graph structure may be better suited when queries are focusing on relationships between entities stored in the database. Cloud companies have spotted this trend and provide new solutions to set up graph databases in the cloud. Amazon and Google have made the choice of providing ways to connect JanusGraph to DynamoDB and BigTable respectively. Microsoft has chosen to provide its own solution for the server while relying on Gremlin for handling queries. It is important to understand that graph databases are cutting edge tools, they are rapidly evolving, with new versions coming at a fast pace and scarse documentation and tutorials. It may be difficult to grasp for beginners. I propose here a tutorial for setting up a graph database on the Microsoft cloud "Azure" as well as a small test of it, giving my first impressions. A graph database solution has been recently released within Cosmos db.

# Introduction

Graph databases and their pluggins/modules are rapidly evolving and I am convinced that the present tutorial will be quickly out of date. Because of its lack of maturity, this field of engineering is not as neat and documented as what we are used to (for instance with SQL). If I point out the weaknesses here, I also acknowledge the efforts and work of the persons designing the solutions or providing open source code that simplify the life of users. I want to thank them for helping widespread the graph approach and the elegance of graph modelling. I encourage courageous or curious minds to try out and discover the word of graphs.

# Setting up the Graph database server

Microsoft has made the choice of providing a graph database service easy to set up and start. Indeed, in a few clicks, the server is up and running. Without exagerating much, it is one click to create a Cosmos database and one click to create the graph DB. The Gremlin server is automatically launched with a domain name `YOURGRAPHNAME.graphs.azure.com` ready to listen any ssl connection on port `443`. It avoids the burden of configuring the database. You just need to choose the name of your graph (and of the collection). The [tutorial](https://docs.microsoft.com/en-us/azure/cosmos-db/create-graph-gremlin-console) for creating the graph is straight forward (first part of the web page).

![Screenshot of the Cosmos DB dashboard]({{ site.baseurl }}/images/cosmosGraph/cosmosdbdashboard.png "The Cosmos DB dashboard")

The first impression is pretty good. The user and password for the ssl connection is automatically generated, and ready to use. In addition, no need to configure the Gremlin server or its interaction with the Cosmos database. This is quite comfortable, even more when you compare to the set up of a graph database on AWS (see my previous [blog post]({{ site.baseurl }}{% post_url 2017-09-13-janusgraph-running-on-aws-with-dynamodb %})).

However, at least to me, it looks like an oversimplification to the expense of flexibility. The troubles appear as soon as you want to communicate with the server...

# Connecting to the Gremlin server

 Several tutorials are available to explain how to interact with the server. They cover .Net, Java, Javascript and the Gremlin Console.

 As you may know I am a big fan of Python and I felt a bit frustrated  when I noticed I can not use it to query the server [^1].

 So I decided to have a try with a different language and I picked the popular Javascript (Node.js). At first I found it confusing as the address of the graph given by the Azure portal contains `https` while the gremlin module for Javascript explicitly states that it only handles websocket connections. I found out that, although not documented, you can connect (and you do in all the tutorials!) to the server using secure websocket `wss`. I was also a bit perplex about the [javascript module](https://github.com/CosmosDB/gremlin-javascript) used which is just a fork of a project made by a contributor of Gremlin Tinkerpop. This module is not in the official Tinkerpop repository. Of course, being on the repository of an individual does not preclude a an efficient module of good quality. It just raises questions about the continuity of the work. Microsoft engineers have started to contribute to this module, adding security handling, so it is going in a good direction.

~~~ javascript
const client = Gremlin.createClient(
    443, 
    config.endpoint, 
    { 
        "session": false, 
        "ssl": true, 
        "user": `/dbs/${config.database}/colls/${config.collection}`,
        "password": config.primaryKey
    });
~~~

# Test of th writing in the data base

# Questions about the optimization and tuning of the database

The [list of Gremlin steps](https://docs.microsoft.com/en-us/azure/cosmos-db/gremlin-support) supported by Cosmos DB does not yet contain all the possible steps. However, this reduced list will be sufficient for most usecases and for users willing to learn Gremlin and graph databases. Strangely, the steps `loop` and `choose` are stated on this [tutorial-query-graph page](https://docs.microsoft.com/en-us/azure/cosmos-db/tutorial-query-graph) but they are not on the list of supported steps.

Indexing is really important in graph databases as you need to start from somewhere before you travel through the connections and relationships. The starting point(s) is (are) chosen according to a keyword or a value and if there is not any index, you have to scan the full graph to find them. According to Azure webpages, all entries in the DB are automatically indexed (vertex and edge properties) avoiding the need to manually creating them. However, it does not allow for partial or fuzzy matching. For example, with JanusGraph, users can combine Elasticsearch with the graph, which makes the keywords `textContains` or `textRegex` [available as gremlin predicates](http://docs.janusgraph.org/latest/search-predicates.html), to search in strings.

When you start setting up a database, you may need to load a large amount of data at the beginning. If you need to load a large amount of data at once, you can rely on specific functions that speed up the process. [Here is an example](http://docs.janusgraph.org/latest/bulk-loading.html) concerning JanusGraph. You may also rely on the Gremlin console with [bulkloadervertexprogram](http://tinkerpop.apache.org/docs/3.1.0-incubating/#bulkloadervertexprogram). It is available for several backends but I do not know if this works with Cosmos. The Azure tutorials do not say anything about bulk loading. Still, relying on the `bindings` mechanism in traversals (see this [thread](https://groups.google.com/forum/?utm_medium=email&utm_source=footer#!msg/gremlin-users/lE67mACc3QM/xrdvkjniAwAJ)) is always available and should be fine for most users.

# Conclusion

To conclude, the graph setting of Cosmos DB is perfectly suited for users willing to learn and get a grip on graph models and a graph database. Azure avoid us the painful configuration of the server and the database, and we can concentrate on learning the graph logic and the Gremlin language. Meanwhile, the service will evolve and hopefully provide more flexibility for the configuration in the near future.


[^1]: There is an issue with the serialization, the way the server receive and send data. The Cosmos DB graph is configured to accept only data in GraphSON v1 format (see [here](https://docs.microsoft.com/en-us/azure/cosmos-db/create-graph-gremlin-console)). Unfortunately, [pythongremlin](http://tinkerpop.apache.org/docs/current/reference/#gremlin-python) only handle GraphSON v2. (At the time of writing the latest version of gremlinpython is 3.3.0). This is not a big issue and I am confident this will be solved quickly.