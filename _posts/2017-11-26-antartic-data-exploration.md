---
layout: post
comments: true
title: Exploring the oceans, around Antartica
categories: [signal processing, python, sonar, biology]
thumbnail: /images/ACE/145px-Antarctica.svg.png
published: false
---


The South Pole is

# Context

Recently, I have participated to my first "data jam days". It took place at EPFL in Lausanne, Switzerland. The concept is to invite people of various backgrounds in one place during two days, and let them work together on some nice datasets. It was a cool experience and I look forward to the next one.

For the two days, I teamed up with [Camille Le Guen](http://camleguen.wixsite.com/monsite), the ACE expert who presented us the dataset. She is doing her PhD and has spent several weeks on the ship around Antartica. She has many fun stories about her experience on the ship, camping on some of the Islands, catching pingouins, puting GPS trackers to find out were they find their food.


![Antartica]({{ site.baseurl }}/images/ACE/481px-Antarctica.svg.png "Antartica")


# The dataset 
Several datasets were presented and I chose to work on the one from the [Antartic Circumnavigation Expedition](http://spi-ace-expedition.ch/). This is one of the 22 scientific experiments done during the expedition. Sonar signals were collected continuously while the ship was travelling around Antartica. The purpose is to extract echos in the data that are due to krill swarms (mainly but can also be due to other living organisms). The swarms reflect the acoustic waves and this is detected by the sonar. Unfortunately, these reflections are weak and it is difficult to distinguish them from the noise and the other sonar artefacts. 

Let us look at a plot of the sonar data. On the image below, we can see the ocean depth on the y axis. The top is just below the surface, were the sonar emitter is located. The bottom is here at around 30m below the suface. We decided to focus on this region as it is were the krill swarms are supposed to be, and were the signal is of better quality. The x axis represents the time. The sonar emits a ping every 8 seconds, so each value is a 8 second time frame. As the ship is moving you can also see the x axis as a distance.

![Example of sonogram data]({{ site.baseurl }}/images/ACE/sonogramexample.png "Sonogram data")

Each ping of the sonar is tagged with it GPS coordinates and the time. Hence we can find the precise location of the krill swarm when we have detected one.


# Our goal

Two days is very short and it is difficult to ask for ambitious results. After a few minutes of brainstorming and a coffee, we settled a roadmap. We wanted to have at the end an image of the earth and some ticks where krill swarms were detected. If we had time we could add information on each swarm, like its width and depth and play with color and size of the ticks. 

We fixed several milestones: 

* Detect the presence of krill inside the data
* Extract its information
* Create a map to visualize the results



# Signal processing and denoising

The main task for us was to extract the krill swarm signature among the noise. And indeed, the data is terribly noisy. Usually in data science, you have cleaner datasets. It was a great challenge but I was confident. I have spent several years in the past, working on acoustic and radar signals as well as denoising techniques.

Fortunately for us, there is a great Python module called [scikit-image](http://scikit-image.org/) or skimage that helped us to quickly apply different denoising techniques as see what gave the best outcome.


## Removing salt-and-pepper noise with a median filter



## Smoothing the signal with a Gaussian filter
