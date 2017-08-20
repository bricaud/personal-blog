---
layout: post
title: How graphs can help you find information in data
---


The nice post of my colleague and friend [Volodymir](http://blog.miz.space/) on [Wikipedia graph mining](http://blog.miz.space/research/2017/08/14/wikipedia-collective-memory-dynamic-graph-analysis-graphx-spark-scala-time-series-network/), made me think that I should blog as well on my exploration of graphs and data analysis. Blogging is an interesting way to spread some science to a broader audience and scientists should do that more often. Also, this could help me show the fascinating world of graphs and networks and how it can be useful in practice. So here is my first contribution. 

The goal of this post is to demonstrate with an example that one can build a graph from some unstructured dataset to extract valuable information. Our dataset will be a set of texts. I have chosen Hillary Clinton's emails as they are publicly available and it talks to people. We can find the dataset on Kaggle [here](https://www.kaggle.com/kaggle/hillary-clinton-emails). This dataset contains the emails sent and received by Hillary Clinton between 2009 and 2011 in her private mailbox. As I said, the data has been made freely accessible to the public, except for some parts which have been censored. There are XX emails.