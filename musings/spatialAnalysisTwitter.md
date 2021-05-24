---
layout: page
title: Replicability and Reproducibility of A Spatial Analysis Using Twitter
---

Wang et al. used the Twitter search API to first identify tweets with the keywords "fire" and "wildfire", and then further with keyword searches that included locations of wildfires (keywords and tweets associated are summarized in Table 1). Locations were provided in Table 2, which included descriptions of places as well as geographic coordinates (general spatial-temporal data). Spatial analysis was only possible on tweets that had coordinate metadata. They also analyzed retweet networks to identify trend setters.

Three main analytical techniques were used: kernel density estimation, text mining and social network analysis.

Kernel density estimation was used to create a map of wildfire related tweets, taking coordinates of tweets and exporting a raster with a value representing the number of tweets for an area. Further, a dual kernel density estimation was used to visualize tweet locations for two specific fires (randomly chosen from Table 2).

Text mining was used to identify important terms used in wildfire related tweets (using the "tm" package in R 3.12), and social network analysis was used to understand the behavior of users communicating about wildfires through retweets (using a k-mean clustering method through the R package "igraph").  

Results showing the number of tweets over time were displayed using bar charts, while the spatial distribution of wildfire tweets (displayed using different sized centroids), and the kernel density analyses were visualized through maps (displayed using a heat map).

Term frequency was visualized using a bar chart as well. Term cluster results were displayed in a table and the distribution of the retweet network was visualized in line graphs. The retweet network was visualized in a node diagram where nodes were drawn between users when one retweeted another.

This study seems to be both reproducible and replicable. There is much detail on what R packages and spatial analyses were used, which lends itself well to both replicability and reproducibility. However, there is information missing that would make reproducibility slightly difficult, such as the specific parameters used in the kernel density estimate. It was also unclear how much of the total tweets that met their keyword search terms were used, as at the end of the article they mention how they used a 1% sample limitation, but there is no further detail on that in the rest of the paper. Most importantly however, there is detail at the ground level of how they got their data, from where, and with what search, which eliminates a lot of uncertainty and error that could be introduced when reproducing studies without key information on the data used. In a similar way, the study also lends itself to replicability, in which new researchers could look at wildfire tweets in a different part of the world, or look at a different kind of natural disaster in California, or elsewhere.

Sources:
* Wang, Z., X. Ye, and M. H. Tsou. 2016. Spatial, temporal, and content analysis of Twitter for wildfire hazards. Natural Hazards 83 (1):523–540.
