---
layout: post
comments: true
title: Testing the Cosmos DB graph database
categories: [graph, coding, cloud]
thumbnail: /images/cosmosGraph/cosmosdbdashboard.png
published: true
---

Graph databases raise more and more interest as alternatives to standard SQL databases. Indeed, the graph structure may be better suited when queries are focusing on relationships between entities stored in the database. Cloud companies have spotted this trend and provide new solutions to set up graph databases in the cloud. Amazon and Google have made the choice of providing ways to connect JanusGraph to DynamoDB and BigTable respectively. Microsoft has chosen to provide its own graph database while relying on Gremlin for handling queries. Azure is using Cosmos DB as a backend for it. I have tested the Cosmos graph DB and I want here to share my first impressions. 



# Introduction

Graph databases and their surrounding plugins/modules are rapidly evolving and I am convinced that the present tutorial will be quickly out of date. Because of its lack of maturity, this field of engineering is not as neat and documented as what we are used to (for instance with SQL). If I point out the weaknesses here, I also acknowledge the efforts and work of the persons designing the solutions or providing open source code that simplify the life of users. I want to thank them for helping widespread the graph approach and the elegance of graph modeling. It is time now to encourage curious minds to try out and discover the word of graphs.

# Setting up the Graph database server

Microsoft provides a graph database service easy to set up and start. Indeed, in a few clicks, the server is up and running. Without exaggerating much, it is one click to create a Cosmos database and one click to create the graph DB. The Gremlin server is automatically launched with a domain name `YOURGRAPHNAME.graphs.azure.com`, ready to listen to any ssl connection on port `443`. It avoids the burden of configuring the database. You just need to choose the name of your graph (and of the collection). The [tutorial](https://docs.microsoft.com/en-us/azure/cosmos-db/create-graph-gremlin-console) for creating the graph is straightforward (first part of the webpage).

![Screenshot of the Cosmos DB dashboard]({{ site.baseurl }}/images/cosmosGraph/cosmosdbdashboard.png "The Cosmos DB dashboard")

The first impression is pretty good. The user and password for the ssl connection are automatically generated, and ready to use. In addition, no need to configure the Gremlin server or its interaction with the Cosmos database. This is quite comfortable, even more when you compare to the set up of a graph database on AWS (see my previous [blog post]({{ site.baseurl }}{% post_url 2017-09-13-janusgraph-running-on-aws-with-dynamodb %})).


# Connecting to the Gremlin server

 Several tutorials are available to explain how to interact with the server. They cover [.Net](https://docs.microsoft.com/en-us/azure/cosmos-db/create-graph-dotnet), [Java](https://docs.microsoft.com/en-us/azure/cosmos-db/create-graph-java), [Javascript](https://docs.microsoft.com/en-us/azure/cosmos-db/create-graph-nodejs) and the [Gremlin Console](https://docs.microsoft.com/en-us/azure/cosmos-db/create-graph-gremlin-console).

 As you may know, I am a big fan of Python and I felt a bit frustrated when I noticed I can not use it to query the server [[^1]].

 So I decided to have a try with a different language and I picked the popular Javascript (Node.js). At first I found it confusing as the address of the graph given by the Azure portal contains `https` while the gremlin module for Javascript explicitly states that it only handles websocket connections. I found out that, although not documented, you can connect (and you do in all the tutorials!) to the server using secure websocket `wss`. I was also a bit perplex about the [javascript module](https://github.com/CosmosDB/gremlin-javascript) used which is just a fork of a project made by a contributor of Gremlin Tinkerpop. This module is not in the official Tinkerpop repository. Of course, being on the repository of an individual does not preclude an efficient module of good quality. It just raises questions about the continuity of the work. Microsoft engineers have started to contribute to this module, adding security handling, so it is going in a good direction.

You may connect to the Gremlin server using the function `Gremlin.createClient` (form the gremlin module), specifying the port `443`, the address `config.endpoint` and the ssl credentials `user` and `password` (stored in the config file, `config.js`).

{% highlight javascript %}
const client = Gremlin.createClient(
    443, 
    config.endpoint, 
    { 
        "session": false, 
        "ssl": true, 
        "user": `/dbs/${config.database}/colls/${config.collection}`,
        "password": config.primaryKey
    });
{% endhighlight %}

Then a query can be sent using the `client.execute` function. That's it! Pretty easy. You have to find by yourself the structure of the returned data but that should not be too difficult, it uses the JSON format. For each vertex, the data is organized as a dictionary with keys `id`, `label`, `type` and `properties`. The value associated to `properties` is again a dictionary with all property names as keys, and values being lists. 

# Writing to the database

I have used and modified the [code from the javascript tutorial](https://github.com/Azure-Samples/azure-cosmos-db-graph-nodejs-getting-started) to insert some data to the database. I wanted to know the speed of loading data into the graph. No batch / bulk loading here. This can only be done by sending Gremlin requests to the server, and you have to insert one node at a time (one per gremlin query). Typically, it is of the following form if you want to add a person with name `Benjamin`:

~~~ javascript
var gremlin_query = "g.addV('person').property('name', name)"
var bindings = {name: 'Benjamin'}
client.execute(gremlin_query, bindings, function_to_process_the_results);
~~~

The `function_to_process_the_results` may be a function that calls recursively `client.execute`, in order to process sequencially the requests. In that case the bindings are changing with the node to add. For the `i`th node and assuming the node data is stored in an object called `data[i]`:

~~~ javascript
var node_name = data[i].name
var bindings = {name: node_name}
~~~

*(The following part was updated on 12th October 2017, thanks to the comments and suggestions of [jbmusso](https://github.com/jbmusso) and of the Cosmos graph DB team)*.

According to my tests, If I simply send the requests for writing nodes one by one, the writing speed is roughly around 6 to 12 nodes per second (it is fluctuating, but I don't know exactly why. It might be because of my internet connection). However, if I run two such sequential requests in parallel, it takes roughly the same time, but it writes twice the number of nodes. It shows that the Cosmos DB database is not to be blamed here. It answers nicely to the requests in parallel. It just says that loading a large number of nodes sequentially to a graph database is not the most efficient way. I did not perform the test on other graph databases but this should hold for any databases (not only Cosmos DB). 

A function that efficiently loads large datasets into a graph database is something which would be quite useful and is missing at the moment. According to the Cosmos graph DB team, who contacted me, they are working on a bulk import tool. I look forward to it! Meanwhile, I may make a pull request with such a function to [gremlin-javascript](https://github.com/jbmusso/gremlin-javascript).

# Optimizing and tuning of the database

The [list of Gremlin steps](https://docs.microsoft.com/en-us/azure/cosmos-db/gremlin-support) supported by Cosmos DB does not yet contain all the possible steps. However, this reduced list will be sufficient for most use-cases and for users willing to learn Gremlin and graph databases. Strangely, the steps `loop` and `choose` are stated on this [tutorial-query-graph page](https://docs.microsoft.com/en-us/azure/cosmos-db/tutorial-query-graph) but they are not on the list of supported steps.

Indexing is really important in graph databases as you need to start from somewhere before you travel through the connections and relationships. The starting point(s) is (are) chosen according to a keyword or a value and if there is no index, you have to scan the full graph to find them. According to Azure webpages, all entries in the DB are automatically indexed (vertex and edge properties) avoiding the need to manually creating them. However, it does not allow for partial or fuzzy matching. For example, with JanusGraph, users can combine Elasticsearch with the graph, which makes the keywords `textContains` or `textRegex` [available as gremlin predicates](http://docs.janusgraph.org/latest/search-predicates.html), to search in strings.

As we have seen in the previous section, when you start setting up a database, you may need to load a large amount of data at the beginning. If you need to load a large amount of data at once, you can rely on specific functions that speed up the process. [Here is an example](http://docs.janusgraph.org/latest/bulk-loading.html) concerning JanusGraph. You may also rely on the Gremlin console with [bulkloadervertexprogram](http://tinkerpop.apache.org/docs/3.1.0-incubating/#bulkloadervertexprogram). It is available for several backends but I do not know if this works with Cosmos. The Azure tutorials do not say anything about bulk loading. Still, relying on the `bindings` mechanism in traversals (see this [thread](https://groups.google.com/forum/?utm_medium=email&utm_source=footer#!msg/gremlin-users/lE67mACc3QM/xrdvkjniAwAJ)) is always available and should be fine for most users.

# Conclusion

To conclude, the graph setting of Cosmos DB is perfectly suited for users willing to learn and get a grip on graph models and a graph database. Azure avoid us the painful configuration of the server and the database, and we can concentrate on learning the graph logic and the Gremlin language. However, this simplicity does not allow for flexibility and the advanced graph users might be a bit disappointed. Meanwhile, the service will evolve and hopefully provide more advanced settings, more tutorials and documentation, and more open-source code at our disposal in the near future.

It is important to understand that graph databases are cutting edge tools, they are rapidly evolving, with new versions coming at a fast pace and scarce documentation and tutorials. We are just at the beginning of this new concept, this new way of thinking and structuring data. Observing its evolution is fascinating.


[^1]: There is an issue with the serialization, the way the server receive and send data. The Cosmos DB graph is configured to accept only data in GraphSON v1 format (see [here](https://docs.microsoft.com/en-us/azure/cosmos-db/create-graph-gremlin-console)). Unfortunately, [pythongremlin](http://tinkerpop.apache.org/docs/current/reference/#gremlin-python) only handle GraphSON v2. (At the time of writing the latest version of gremlinpython is 3.3.0).