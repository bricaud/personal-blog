---
layout: post
title: How graphs can help you find information in data
---

If you think graphs and networks techniques are only useful for graph datasets or data structured as network of entities, you are wrong. You can leverage powerful graph methods by designing a graph from unstructured data. In this example I show how to extract information from a set of texts (emails). At the end of this post you will discover what H. Clinton was talking about in her emails, on which topics she was exchanging information and who/what was involved in each topic.

The nice post of my colleague and friend [Volodymir](http://blog.miz.space/) on [Wikipedia graph mining](http://blog.miz.space/research/2017/08/14/wikipedia-collective-memory-dynamic-graph-analysis-graphx-spark-scala-time-series-network/), made me think that I should blog as well on my exploration of graphs and data analysis. Blogging is an interesting way to spread some science to a broader audience and scientists should do that more often. Also, this could help me show the fascinating world of graphs and networks and how it can be useful in practice. So here is my first contribution. 

## The data

The goal of this post is to demonstrate with an example that one can build a graph from some unstructured dataset to extract valuable information. Our dataset will be a set of texts. I have chosen Hillary Clinton's emails as they are publicly available, most of the world has been aware of this controversy or can easily get some information about it on [wikipedia](https://en.wikipedia.org/wiki/Hillary_Clinton_email_controversy). We can find the dataset on Kaggle [here](https://www.kaggle.com/kaggle/hillary-clinton-emails). This dataset contains the emails sent and received by Hillary Clinton between 2009 and 2011 in her private mailbox. As I said, the data has been made freely accessible to the public, except for some parts which have been censored by the US government. There are 7945 emails.

## The graph

The code for the data processing and graph design is available on [my github account](https://github.com/bricaud/HCmails).

The idea is to extract some keywords form the emails and see how they relate together. From this, we could infer some interesting stories about politics and the work of H. C. 

### Nodes

We want to find some important words in the texts, and get rid of the useless articles for example. We could use a Natural Language Processing toolbox, there are several in Python. However, I want to keep this example simple. So we will select the proper nouns in the texts which can be found easily because they begin with a capital letter. Unfortunately, not all words beginning with a Capital letter are proper nouns. The first word of each sentence has a capital as well. To avoid to problem, we can get rid of the words that appear frequently both with or without a capital letter. It is sign that they are not proper nouns. This selection process is not perfect but it is a compromise between complexity and efficiency.

The keywords that have just been extracted from the corpus will form the nodes of our graph.


### Edges

Let us connect the nodes together if the keywords associated can be found in the same email. Moreover, we will associate a weight to the link, proportional to the number of text where we have found them together. We expect to find clusters of well connected words, forming topics.


## The Visualization and analysis

![Graph of words]({{ site.baseurl }}/images/clintonmails.png "Graph of words appearing in Clinton's emails")

This is an example of a visualization of a network of words. A look at the graph provides interesting insights into this dataset of emails. Topics discussed in the emails can be guessed from the figure and the labels.


The nodes of the graph represent proper nouns found in the emails. They are connected if the nouns have been found together in several mails. In order to separate nodes into groups or clusters, a community detection algorithm has been applied to the graph. On the figure, each color represents a different community. A closer look shows that the communities are related to particular topics:

* Middle East (dark purple)
* Iran Pakistan (dark green)
* North Africa (blue)
* National politics (light green, red and pink)
* Europe (light purple)
* South America (light orange)

As expected, H. Clinton messages are mainly focused on Foreign affairs. At that time, the main concerns were about middle east and muslim countries. 

Some themes are clear and well-separated, some other not. For example if you drag the node "Japan" (in light orange), it has been classified in the same cluster as south american countries and has a link with Brazil (that would be interesting to know why!). Notice that it is the case for Korea as well. It is also connected to the word "Asia" in the dark green cluster, and also to China in the light green cluster. These connections are of course not surprising and shows a glimpse of the complexity of international relationships. 
![Graph of words Middle East]({{ site.baseurl }}/images/zoommiddleeast.png "Zoom on the Middle East")

Concerning the relevance of the data displayed, we can point out a few uninformative nodes. The visualization could be improved further with an additional cleaning of the dataset. For example, "Clinton" should be removed (unless it refers to Bill) as everything is obviously related to Hillary. This node is a hub, with many connections, that add some confusion to the graph and the connected topics. Word containing numbers or nodes with month names should also be removed.
