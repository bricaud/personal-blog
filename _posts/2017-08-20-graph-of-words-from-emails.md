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

But it is not enough to make an nice insightful graph to visualize. If you are someone experienced with data analysis you may think that this seems too good to be true. You are right. There are too many edges in the graph and it requires a bit more processing. So some  (in fact, many) edges have been removed based on their weight and the degree of the nodes they connect. We remove the weakest links but weakness is relative to the node degree: We require that more links are removed if they connect node with numerous connections.


## The Visualization and analysis

This is an example of a visualization of a network of words. A look at the graph provides interesting insights into this dataset of emails. Topics discussed in the emails can be guessed from the figure and the labels.

In order to separate nodes into groups or clusters, a community detection algorithm has been applied to the graph. On the figure, each color represents a different community.
In addition, the radius of each node is related it the number of occurences of the word in the texts. The layout of the graph is a force directed layout, meaning that nodes repulse each others and links are some elastic binding between them.

The interactive graph can be found by clicking on the following link:
[Interactive visualization](https://bricaud.github.io/HCmails/).

Since it is hard to explain on an interactive visualization, I have made some screen shots of the graph. The following figure show a global view of the graph.
![Graph of words]({{ site.baseurl }}/images/HCmails/HCmails1.png "Graph of words appearing in Clinton's emails")

On the borders of the graph we can see several groups of nodes, many of them having a large radius (appearing in many emails). Notice that the visual clusters are not the same as the clusters found by the community detection. This is not a problem and both clustering can bring slightly different information.


A closer look at the node colors shows that the communities are related to particular topics:

* Foreign affairs (light orange)
* US politics (Green)
* United Kingdom (dark blue)
* Clinton's collaborators (light blue)
* Travel/meetings (dark orange)

As expected, H. Clinton messages are mainly focused on US politics and foreign affairs. First interesting fact is that she seems to treat UK politics differently from the rest of the world. 
![UK cluster]({{ site.baseurl }}/images/HCmails/HCmailzoomGB.png "Zoom on the UK cluster")

Staying in the foreign affairs, a large cluster can be seen containing word related to Middle East, Israel and Palestine; A major concern of US politics for years.

![Middle East cluster]({{ site.baseurl }}/images/HCmails/HCmailszoommiddleeast.png "Zoom on the Middle East cluster")

You can also spot in the middle of the graph a group of nodes related to Talibans.
![Taliban cluster]({{ site.baseurl }}/images/HCmails/HCmailszoomAfghan.png "Zoom on the Taliban cluster")

At that time, there was a major concern about Middle East and muslim countries. This is still true today!

Concerning the US politics, there is a large cluster with various keywords at the bottom of the graph. The precise topic is unsure and could involve several themes. Further analysis of the mails should be done to understand how they relate.

![US politics cluster]({{ site.baseurl }}/images/HCmails/HCmailszoomUS.png "Zoom on the US politics cluster")

During her mandate, it seems that one of the US elections has raised H. C. interest in particular. 
![Election cluster]({{ site.baseurl }}/images/HCmails/HCmailszoomelections.png "Zoom on the Election cluster")


Eventually, it is interesting to note that H. C. has close collaborators to whom she exchange a lot. They are visible as light blue nodes

![Assistants cluster]({{ site.baseurl }}/images/HCmails/HCmailszoomassit.png "Zoom on the assistants cluster")


Some themes are clear and well-separated, some other not. For example if you drag the node "Japan" (in light orange), it has been classified in the same cluster as south american countries and has a link with Brazil (that would be interesting to know why!). Notice that it is the case for Korea as well. It is also connected to the word "Asia" in the dark green cluster, and also to China in the light green cluster. These connections are of course not surprising and shows a glimpse of the complexity of international relationships. 
![Graph of words Middle East]({{ site.baseurl }}/images/zoommiddleeast.png "Zoom on the Middle East")

Concerning the relevance of the data displayed, we can point out a few uninformative nodes. The visualization could be improved further with an additional cleaning of the dataset. For example, "Clinton" should be removed (unless it refers to Bill) as everything is obviously related to Hillary. This node is a hub, with many connections, that add some confusion to the graph and the connected topics. Word containing numbers or nodes with month names should also be removed.

Further development could be done by taking the time into account. Clearly, some of the topics have a limited time span and the keywords they contain could be 