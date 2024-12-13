---
layout: page
title: Back to the Future 
subtitle: Back to the Future - Time-Traveling through Wikispeedia
---
# Introduction (mainly datastory)

<div class="chat">
  <div class="message Marty">
     <img src="../img/Marty_and_Doc/marty1.png" alt="Marty" class="profile-pic">Hey Doc what's up? You know, this web game, Wikispeedia? I've been playin' it a few times and it's way harder that I thought it would be.</div>
  <div class="message Doc">
     <img src="../img/Marty_and_Doc/doc1.png" alt="Doc" class="profile-pic"> What are you talking about Marty? You know your old Doc, I am not quite into online games or whatsoever.</div>
  <div class="message Marty">
     <img src="../img/Marty_and_Doc/marty1.png" alt="Marty" class="profile-pic">Ok, ok, hear me out:</div>
</div>

M: Hey Doc what's up? You know, this web game, Wikispeedia? I've been playin' it a few times and it's way harder that I thought it would.
D: What are you talking about Marty? You know your old Doc, I am not quite into online games or whatsoever.
M: Ok, ok, hear me out:
~ small presentation about Wikispeedia principle and how to play the game ~

<div class="chat">
  <div class="message Doc">Fascinating! Marty, you are saying it is harder that you thought, how so?</div>
  <div class="message Marty">Yeah Doc, I dunno, sometimes I don't even now the target so I try but I just fail… Or sometimes I just keep going back and forth between articles because they just don't look like how I expected them to be, like at all! It seems like they are not nowadays Wikipedia articles and do not contain the next link I was looking for, things like that… Is this game too old for me? Am I just bad at this game?</div>
  <div class="message Doc">Hmm I see… Well Marty, you just gave me a BRILLIANT idea! Let us inspect this game and see how people performed on it since it's been created!</div>
  <div class="message Marty">Sure Doc, take the lead!</div>
</div>

D: Fascinating! Marty, you are saying it is harder that you thought, how so?
M: Yeah Doc, I dunno, sometimes i don't even now the target so I try but I just fail… Or sometimes I just keep going back and forth between articles because they just don't look like how I expected them to be, like at all! It seems like they are not nowadays Wikipedia articles and do not contain the next link I was looking for, things like that… Is this game too old for me?Am I just bad at this game?
D: Hmm I see… Well Marty, you just gave me a BRILLIANT idea! Let us inspect this game and see how people performed on it since it's been created!
M: Sure Doc, take the lead!
~ Presentation of the dataset by Doc ~

# Part 1: How are people performing in Wikispeedia?


D: The articles present in the Wikispeedia dataset have categories. Do these categories influence your success, Marty? Let's explore that together!


First, how do the categories look like? For most of them, one main category is followed by more precise subcategories. For example, the mixed-breed dog article has for main category "Science", first subcategory "Biology" and second subcategory "Mammals". For simplicity, we will keep only the first category. You can take a look at the distribution of those main categories here. HERE::\<insert image of Einstein the dog\>

HERE:: more analysis?


Second, we notice that among the 4598 articles, some have more than 1 main category: we count 590 articles with 2 main categories and 8 articles with 3. It complicates our analysis. To keep things simple, we will impose rules on which main category we think is the most important for the article considered. For this, we have created a partial ordering in the categories that is completely arbitrary.

<iframe src="/ada-outlier-datastory/assets/img/pie_cat.html" width="800px" height="400px" alt='Pie chart of the categories'></iframe>
Back in 2007, science articles represented almost 1/4 of the encyclopedia, whereas art articles comprised less than 1% of it. 



Ok, now we are ready for some data analysis. Let's first look at the links between the articles: from which to which categories go the links? Do they lead to an article from the same category or to another? Is it easy to navigate to another category?

<!---
<div class="flourish-embed flourish-sankey" data-src="visualisation/20671114"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/20671114/thumbnail" width="100%" alt="sankey visualization" /></noscript></div>
--->

<iframe src="/ada-outlier-datastory/assets/img/links_categories.html" width="900px" height="600px" alt='links_categories'></iframe>

Wow, lots of information on this plot! First, the diagonal, i.e. links staying in the same category has bigger values compared to the lines or columns in general. Then, we can observe that the brighter columns are the ones from science, geography and countries. For science and geography, it makes sense as these are the most represented categories as we have seen previously. On the other hand, it seems very easy to reach articles about countries: there are more than twice of links pointing to countries as links going out from countries. It seems logical as for many concepts, the place of invention discovery or birth is mentioned, including the country. Science articles are the ones linking out the least to other categories, with only 41% of links going elsewhere than in science articles. With these data in mind, are there categories of articles harder to guess for players?


To answer this question, we can investigate the categories of starting articles and target articles of the players. 
<!---
<div class="flourish-embed flourish-sankey" data-src="visualisation/20646616"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/20646616/thumbnail" width="100%" alt="sankey visualization" /></noscript></div>
--->

<iframe src="/ada-outlier-datastory/assets/img/categories_finished_paths_start2target.html" width="900px" height="600px" alt='categories_finished_paths_start2target'></iframe>
<iframe src="/ada-outlier-datastory/assets/img/categories_unfinished_paths_start2target.html" width="900px" height="600px" alt='categories_unfinished_paths_start2target'></iframe>

