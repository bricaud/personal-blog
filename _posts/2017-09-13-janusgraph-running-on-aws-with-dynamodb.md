---
layout: post
comments: true
title: Setting up JanusGraph on AWS using EC2 and DynamoDB
categories: [graph, coding]
thumbnail: /images/janusGraphInstall/gremlinconsole.png
---

Graph Databases are not yet widely used and it is still not completely straighforward to run one in the cloud. I describe here the different steps I made to install and run the JanusGraph database, using the NoSQL database DynamoDB as a storage backend. The cloud is the one from Amazon (AWS).

# Architecture

To run the graph database, the idea is use the possibilities offered by a cloud service such as AWS. The [JanusGraph](http://janusgraph.org/) is a scalable graph database for which you can choose a backend NoSQL database, such as Cassandra, Hbase or BerkeleyDB, for storing the data. The licence is Apache 2.0 and the project is owned by the Linux Fundation (It is a fork from the Titan graph database). The repository is [here](https://github.com/JanusGraph/janusgraph). Although not in the official list, it is also possible to choose Amazon's [DynamoDB](https://en.wikipedia.org/wiki/Amazon_DynamoDB). This latter database is the one provided as a service in AWS. You can start it in a few clicks and you are billed on the amount of data it contains. You do not need any computer ressources, everything is handled by Amazon.

The idea is to run the JanusGraph on an [EC2](https://en.wikipedia.org/wiki/Amazon_Elastic_Compute_Cloud) machine and make the interface with the DynamoDB for storing the data. Optionally, you may also set up an access to a disk storage (with [S3](https://en.wikipedia.org/wiki/Amazon_S3)) where you can save the configuration files.

![Architecture view](/images/janusGraph/install/janusGraphSchema.png)

In order to help setting up the graph database, the AWS team provides some [code and directions](https://github.com/awslabs/dynamodb-janusgraph-storage-backend). All is in there but the guidelines where not exactly leading to what I wanted. I did not want to run the script that launch an EC2 instance fully configured. I wanted to install JanusGraph on my already made EC2 instance and be able to add more applications on this instance.

## Set up the EC2 instance

First you need an EC2 instance where to install the graph server. Go to the [AWS console](http://console.aws.amazon.com/) and launch one if it is not already done. Get your keys and connect to it using ssh.
Install the pre-requisites (first steps in the [awslabs' repo](https://github.com/awslabs/dynamodb-janusgraph-storage-backend)):

```
curl https://raw.githubusercontent.com/awslabs/dynamodb-janusgraph-storage-backend/master/src/test/resources/install-reqs.sh | bash
exit
```
and clone the repository

```
git clone https://github.com/awslabs/dynamodb-janusgraph-storage-backend.git && cd dynamodb-janusgraph-storage-backend
```

Now, inside the clone repository, run
```
src/test/resources/install-gremlin-server.sh
```

When this is done,

```
cd server/dynamodb-janusgraph-storage-backend-1.1.1
```
In the `conf` folder, you will find the configuration files. You may have to modify the file `dynamodb.properties`, in particular the line

```
storage.dynamodb.client.signing-region=us-west-2
```
with your region. You may do it with the nano editor
```
nano conf/dynamodb.properties
```


## Set up DynamoDB
You need DynamoDB and database tables configured to receive the data from JanusGraph. Go [here](https://console.aws.amazon.com/dynamodb/home) to check whether you can have access to it.
To create the table, you need to run the [script provided here](https://github.com/awslabs/dynamodb-janusgraph-storage-backend#cloudformation-template-table) in [CloudFormation](https://aws.amazon.com/cloudformation). You may access CoudFormation from the [AWS console](http://console.aws.amazon.com/), then create a new stack. Download the script and choose to create the stack from this file.

## Set up S3
If you do not have any S3 service go to the [AWS console](http://console.aws.amazon.com/) to create one.

## Allow interactions between EC2 and dynamoDB
To allow EC2 to access DynamoDB and S3, create a new [IAM role](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#attach-iam-role) and attach it to the EC2 instance. In this IAM role, choose full access to dynamoDB and read (or full) access to S3.

## Running the server

Coming back to the EC2 instance, launch the server:

```
cd /home/ec2-user/dynamodb-janusgraph-storage-backend/server/dynamodb-janusgraph-storage-backend-1.1.1
bin/gremlin-server.sh /home/ec2-user/dynamodb-janusgraph-storage-backend/server/dynamodb-janusgraph-storage-backend-1.1.1/conf/gremlin-server/gremlin-server.yaml
```

## Accessing the graph database

In the same directory,
```
bin/gremlin.sh
```
and inside the console, connect to the server:
```
:remote connect tinkerpop.server conf/remote.yaml session
:remote console
```

You should be able to query the graph database.