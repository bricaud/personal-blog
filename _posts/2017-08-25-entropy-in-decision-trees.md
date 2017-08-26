---
layout: post
comments: true
title: A simple explanation of entropy in decision trees
categories: [tutorial, machine learning]
thumbnail: /images/Entropy.entropythumb.png
published: false
---

When browsing Machine Learning tutorials, every now and then I find the explanations obscure and not adapted to beginners. Sometimes, there is a strong focus on maths, which is not bad in itself, but makes it difficult to grasp for people not used to high level maths (and there are many of them who wish to learn the basic concepts of Machine Learning!). Sometimes, the emphasis is on the main part of the algorithm and some details are left missing. Then you tend to think it is missing because it is so simple that everyone will understand. But in many case it is not! And it leaves the false impression that because you are not able to understand this step, you are not clever enough to grasp the concept. Pretty discouraging!

In this series of blog posts, I want to clarify or at least provide a different explanation of some of the concepts in machine learning, in the hope of helping people increasing their understanding about these methods.
Since I have spent quite some time studying the concept of entropy in academia, I will start my Machine Learning tutorial with it.
It is a key step in decision trees, however, entropy is often overlooked (as well as the Gini coefficient). This is really an important concept to get, in order to fully understand decision trees.

## Metaphoric definition of entropy

Plato, with his cave, knew that metaphors are good for explaning deep ideas. Let try to get some inspiration from him. I like the definition of entropy given sometimes by physicists:

> Entropy is a measure of disorder

So let us take this point of view and compare our dataset as a messy room:

> Entropy is an indicator on how messy your data is.

![Messy room](/images/entropy/messy_room.jpg "My Kids messy room")

Imagine you are about to tidy your room. Usually you use a subjective measure to estimate how messy is it. (Probably your mother has a different one or a more sensitive one :), but this is not the topic here). You know that objects must be on the shelves and probably grouped together, by type: books with books, toys with other toys ...
Fortunately, the visual inspection can be replaced by a more mathematical approach for the data. A mathematical function exists for estimating the mess among mathematical objects and we can apply it to our data.
The requirement of this function is that it provides a minimal value if there is the same kind of objects in the set and a maximal value if there is a uniform mixing of objects with different labels (or categories) in the set. This is a mathematical sense of being ordered!


## Why entropy and decision trees?

In decision trees, the goal is to tidy the data. You try to separate your data and group the samples together in the classes they belong to. You know their label since you construct the trees from the training set. You maximize the purity of the groups as much as possible each time you create a new node of the tree (meaning you cut your set in two). Of course at the end of the tree you want to have a clear answer. To which group does this sample belongs to? Based on this arrangment of features, without doubt it belongs to Group 1! So decision trees are here to tidy the dataset by looking at the values of the feature vector associated to each data point. Based on the values of each feature, decisions are made that eventually leads to a leaf and an answer. 

At each step, each branching, you want to decrease the entropy, so this quantity is computed before the cut and after the cut. If it decreases, we can proceed to the next step otherwise we must stop. 

What is nice with the entropy is that you can sum it up if you probe 2 (or more) different sets which are independent.

## Mathematical definition of entropy

The entropy is given by the following equation:

![Entropy formula](http://chart.apis.google.com/chart?cht=tx&chl=%24E%20%3D%20-%5Csum_i%20p_i%20%5Cln%20p_i%24%0A)

![Entropy function](/images/entropy/entropyfunction.png "Entropy function")

## Limits of decision trees

Since it goes step by step, decision trees may not provide the optimal classification. It minimizes the entropy at each step but has no global view on the optimization process. Let me explain this differently. Sometimes it may be more efficient to start to put in order the bigger items event if there are not many of them and the the impression of tidiness does not increase much after. You could then put the smaller items on top of the bigger to get a nicer view of your room. Starting the other way round might not lead to a room as neat.