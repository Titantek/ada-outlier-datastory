---
layout: page
title: Back to the Future 
subtitle: Back to the Future - Time-Traveling through Wikispeedia
---
# Introduction (mainly datastory)

M: Hey Doc what's up? You know, this web game, Wikispeedia? I've been playin' it a few times and it's way harder that I thought it would.
D: What are you talking about Marty? You know your old Doc, I am not quite into online games or whatsoever.
M: Ok, ok, hear me out:
~ small presentation about Wikispeedia principle and how to play the game ~
D: Fascinating! Marty, you are saying it is harder that you thought, how so?
M: Yeah Doc, I dunno, sometimes i don't even now the target so I try but I just fail… Or sometimes I just keep going back and forth between articles because they just don't look like how I expected them to be, like at all! It seems like they are not nowadays Wikipedia articles and do not contain the next link I was looking for, things like that… Is this game too old for me?Am I just bad at this game?
D: Hmm I see… Well Marty, you just gave me a BRILLIANT idea! Let us inspect this game and see how people performed on it since it's been created!
M: Sure Doc, take the lead!
~ Presentation of the dataset by Doc ~

# Part 1: How are people performing in Wikispeedia?


D: The articles present in the Wikispeedia dataset have categories. Do these categories influence your success, Marty? Let's explore that together!


First, how do the categories look like? For most of them one main category is followed by more precise subcategories. For example, the mixed-breed dog article has for main category "Science", first subcategory "Biology" and second subcategory "Mammals". For simplicity, we will keep only the first category. You can observe here the distribution of those main categories. HERE::\<insert image of Einstein the dog\>
<iframe src="/ada-outlier-datastory/assets/img/pie_cat.html" width="800px" height="400px" alt='Pie chart of the categories'></iframe>
Back in 2007, science articles represented almost 1/4 of the encyclopedia where as art articles consisted of less than 1% of it! HERE:: more analysis?


Second, we notice that among the 4598 articles, some have more than 1 main category: we count 590 articles with 2 main categories and 8 articles with 3. It complicates our analysis. To keep things simple, we will impose rules on which main category we think is the most important for the article considered. For this, we have created a partial ordering in the categories that is completely arbitrary.

Ok, now we are ready for some data analysis. Let's first look at the links between the articles: from which to which categories go the links? Do they lead to an article from the same category or to another? Is it easy to navigate to another category?
<div class="flourish-embed flourish-sankey" data-src="visualisation/20671114"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/20671114/thumbnail" width="100%" alt="sankey visualization" /></noscript></div>
It seems really to reach articles about countries: there are more than twice of links pointing to countries than links going out from countries. It seems logical as for many concepts, the place of invention discovery or birth is mentioned, including the country. Science articles are the ones linking out the less to other categories, with only 41% of links going elsewhere than in science articles. With these data in mind, are there categories of articles harder to guess for players?


To answer this question, we can investigate the categories of starting articles and target articles of the players.
<div class="flourish-embed flourish-sankey" data-src="visualisation/20646616"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/20646616/thumbnail" width="100%" alt="sankey visualization" /></noscript></div>

For unfinished paths, the middle step corresponds to the category of the last article visited. In 28% of the cases, the player is already in the good category when he/she stops playing. But do we need to be in the same category as the target article to find it? It depends! For "People" category for example, less than 10% of players who succeeded were on an article in this category right before winning! However, in the case of "Science", the rate goes up to 81%!
<div class="flourish-embed flourish-sankey" data-src="visualisation/20673754"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/20673754/thumbnail" width="100%" alt="sankey visualization" /></noscript></div>

Let's do some tests to compare the distribution of categories between finished and unfinished paths.  

Analysis of other datas for finished/unfinished:

![distrib_shortest_paths_duration](/ada-outlier-datastory/assets/img/distrib_shortest_path_duration.png)

Further comparison between finished/unfinished:

![distrib_paths_per_game](/ada-outlier-datastory/assets/img/distrib_paths_per_game.png)

Are there other factors that influence the success rate? Let's investigate that.

HERE:: definition of the shortest path = number of links clicked + 1. Shortest path of 2 = the link of the target is already on the start article.
explain that we kicked out games where player didn't click at all. so shortest length possible of a game = 2

One can assume that the shorter the shortest path, the more likely it is to find a path, because both articles are closely connected by links. This is well illustrated in the following plot. The longer the shortest path is, the fewer finished paths there are! The biggest shortest path for which we have finished paths is 7. Only 17.37% of the game collected are victories. We also notice that two-thirds of the players did not go far enough anyway to reach the target, as they stopped before even reaching the shortest path length. As we could expect, the bigger success rate occurs with a shortest path of 3 and decreases monotonically while the shortest path increases. 

<iframe src="/ada-outlier-datastory/assets/img/distrib_path_lengths_wrt_shortest_path.html" width="900px" height="600px" alt='distrib_path_lengths_wrt_shortest_path'></iframe>

Another parameter might be the number of links leading to the target: intuitively, the more there are the easier it is to reach the article. Let's work on this hypothesis. The following plot shows the distribution of the links to the target number depending on whether the player found the target. The distributions look different! Let's try a t-test of independence to confirm our intuition. Our null hypothesis is that the two distributions are identical. Using the `ttest_ind_from_stats` function from scipy, we obtain a p-value of 0 and a test statistic of 45.50. We can thus safely reject our null hypothesis and conclude that the two distributions are indeed different! Both distribution shapes are similar, but the one from unfinished paths is shifted to the left and there is a peak at 1.

<iframe src="/ada-outlier-datastory/assets/img/distrib_links_to_target" width="900px" height="600px" alt='distrib_links_to_target'></iframe>

How can we now use these observations to predict player success? Let's do a logistic regression! We want to predict if a player will succeed for a given start and end article, depending on the shortest path and the number of links pointing to the target.

Log. regression:
table with results?








# Part 2: How did Wikipedia's structure evolve since 2007?

M: Okay I know more about Wikispeedia and the general performance of players on the game. I wonder 

Wikipedia structure comparison : 
overall differences 

![distrib_links_per_article](/ada-outlier-datastory/assets/img/distrib_links_per_article.png)


![diff_links_per_article](/ada-outlier-datastory/assets/img/diff_links_per_article.png)

network differences 

![heatmap_diff](/ada-outlier-datastory/assets/img/heatmap_diff.png)


![reachable_nodes](/ada-outlier-datastory/assets/img/reachable_nodes.png)


# Part 3

# Part 4

# References

[1] Robert West and Jure Leskovec:
     Human Wayfinding in Information Networks.
     21st International World Wide Web Conference (WWW), 2012.
     
[2] Robert West, Joelle Pineau, and Doina Precup:
     Wikispeedia: An Online Game for Inferring Semantic Distances between Concepts.
     21st International Joint Conference on Artificial Intelligence (IJCAI), 2009.