Both heatmaps look similar! But what does the statistics tell us? Let's perform a chi2 contingency test with `scipy.stats.chi2_contingency` function: our null hypothesis is that the distributions are independent. We choose a level of significance of $\alpha=1$%.
What is meant by distribution is a vector of $15\times15$ that contains the count of links from the start category to the end category. It's simply the data from the heatmap, in the form of counts.
We did $\chi^2$-test between the start-to-target article categories distributions of finished paths and unfinished paths and between the start-to-target and start-to-end article categories distributions of unfinished paths. Both give a p-value of 0 and a test statistic of respectively 3018.55 and 8297.54. We can thus safely reject the null hypothesis of independence and declare the categories distributions similar. HERE:: should I add the 2 other stats tests? (same results)
Well, our analysis of the categories does not explain why players lose!


<!---
For unfinished paths, the middle step corresponds to the category of the last article visited. In 28% of the cases, the player is already in the good category when he/she stops playing. But do we need to be in the same category as the target article to find it? It depends! For "People" category for example, less than 10% of players who succeeded were on an article in this category right before winning! However, in the case of "Science", the rate goes up to 81%!
--->

<!---
<div class="flourish-embed flourish-sankey" data-src="visualisation/20673754"><script src="https://public.flourish.studio/resources/embed.js"></script><noscript><img src="https://public.flourish.studio/visualisation/20673754/thumbnail" width="100%" alt="sankey visualization" /></noscript></div>
--->
<!---
<iframe src="/ada-outlier-datastory/assets/img/categories_unfinished_paths_start2target.html" width="900px" height="600px" alt='categories_unfinished_paths_start2target'></iframe>

<iframe src="/ada-outlier-datastory/assets/img/categories_unfinished_paths_start2end.html" width="900px" height="600px" alt='categories_unfinished_paths_start2end'></iframe>


<iframe src="/ada-outlier-datastory/assets/img/graph_complique.html" width="900px" height="600px" alt='graph_complique'></iframe>
--->

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

Let us now compare the differences between the old wikipedia from 2007 and our current wikipedia from 2024. The first factor that could influence the performances of the players is the number of links per articles. Wikipedia is expanding everyday thanks to its collaborative process and has significantly improved and grown since 2007. Let's see how much that changes compared to now ! 

> 2007 : 119882 links
> 2024 : 225800 links

![distrib_links_per_article](/ada-outlier-datastory/assets/img/distrib_links_per_article.png)

As expected, there is much more links per pages in our 2024 dataset ! The distribution also shows that more pages have a higher number of links. This could probably influence users' performances. 

<div class="chat">
  <div class="message Marty">Cool! So that's why the game is harder in 2007?</div>
  <div class="message Doc">Wait a bit Marty let's look more into the details before driving any conclusions. Let's look at individual articles:</div>
</div>

In this plot, we visualize every articles within our dataset of the 4604 selected articles from the Wikispeedia game on the x axis and compute the differences in the number of links between the two timepoints. Anything above zero, in green, represents more links in 2024 than in 2007, and anything below, orange, corresponds to less links in the page in 2024 than in 2007.

![diff_links_per_article](/ada-outlier-datastory/assets/img/diff_links_per_article.png)



network differences 

![heatmap_diff](/ada-outlier-datastory/assets/img/heatmap_diff.png)


![reachable_nodes](/ada-outlier-datastory/assets/img/reachable_nodes.png)


# Part 3

# Part 4: Are the players(LLMs) stronger in 2024 than in 2007 ?

**Marty** : Doc are the players today stronger than in 2007 ? 
**Doc** : I don't know Marty, we don't have any data about the players in 2024.
**Marty** : But we have the data from 2007, can't we compare the two years ?
**Doc** : We might be able to do that, let me think about it... we can use my favorite tool LLMs <3 to compare the two years.
**Marty** : LLMs ? But, the results will differ from the ones we got from the players's data, right ? Which model should we use ?
**Doc** : Yes, it might be different, but we can test different models and see which is the most similar to the players' data.
**Marty** : That's a great idea Doc, let's do it ! but how would we know if the model is similar to the players' data ?
**Doc** : First, we can verify that the model can finish a game, then we can compare the path length with the players, and eventually, we can measure if the model has chosen the same articles as the players.
**Marty** : Wow, that's a great plan Doc, and once we have the model, we can compare the two years and see if the LLM model is better at the game in 2024 than in 2007.
**Marty** : Heeu Doc, I know you are a genius, but how will you train the models on the 2007 data ?
**Doc** : I will use the games that at least 10 players have played and I will train the models with [Ollama](https://ollama.com/) and based on the path length distribution of the players that we can see below(**INSERT**). I will limit the number of prompts to 50.

![players_path_length](/ada-outlier-datastory/assets/img/players_path_length.svg)

![llms_path_not_found](/ada-outlier-datastory/assets/img/llms_path_not_found.svg)

<iframe src="/ada-outlier-datastory/assets/img/performance_scatter.html" width="100%" alt='models_performance' frameBorder="0"></iframe>

![llm_jacard](/ada-outlier-datastory/assets/img/jacard.svg)

![llama3_2007_2024](/ada-outlier-datastory/assets/img/llama3_2007_2024.svg)





# References

[1] Robert West and Jure Leskovec:
     Human Wayfinding in Information Networks.
     21st International World Wide Web Conference (WWW), 2012.
     
[2] Robert West, Joelle Pineau, and Doina Precup:
     Wikispeedia: An Online Game for Inferring Semantic Distances between Concepts.
     21st International Joint Conference on Artificial Intelligence (IJCAI), 2009.

