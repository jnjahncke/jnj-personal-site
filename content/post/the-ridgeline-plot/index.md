---
title: 'The Ridgeline Plot: Danceability Across the Ages'
author: ''
date: '2020-04-22'
slug: the-ridgeline-plot
categories: []
tags:
  - data viz
  - how to
subtitle: ''
summary: ''
authors: []
lastmod: '2020-04-22T09:06:16-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: yes
projects: []
---
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />




```
## Warning: package 'ggridges' was built under R version 4.2.3
```

```
## Warning: package 'knitr' was built under R version 4.2.3
```


Ridgeline plots are a variation of density plots in which you aim to compare the distributions of several categorical variables (represented on the y-axis) for a single continuous variable (represented on the x-axis). This is a quick way to compare a large number of groups where doing something like a simple `geom_density()` + `facet_wrap()` would occupy a large amount of space. By making use of transparency the ridges can be places in close proximity to save space.

This ridgeline plot was created using the package `ggridges`, which integrates with ggplot in R. Here I used `geom_density_ridges()`. There are ways to add more features to the ridges (ex. raincloud, rug). See the following resources for more information:

* [CRAN vignette](https://cran.r-project.org/web/packages/ggridges/vignettes/introduction.html)  
* [Wilke's Github](https://github.com/wilkelab/ggridges) 


<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />

# The Data: Billboard 200 Tracks

This data set is from [Components One's Datasets](https://components.one/datasets/billboard-200/). It's a large database containing data on 340,000 tracks from Billboard 200 albums released from 1963-2019. Included for each track is the following:


* Track name
* Track ID on Spotify
* Album name
* Album ID on Spotify
* Artist name
* Duration
* Release date of the album
* Spotify's EchoNest acoustic data:
  * Acousticness
  * Danceability
  * Energy
  * Instrumentalness
  * Liveness
  * Loudness
  * Speechiness
  * Key
  * Time signature
  * Valence
  
### A glimpse of the data:

<table class="table table-striped" style="margin-left: auto; margin-right: auto;">
 <thead>
  <tr>
   <th style="text-align:left;"> Song </th>
   <th style="text-align:left;"> Artist </th>
   <th style="text-align:center;"> Year </th>
   <th style="text-align:center;"> Decade </th>
   <th style="text-align:center;"> Song Danceability </th>
   <th style="text-align:center;"> Year Danceability </th>
   <th style="text-align:center;"> Decade Danceability </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> A </td>
   <td style="text-align:left;"> Cartel </td>
   <td style="text-align:center;"> 2005 </td>
   <td style="text-align:center;"> 2000's </td>
   <td style="text-align:center;"> 0.28 </td>
   <td style="text-align:center;"> 0.55 </td>
   <td style="text-align:center;"> 0.55 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> A </td>
   <td style="text-align:left;"> Barenaked Ladies </td>
   <td style="text-align:center;"> 1994 </td>
   <td style="text-align:center;"> 1990's </td>
   <td style="text-align:center;"> 0.78 </td>
   <td style="text-align:center;"> 0.55 </td>
   <td style="text-align:center;"> 0.57 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> A-1 Performance </td>
   <td style="text-align:left;"> AZ </td>
   <td style="text-align:center;"> 2002 </td>
   <td style="text-align:center;"> 2000's </td>
   <td style="text-align:center;"> 0.72 </td>
   <td style="text-align:center;"> 0.57 </td>
   <td style="text-align:center;"> 0.55 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> A-11 </td>
   <td style="text-align:left;"> Jamey Johnson </td>
   <td style="text-align:center;"> 2012 </td>
   <td style="text-align:center;"> 2010's </td>
   <td style="text-align:center;"> 0.73 </td>
   <td style="text-align:center;"> 0.51 </td>
   <td style="text-align:center;"> 0.53 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> A-11 </td>
   <td style="text-align:left;"> Buck Owens </td>
   <td style="text-align:center;"> 1995 </td>
   <td style="text-align:center;"> 1990's </td>
   <td style="text-align:center;"> 0.55 </td>
   <td style="text-align:center;"> 0.56 </td>
   <td style="text-align:center;"> 0.57 </td>
  </tr>
</tbody>
</table>

# Representation Description

I found the idea of "danceability" interesting; I wanted to see how danceability changed over time. There are a lot of years represented in this dataset. Initial data exploration showed that data for tracks before 1960 were much less abundant than other years (ex. there were 4 tracks from the 1930's and 10,000+ from 1999 alone) so I decided to exclude data from before 1960. I was still left with a lot of data so I figured that the best way to quickly see a trend was through the use of color. What I'm trying to show in this graph is that the 1980's and 90's have higher danceability scores than other decades.

On the x-axis is the danceability score, ranging from 0 to 1. On the y-axis is each year (or decade). (In one iteration of this graph I've used `facet_wrap()` to split up the data by decade.) There is a continuous color scale used to encode for the average daceability rating of the given year (or decade). Transparency is used to allow for easier discernability of the underlying ridges. Each ridge represents the distribution of danceability scores for all tracks released that year.

# How to interpret

The most danceable era should be that with the brightest (most yellow/white) ridges. Here it is clear that the 1980's and '90s are brightest. The peaks of the ridges should also line up somewhat continuously and show the trend for how danceability changes from year to year, in addition to the color trend (re: redundancy). A more subtle trend is that the variance within each year seems to become wider over time as well. This graph makes the identification of continuous trends as well as stark outliers quite easy.

# Presentation Tips

To draw attention to specific ridges, callout annotations can be used. Color can be used on a continuous scale to observe relative differences between ridges. Discrete colors can be used to compare between categorical variables with no ordinal relationship (in this case I suggest a color scale that does not imply order).

# Variations and Alternatives

Ridgeline plots are related to histograms, density plots, and violin plots. Compared to ridgeline plots these all have the disadvantage of taking up more space. Histograms bin the data into a given number of bins and therefore don't have the smooth look of ridgeline plots but do indicate changes from bin to bin with more fidelity. Density plots are ridgeline plots that are either overplotted or separated into facets. They accomplish the same thing as ridgeline plots but in the case of overplotting there is no sense of change across variables and in the case of faceting they are more difficult to compare. Violin plots are very similar to the ridgeline plot, especially the half violin. Full violin plots would take up substantially more space than the ridgeline plot.

## The Ridgeline Plot(s)

### Danceability by Decade


```r
# Filter to only include data after 1960
decadeplot <- ggplot(track_data %>% filter(year >= "1960"), aes(x = danceability, y = decade, fill = DecadeDanceability)) +
  geom_density_ridges(scale = 4, alpha = 0.9, color = "red4") +
  scale_y_discrete(expand = c(0, 0)) +
  scale_x_continuous(expand = c(0, 0)) +
  coord_cartesian(clip = "off") + # to avoid clipping of the top bit of the top curve
  scale_fill_viridis(option = "inferno") +
  theme_minimal() +
  labs(fill = "Mean Decade\nDanceability\nScore",
       title = "1980's & 90's: The Most Danceable Decades?",
       subtitle = 'How does the "danceability" of music change over time?',
       x = "Song Danceability Score",
       y = "Decade") +
  theme(axis.title.y = element_text(hjust = 0.25)) # Move y lable to be in the center of the y-axis labels, not the whole y-axis
decadeplot
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />




### Danceability by Year: Faceted by Decade


```r
ggplot(track_data %>% filter(year >= "1960"), aes(x = danceability, y = year, fill = YearDanceability)) +
  geom_density_ridges(scale = 4, alpha = 0.9, color = "red4") + 
  facet_wrap(~ decade, scales = "free_y", nrow = 1) + # Need free_y scale otherwise the plots would not be aligned
  scale_y_discrete(expand = c(0,0,0, 4)) + # The facet labels were covering the top of the top curve so I introduced some padding
  scale_x_continuous(expand = c(0, 0)) +
  coord_cartesian(clip = "off") + # to avoid clipping of the top bit of the top curve
  scale_fill_viridis(option = "inferno") +
  theme_minimal() +
  labs(fill = "Mean Year\nDanceability\nScore",
       title = "1980's & 90's: The Most Danceable Decades?",
       subtitle = 'How does the "danceability" of music change over time?',
       x = "Song Danceability Score",
       y = "Year") +
  theme(strip.background = element_rect(color = "white", fill = "lightgray"), # Format the facel labels
        axis.title.y = element_text(hjust = 0.35)) # Change the justification of the y-axis label
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="1152" />

### Danceability by Year


```r
ggplot(track_data %>% filter(decade >= "1960"), aes(x = danceability, y = year, fill = YearDanceability)) +
  geom_density_ridges(scale = 4, alpha = 0.9, color = "red4") + 
  scale_y_discrete(expand = c(0, 0)) +
  scale_x_continuous(expand = c(0, 0)) +
  coord_cartesian(clip = "off") + # to avoid clipping of the top bit of the top curve
  scale_fill_viridis(option = "inferno") +
  theme_minimal() +
  labs(fill = "Mean Year\nDanceability\nScore",
       title = "1980's & 90's: The Most Danceable Decades?",
       subtitle = 'How does the "danceability" of music change over time?',
       x = "Song Danceability Score",
       y = "Year")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="576" />
