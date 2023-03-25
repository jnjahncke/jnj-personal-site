---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Sick With Mono"
author: ''
date: '2021-05-25'
slug: sick-with-mono
tags: ['data viz']
subtitle: ''
summary: ''
authors: []
categories: []
lastmod: '2021-05-25T10:06:10-07:00'
featured: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: true

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---





### The Data: Heart Rate and Temperature Measurements While Sick



Somehow I managed to get mono during a global pandemic at the age of 28. While I was sick I kept track of my temperature just using an oral thermometer, noting the time and temperature. My heart rate data comes from my Fitbit and represents my resting heart rate for the day. Here's a glimpse of the data (first 10 rows):

<style>
  .col2 {
    columns: 2 200px;         /* number of columns and width in pixels*/
    -webkit-columns: 2 200px; /* chrome, safari */
    -moz-columns: 2 200px;    /* firefox */
  }
  .col3 {
    columns: 3 100px;
    -webkit-columns: 3 100px;
    -moz-columns: 3 100px;
  }
</style>

<div class="col2">

```
## [1] "temps"
```

```
## # A tibble: 10 × 3
##    Date       Time                 Temp
##    <date>     <dttm>              <dbl>
##  1 2021-05-11 1899-12-31 19:48:00 101. 
##  2 2021-05-11 1899-12-31 20:41:00 101. 
##  3 2021-05-12 1899-12-31 08:13:00  98.2
##  4 2021-05-12 1899-12-31 13:38:00 100. 
##  5 2021-05-12 1899-12-31 16:42:00  98.6
##  6 2021-05-12 1899-12-31 18:29:00 100. 
##  7 2021-05-12 1899-12-31 19:22:00 101. 
##  8 2021-05-12 1899-12-31 19:39:00 101. 
##  9 2021-05-12 1899-12-31 20:09:00 101. 
## 10 2021-05-12 1899-12-31 20:37:00 101.
```

```
## [1] "hr"
```

```
## # A tibble: 10 × 2
##    Date          HR
##    <date>     <dbl>
##  1 2021-05-01    76
##  2 2021-05-02    74
##  3 2021-05-03    74
##  4 2021-05-04    75
##  5 2021-05-05    75
##  6 2021-05-06    78
##  7 2021-05-07    78
##  8 2021-05-08    77
##  9 2021-05-09    79
## 10 2021-05-10    81
```
</div>

Why does the time column have dates from the year 1899? Probably an Excel thing. Doesn't matter for my purposes, I'll only use the HH:MM:SS data from that column.

### The Visualization: How does my heart rate and temperature change over time?



<img src="staticunnamed-chunk-5-1.png" width="1008" />

### The Details: How the Plot was Made

For this visualization we need to load the following packages: `tidyverse`, `lubridate`, `patchwork`, `extrafont`, `showtext`. `lubridate` will help us handle working with datetime. (FAQ: Isn't `lubridate` a part of `tidyverse`? Yes but. Yes, it gets installed when you install `tidyverse`. BUT it isn't one of the "core" `tidyverse` packages that gets loaded when you load `tidyverse` so you need to load it separately.) `patchwork` will be used to combine the two plots. `extrafont` and `showtext` are for the font used in the plot title.

I had to fiddle with chunk options a bit to get the font to display in rmarkdown. Here's what I did ([source](https://cran.rstudio.com/web/packages/showtext/vignettes/introduction.html)):

*  Load `extrafont` and `showtext`.    
*  Import the desired font from Google Fonts using `font_add_google("Merriweather")`.    
*  Run `showtext_auto(enable = TRUE)` to automatically render the fonts.  
*  In the setup chunk include `fig_retina = 1` so that the fonts in the HTML output render at the correct size.  
*  In the chunk(s) where you are displaying the visualization include `fig.showtext = TRUE` in the chunk options.  

Here is the code for the two plots separately:


```r
temp_plot <- temps %>% ggplot(aes(x = Time, y = Temp, color = Date, group = Date)) +
  geom_hline(yintercept = 100, lty = "dashed", color = "red") +
  geom_hline(yintercept = 98.6, lty = "dashed", color = "black") +
  geom_point(size = 3) +
  coord_cartesian(ylim = c(98,102)) +
  scale_color_viridis_c(option = "A", trans = "date", end = 0.95) +
  scale_x_datetime(date_labels = "%I %p", date_breaks = "2 hours") +
  labs(x = "Time of Day", y = "Temperature (F)") +
  theme_classic() +
  theme(plot.background = element_rect(fill = "grey95", color = "grey95"),
        panel.background = element_rect(fill = "grey95"),
        legend.background = element_rect(fill = "grey95"))

hr_plot <- hr %>%
  ggplot(aes(x = Date, y = HR, color = Date)) +
  geom_point(size = 3) +
  geom_line(size = 1) +
  scale_color_viridis_c(option = "A", trans = "date", end = 0.95) +
  scale_x_date(date_labels = "%b \n %d", date_breaks = "3 days") +
  labs(x = "Date", y = "Heart Rate (bpm)") +
  theme_classic() +
  theme(plot.background = element_rect(fill = "grey95", color = "grey95"),
        panel.background = element_rect(fill = "grey95"),
        legend.background = element_rect(fill = "grey95"),
        legend.position = "none")
```

([Here](https://help.gnome.org/users/gthumb/stable/gthumb-date-formats.html.en) is where I learned about datetime formats.)

Next, I used patchwork to arrange the plots and add/format the title. With patchwork you can create a layout just by adding and/or dividing the plots. I also need to point out here that my code chunk options are the following for this chunk: `{r fig.height=5, fig.width=10.5, fig.showtext = TRUE}`.


```r
hr_plot + temp_plot + plot_layout(widths = c(3.5,7)) + 
  plot_annotation(title = "I Got Mono and Graphed It", 
                  subtitle = "My heart rate and body temperature during the two weeks I was out sick.",
                  theme = theme(plot.title = element_text(family = "Merriweather", size = 20, hjust = 0.07),
                                plot.subtitle = element_text(family = "Merriweather", hjust = 0.095),
                                plot.background = element_rect(fill = "grey95")))
```

<img src="staticunnamed-chunk-7-1.png" width="1008" />

Why the weird `hjust` values? The default justification is left, all the way at the border of the plot, but I wanted the text to start closer to the y-axis so I scooted the text over a bit using `hjust.`
