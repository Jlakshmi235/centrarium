---
title: The Language of Kickstarter
date: 2017-09-01 09:49:45 Z
categories:
- project
layout: post
---

<style type="text/css">
	ul.a {
    list-style-type: circle;
}
</style>
<img src="/images/kickstarter.jpg" width="100%" height="100%" align="middle">                                                                                                                  
## Introduction:
I wrapped up my fourth project at Metis last week. We were given the task of using Natural Language Processing (NLP) and Unsupervised Learning on any dataset of our chosing and analyse the text data.

I decided to use Kickstarter project descriptions for my analysis. For those of you who aren't too familiar with [Kickstarter](http://www.kickstarter.com/), it is a global crowd funding platform where independent creators and passionate backers come together to bring new ideas to life. Creators can write in length about their projects, discussing their motivation, production plans, risks involved, etc. Backers impressed by the projects can pledge as low as $1 to mark their support for the project. Creators who are able to surpass their funding goal within the duration of their campaign can put their dreams to production with the support of the backers.

## Data Resources:

The project descriptions were scraped from [Kickstarter](http://www.kickstarter.com/) using Selenium. Although there are many online Kickstarer datasets available, none of them had the text of the projects descriptions. Checkout my blog post on how to scrape kickstarter data.


## Objective:

Kickstarter has defined 15 categories of projects i.e. Art, Comics, Crafts, Dance, Design, Fashion, Film & Video, Food, Games, Journalism, Music, Photography, Publishing, Technology, and Theater.  

<img src="/images/project_dist.png" style="width:60%;height:60%;" align="middle">

The objective of my project is to using Natural Language Processing and Unsupervised Learning to analyze the the Kickstarter descriptions and to see whether  

1.some new categories would emerge based on the language used  
2.some categories would roll-up into some others 



## Project Overview:
<img src="/images/kickstarter_flow.png" align="middle">



#### Step 1  
Since Kickstarter is a global platform, there are project pitches written in many languages. So, the first step would be to remove all non-English characters and words.

#### Step 2  
Remove stop words, email ids, hyperlinks, hashtags, numbers etc. from the text. They don't add any value to our analysis.

#### Step 3  
Next step would be to lemmatize the words i.e. reduce different forms of a word to the root form. For example, words like dance, dances, dancing, danced would be reduced to dance.

#### Step 4  
Now we need to convert these sentences to numbers so it can be fed to some machine learning algorithms. I have used the Count Vectorizer for this. The resultant matrix gives the count of the different words present in each project description. 49,740 unique words were found throughout all projects.

#### Step 5
On analysisng the Count Vectorizer matrix, I noticed there were nearly 15,000 words that occured just once or twice throughout all project descriptions. So, I then reduced my dimension space to 5000 words by removing the rare words i.e. they have occured less than 25 times throughout all project descriptions.

#### Step 6
Futher dimensionality reduction was done by applying Latent Dirichlet Allocation. 6 abstract "topics" were modelled from the text. (A topic is a collection of words with an underlying theme.)



#### Step 7
Lastly, the kMeans clustering was applied on the reduced data of 6 topics. The data was grouped into 6 clusters. From the graphs given below, we can see that for k = 6, we get a desirable inertia and silhouette score (both are measures to quatify how good the clusters are)   
<br>   
  <img src="/images/6means.png" style="width:65%;height:65%;" align="middle">

#### Step 8
Given below are the word clouds for the different clusters:


| <img src="/images/books.png" align="middle"> | This cluster has many words regarding books and publishing. Projects in this cluster primarily belong to the Publishing, Comics, Journalism and Art categories. So we can say that language used projects in these 4 categories are very similar.  |
| The main words that stand out in this cluster are "Flim" and "Music". Projects belonging to this cluster are mainly from the Film & Video and Music categories.  | <img src="/images/film_video.png" align="middle" >|
| <img src="/images/food.png" align="middle"> | The projects in this cluster are related to Food. |
| This cluster was a little hard to interpret. After inspecting a few projects from this cluster, I decided to name it "Luxury Items". The text mainly goes like "a lot of craftsmanship is required to create these products", "a lot of time is spent in design and production", "quality isn't comprimised" etc. These projects were from the Design, Fashion, Arts, Crafts categories.  | <img src="/images/luxury.png" align="middle" >|
| <img src="/images/games.png" align="middle"> | The projects in this cluster are related to Games and Technology.|
| This cluster is the most interesting one of all. It had projects across all categories. The main theme in this cluster is "Community", "Awareness" and "Betterment of the Society".| <img src="/images/community.png" align="middle" >|



## "Community / Awareness" Cluster

I further investigated the projects in the cluster because this is not one of the 15 original categories that Kickstarter had defined. 

Given below are 3 projects from this cluster:

| <img src="/images/houston.png" align="middle"> | This project is about restoring the Apollo Mission Control Center (MCC) in Houston. The creators of this project are trying to raise money in order to restore the site and create a world-class visitor experience that will inspire future generations through this amazing story of technological and human achievement. There is an underlying theme of "community" and "for the future generations to see".|
| The goal of this project is to raise awarness about social issues by depicting them through murals across America. This project is put under the Art category but also has an underlying theme of "building awareness" and "making the world a better place"| <img src="/images/flint_murals.png" align="middle" >|
| <img src="/images/flint.png" align="middle"> | This project aims to build awareness amongst the residents of Detroit about the water crisis in Flint through artwork. Although an Art project, there is a latent theme of "community" and "awareness".

There are many more examples of such projects which have a hidden theme of "community" and "awareness" but belong to categories such as "Art", "Design", "Technology" etc.

I strongly feel that Kickstarter should create a new category "Community / Awareness" for such projects. By doing so, philanthropist and benevolent people can easily find all these projects in one place rather than searching for them under Art, Design, Technology etc.


## Conclusion:
By using NLP and Unsupervised learning methods, I was able to get 6 clusters of projects. Most of these cluster were a combination of 2 or more Kickstarter project categories. Language used by projects in Publishing, Comics, Journalism and Art categories were very similar. So was the language used in Design, Fashion, Arts, Crafts projects. A new category of "Community / Awareness" emerged as a result of the NLP and clustering.

