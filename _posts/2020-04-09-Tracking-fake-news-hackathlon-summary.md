---
layout: post
comments: true
title: Hackathlon project: Tracking fake news in social networks
categories: [Scientific projects]
tags: [Social networks, fake news, data science, graphs, hackathlon]
thumbnail: /images/hackathlon/redditgraphwithclusters3.png
published: true
---

On April 3-6 2020 I took part in the Hackathlon [LauzHack against COVID-19](https://covid19.lauzhack.com/) organized by the LauzHack association and EPFL. It was an interesting experience and I would like to summarize here the results we got during the 3 days on our project, as well as my impressions.

I jumped on the occasion to propose a project related to the analysis of fake news and controversial topics about the COVID-19. The project was named 'Tracking fake news in social networks'. Indeed, I have been working on a research project since September 2019 whose goal is to detect and analyse controversies in social networks. I thought it would be a good opportunity to share our knowledge about this phenomenon and make some concrete contributions to fight misinformation.

I was glad to see people supporting this project and commenting on it. About 25 participants joined the project to see what it was about. Some of them were really concerned by the topic and willing to help, worried about the enormous amount of fake news and misinformation on the coronavirus. Some others were just curious about how we would process and tackle this challenge. Overall, 6-7 participants were really active, coding small scripts or analysing the data. Due to the circomstances, the hackathlon was a purely online one, without physical interactions. Everyone was working from home. Interestingly, all the interactions took place using Slack and video-conference and it worked! However, I had the feeling that it was less efficient than an onsite Hackthlon. From time to time, participants (including myself) would disappear for short or long period of time. Of course, distractions are numerous at home and no-one is watching you :).

# Goal

The goal of the project was to collect posts on different social networks and detect the fake news or controversies about the COVID-19. Several objectives were defined with different level of difficulties. The most ambitious goal was to detect automatically fake news. Intermediate objectives were to collect the fake news of the past weeks and make a historical timeline about them, or at least to produce some statistics about controversies and news concerning the COVID-19 in social networks.

# Method

The difficult task was to detect fake news (or controversies) among the huge amount of data. At first sight, this looks challenging as human themselves are often unable to spot them. However, firstly several [scientific studies](https://science.sciencemag.org/content/359/6380/1146) have shown that fake news are usually more popular, spread more and faster in the social network. This indirect sign suggests to focus on the spreading behavior rather than on the content of a post or tweet. Secondly, particular users, such as conspirationist, extremists, have a tendency to heavily share fake news. Identifying and selecting such accounts or channels should help in our task.

After a short discussion, and based on participants preferences, Reddit and Twitter were selected as the social networks we would investigate.
We organized the project into 2 focused subteams and an additional group of participants, making the connection between the teams and suggesting ideas in an 'agile' way. The first subteam was dedicated to the collection of Reddit data related to the COVID and the other to the collection of Tweets. For that, they used Python and the APIs provided by the 2 platforms.

Experience from the research project was very helpful to get a quick start. We had already analyzed several subreddits for the project and the choice was easy. We quickly oriented our focus on subreddits related to the coronavirus. We knew 3 interesting ones, each of them with a different 'editorial line'. The subreddit r/China_flu has more posts sharing controversial topics, r/Coronavirus is rather neutral and r/COVID-19 contains more scientific discussions. In order to design an automatic detection, tagging posts and topics as controversial, neutral or scientific would be indeed valuable. We collected posts and links inside posts, especially links refering to tweets. The list of tweets in each subreddit was then given to the second team, dedicated to the extraction of tweets. During the 3 days, tweets were extracted to be analyzed using different methods.


# The results

The analysis of Tweeter and Reddit data gave us several insights about the Coronavirus and the sharing of information in these social networks.

## Graph of users

The first results came directly from the tools developed for the research project. Starting from a list of users, a graph was created where nodes are users and connections are mention or retweets between users. This graph shows the organization of the social network around users sharing news about the COVID19. 
![Graph of tweeter users]({{site.baseurl}}/images/hackathlon/redditgraphwithclusters3.png)

We started from the tweets referenced in the 3 subreddits, identified the users and made the graph from their interactions. The first fact to notice is that this network is not made of isolated clusters. There are different communities (at least the 3: diffusing controversies, neutral and scientific info) but there is no clear separation between them. A closer look reveals regions in the network that have different topics. These regions are indicated on the figure by large colored circles. Some parts focus on the events of specific country or areas (UK on top, India on the right and New York at the bottom). Other regions are distinguished by their orientation. In the large green circle, in the middle, are located established institution such as WHO and CDC and regular news NY times, Washigton post or CNN. These users mainly share fact-checked information and scientific news. A region in light red concentrates several accounts sharing controversies. We can see the account of the US president as they often refer to him.

## Word cloud

Second result was obtained by extracting the text of all the tweets extracted from Reddit. Selecting the most popular keywords (made of one, two or three words) apearing we got this interesting word cloud having the shape of a virus.

![Word cloud from tweets about COVID-19]({{site.baseurl}}/images/hackathlon/23312.tif_wc.png)

## Evolution of the number of tweets

We also ploted the number tweets per day appearing in the subreddits, talking about a particular country.
![Evolution of the number of tweets]({{site.baseurl}}/images/hackathlon/evolutionoftweets.png)
![Evolution of the number of tweets not including China and the US]({{site.baseurl}}/images/hackathlon/evolutionoftweets_woUS.png)

Peaks can be seen when particular events or news about the virus appears. For example Italy red curve peaks on the 25/02 when north of Italy was confined and on the 10/03 when all of Italy was confined.

The number of posts in the controversial subreddit r/china_flu shows several peaks between January and April.
![evolutionofredditposts]({{site.baseurl}}/images/hackathlon/reddit_china_flu_submissions_activity.png)

## Difference between countries

The top countries discussed in the subreddits are shown on the following figure. It reflects exactly the countries where the epidemy spreading is the highest. Of course, the USA is the most discussed country as the language of the subreddits is english and most of the users are from the USA.
![Reddit posts per country]({{site.baseurl}}/images/hackathlon/postspercountries.png)


## Conclusion

The page of our project can be found [here](https://devpost.com/software/tracking-fake-news-in-social-networks-proposal). The information is minimal as we were short in time to prepare a good communication about our results. We must also say that designing an automatic detection of fake news was highly ambitious for a 3 days Hackathlon. What we obtained is more modest but it gave interesting information on social networks, in particular Reddit and Twitter, and how the information about COVID-19 is treated and shared. This will be useful in the future for our research project.