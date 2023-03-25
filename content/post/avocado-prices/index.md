---
title: Avocado Prices in the US
author: ''
date: '2020-10-14'
slug: avocado-prices
categories: []
tags:
  - data viz
subtitle: ''
summary: ''
authors: []
lastmod: '2020-10-14T16:09:17-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: yes
projects: []
---





### The Data: Avocado Prices

This data was found on [Kaggle](https://www.kaggle.com/timmate/avocado-prices-2020#), originally pulled from the Hass Avocado Board website in 2018 and then updated in 2020.

From the Hass Avocado Board:

> The table below represents weekly 2018 retail scan data for National retail volume (units) and price. Retail scan data comes directly from retailers’ cash registers based on actual retail sales of Hass avocados. Starting in 2013, the table below reflects an expanded, multi-outlet retail data set. Multi-outlet reporting includes an aggregation of the following channels: grocery, mass, club, drug, dollar and military. The Average Price (of avocados) in the table reflects a per unit (per avocado) cost, even when multiple units (avocados) are sold in bags. The Product Lookup codes (PLU’s) in the table are only for Hass avocados. Other varieties of avocados (e.g. greenskins) are not included in this table.


### The Visualization: How do avocado sales vary across the US?



<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="480" />

This plot not only looks at avocado sales in different regions, it also shows the number of avocados sold by avocado size. While this type of visualization does not give numerical readout, it does provide a quick glance at trends. The most striking trend may be that more small/medium avocados are sold than large/extra large avocados.

### The Details: How the Plot was Made

This plot was made in ggplot2 using `geom_tile()`. The theme is `theme_classic()` with some minor modifications to the title/subtitle. One point that needs to be made is that the data was cleaned such that it was tidy and I then used `summarise()` to get the volume sold numbers that I feed into ggplot.


```r
ggplot(data = avocado_summary, mapping = aes(x = size, y = geography, fill = num_sold)) + 
  geom_tile() +
  theme_classic() +
  scale_fill_gradient(low = "white", high = "red") +
  scale_x_discrete(labels = c("Small/Medium", "Large", "Extra Large")) +
  theme(plot.title.position = "plot",
        plot.title = element_text(hjust = 0.5),
        plot.subtitle = element_text(hjust = 0.5),
        legend.title = element_text(hjust = 0)) +
  labs(title = "Number of Hass Avocados Sold in U.S. by Region",
       subtitle = "Total number of avocados sold from 2015-2020.",
       x = "Avocado Size", 
       y = "", 
       fill = "Number Sold \n(log scale)")
```

