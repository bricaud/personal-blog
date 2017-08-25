---
layout: post
comments: true
title: A simple explanation of entropy in decision trees
categories: [tutorial, machine learning]
thumbnail: /images/Entropy.entropythumb.png
published: false
---

When browsing Machine Learning tutorials, every now and then I find the explanations obscure and not adapated to beginners. Sometimes, there is a strong focus on maths, which is not bad in itself, but makes it difficult to grasp for people not used to high level maths (and there are many of them who wish to learn the basic concepts of Machine Learning!). Sometimes, the emphasis is on the main part of the algorithm and some details are left missing. Then you tend to think it is missing because it is so simple that everyone will understand. But in many case it is not! And it leaves the false impression that because you are not able to understand this step, you are not clever enough to grasp the concept. Pretty discouraging!

I want to clarify or at least provide a different explanation of some of the concepts in machine learning, in the hope of helping people increasing their understanding about these methods.
Since I have spent quite some time studying the concept of entropy in academia, I will start my Machine Learning tutorial with it.
It is a key step in decision trees, however, entropy is often overlooked (as well as the Gini coefficient). This is really an important concept to get in order to fully understand decision trees.

## Metaphoric definition of entropy

Plato, with his cave, knew that metaphor are good for explaning deep ideas. Let try to get some inspiration from him. I like the definition given sometimes by physicists:

> Entropy is a measure of disorder

![Messy room](/images/entropy/messy_room.jpg "My Kids messy room")

To get an idea of entropy in our case, let us start with this sentence:

> Entropy is an indicator on how messy your data is.

Imagine you are about to tidy your room. Usually you use a subjective measure to estimate how messy is it. And probably your mother has a different one or a more sensitive one :), but this is not the topic here. You know that objects must be on the shelves and probably grouped together, by type: books with books, toys with other toys ...
Fortunately, the visual inspection can be replaced by a more mathematical approach for the data. A mathematical function exists for estimating the mess among mathematical objects and we can apply it to our data.
The requirement of this function is that it provides a minimal value if there is the same kind of objects in the set and a maximal value if there is a uniform mixing of objects with different labels (or categories) in the set. We want it to be ordered!


## Why entropy and decision trees?

In decision trees, the goal is to tidy the data. You try to separate your data and group the samples together in the classes they belong to. You know their label since you construct the trees from the training set. You maximize the purity of the groups as much as possible each time you create a new node of the tree (meaning you cut your dataset in too). Of course at the end of the tree you want to have a clear answer. To which group does this sample belongs to? So decision trees are here to tidy the dataset by looking at the values of the feature vector associated to the data. Based on the values of each feature, they are making decision on where to put them at each step. 



## Mathematical definition of entropy

$$
E = \sum_p p \ln(p)
$$

![Entropy function](/images/entropy/entropyfunction.png "Entropy function")

## Limits of decision trees

Since it goes step by step, decision trees may not provide the optimal classification. It minimizes the entropy at each step but has no global view on the optimization process. Let me explain this differently. Sometimes it may be more efficient to start to put in order the bigger items event if there are not many of them and the the impression of tidiness does not increase much after. You could then put the smaller items on top of the bigger to get a nicer view of your room. Starting the other way round might not lead to a room as neat.