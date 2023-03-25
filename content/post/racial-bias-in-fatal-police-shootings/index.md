---
title: Racial Bias in Fatal Police Shootings
author: ''
date: '2020-07-18'
slug: racial-bias-in-fatal-police-shootings
categories: []
tags:
  - data viz
  - how to
subtitle: ''
summary: ''
authors: []
lastmod: '2020-07-18T12:33:05-07:00'
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
## Warning: package 'knitr' was built under R version 4.2.3
```



# Presentation:

<p> Hear me talk through the making of this visualization. All code featured in the presentation (and more!) can be found in the sections below. </p>

<video width="800" style="display:block; margin:0 auto;" controls>
  <source src="fatal-police-shootings-recorded-presentation-v2.mp4" type="video/mp4">
</video>


# The Data:

Despite the frequency with which incidents of police brutality occurs in the US, an official centralized record of police violence does not exist. In 2015 The Washington Post took matters into their own hands and created a database of every fatal shooting in the US by a police officer. The record is regularly updated (right now it is current as of June 8, 2020). Currently there are **5401** fatalities represented in the dataset. Here is just a glimpse at the data:


```
## # A tibble: 5,401 × 12
##    name    date       armed   age gender race  city  state threa…¹ flee  body_…²
##    <chr>   <date>     <fct> <dbl> <fct>  <fct> <chr> <fct> <fct>   <fct> <lgl>  
##  1 Tim El… 2015-01-02 gun      53 M      A     Shel… WA    attack  Not … FALSE  
##  2 Lewis … 2015-01-02 gun      47 M      W     Aloha OR    attack  Not … FALSE  
##  3 John P… 2015-01-03 unar…    23 M      H     Wich… KS    other   Not … FALSE  
##  4 Matthe… 2015-01-04 toy …    32 M      W     San … CA    attack  Not … FALSE  
##  5 Michae… 2015-01-04 nail…    39 M      H     Evans CO    attack  Not … FALSE  
##  6 Kennet… 2015-01-04 gun      18 M      W     Guth… OK    attack  Not … FALSE  
##  7 Kennet… 2015-01-05 gun      22 M      H     Chan… AZ    attack  Car   FALSE  
##  8 Brock … 2015-01-06 gun      35 M      W     Assa… KS    attack  Not … FALSE  
##  9 Autumn… 2015-01-06 unar…    34 F      W     Burl… IA    other   Not … TRUE   
## 10 Leslie… 2015-01-06 toy …    47 M      B     Knox… PA    attack  Not … FALSE  
## # … with 5,391 more rows, 1 more variable: signs_of_mental_illness <lgl>, and
## #   abbreviated variable names ¹​threat_level, ²​body_camera
```

While the Washington Post data allows us to see raw number of shootings that occur, if we want to see how Black and White people are differentially targeted by police we are going to need to calculate the *proportion* of Black/White people that are shot. To do this I need data on the population size of Black and White people living in the US. I got this data from the US Census Bureau.

### Explanation of Variables in Washington Post dataset

*Qualitative variables*: `name`, `armed`, `gender`, `race`, `city`, `state`, `signs_of_mental_illness`, `threat_level`, `flee`, `body_camera`  
*Quantitative variables*: `age`

**armed**: Did the victim have a weapon? If yes, what kind? There are **89** weapons represented in the dataset, ranging from a gun to a chair.

**race**:  

*  `W` = White, non-Hispanic (2468)  
*  `B` = Black, non-Hispanic (1291)  
*  `A` = Asian (93)  
*  `N` = Native American (78)  
*  `H` = Hispanic (900)  
*  `O` = Other (48)  
*  `NA` = Unknown (523)

**signs_of_mental_illness**: Did the victim exhibit signs of mental illness?  
`TRUE` = 1216  
`FALSE` = 4185

**threat_level**: Was there a direct and immediate threat to the life of the police officer? This includes incidents where officers or others were shot at, threatened with a gun, attacked with other weapons or physical force, etc.  
`attack` = 3487  
`other` = 1914

**flee**: Was the victim moving away from the officers?  
`Not fleeing` = 3406  
`Car` = 898  
`Foot` = 689  
`Other` = 162  
`NA` = 246

**body_camera**: Reported as `TRUE` if news reports indicated an officer was wearing a body camera *and* it may have recorded at least a portion of the incident.  
`TRUE` = 615  
`FALSE` = 4796


### Links to datasets

*  [Shooting fatality data c/o Washington Post](https://github.com/washingtonpost/data-police-shootings)
*  [Population data c/o US Census Bureau](https://www.census.gov/data/tables/time-series/demo/popest/2010s-national-total.html)

# The Visualization:




```
## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
## ℹ Please use `linewidth` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="432" />

### Intended Audience

It's never fun to talk about violence, especially violence present in a system that is supposed to hold honor, however I think that it is important that *everyone* be made aware of the racial bias that is reflected in statistics describing police brutality. This audience for this visualization is therefore broad: people of all ages, genders, education levels, socioeconomic status, etc. This plot is easily understood even without previous experience with dumbbell plots.

### About Dumbbell Plots

Also called connected dot plots or dumbbell dot plots, dumbbell plots are a version of lollipop charts that features comparison between 2 (or 3) groups. Lollipop charts are closely related to bar charts but are only effective in conveying information about a single group. By using the dumbbell layout you can increase the depth of information conveyed. While a grouped bar chart would also convey information about the two groups, dumbbell plots take advantage of the Gestalt principle of continuity to aid the eye in following the directionality of the relationship.

### How to Read it and What to Look For

Dumbbell plots consist of 2 (or 3) points connected by a line. Often there are multiple "dumbbells" to represent different groups, timepoints, etc. The points indicate the numerical (or categorical) value for a group. The line connecting two points exists to indicate the relationship between the two points, both in directionality and magnitude. It also functions to guide the eye in appropriate grouping of points. In my visualization I am trying to convey the relationship between Black and White victims of fatal shootings. The x-axis carries information about the number of victims. The y-axis represents time, in years. We can then follow the incidence of shootings across time for both races.

### Representation Description/Intended Message

My goal for this visualization was to illustrate how Black and White populations are targetd differently by gun violence, specifically gun violence in the contect of fatal line-of-duty police shootings. To do this, I cannot present the raw number of shootings in each population becuase the population sizes are vastly different (Black people *are* a minority, after all). Instead, I'm showing the number of fatal shootings per 1 million people of a given race (ie. the proportion of each population that dies due to gun violence). Below is a table of those numbers:

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;border-bottom: 0;">
 <thead>
<tr>
<th style="empty-cells: hide;border-bottom:hidden;" colspan="1"></th>
<th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Total Number of<br>Fatal Shootings</div></th>
<th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Total Population<br>(in Millions)</div></th>
<th style="border-bottom:hidden;padding-bottom:0; padding-left:3px;padding-right:3px;text-align: center; " colspan="2"><div style="border-bottom: 1px solid #ddd; padding-bottom: 5px; ">Deaths per 1 Million People of Indicated Race</div></th>
</tr>
  <tr>
   <th style="text-align:left;"> Year </th>
   <th style="text-align:center;"> White </th>
   <th style="text-align:center;"> Black </th>
   <th style="text-align:center;"> White </th>
   <th style="text-align:center;"> Black </th>
   <th style="text-align:center;"> White </th>
   <th style="text-align:center;"> Black </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;width: 6em; "> 2015 </td>
   <td style="text-align:center;width: 6em; "> 497.00 </td>
   <td style="text-align:center;width: 6em; "> 258.00 </td>
   <td style="text-align:center;width: 6em; "> 247.78 </td>
   <td style="text-align:center;width: 6em; "> 42.63 </td>
   <td style="text-align:center;width: 6em; "> 2.01 </td>
   <td style="text-align:center;width: 6em; "> 6.05 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 6em; "> 2016 </td>
   <td style="text-align:center;width: 6em; "> 468.00 </td>
   <td style="text-align:center;width: 6em; "> 234.00 </td>
   <td style="text-align:center;width: 6em; "> 248.50 </td>
   <td style="text-align:center;width: 6em; "> 43.00 </td>
   <td style="text-align:center;width: 6em; "> 1.88 </td>
   <td style="text-align:center;width: 6em; "> 5.44 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 6em; "> 2017 </td>
   <td style="text-align:center;width: 6em; "> 459.00 </td>
   <td style="text-align:center;width: 6em; "> 224.00 </td>
   <td style="text-align:center;width: 6em; "> 249.62 </td>
   <td style="text-align:center;width: 6em; "> 43.50 </td>
   <td style="text-align:center;width: 6em; "> 1.84 </td>
   <td style="text-align:center;width: 6em; "> 5.15 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 6em; "> 2018 </td>
   <td style="text-align:center;width: 6em; "> 454.00 </td>
   <td style="text-align:center;width: 6em; "> 229.00 </td>
   <td style="text-align:center;width: 6em; "> 250.14 </td>
   <td style="text-align:center;width: 6em; "> 43.80 </td>
   <td style="text-align:center;width: 6em; "> 1.81 </td>
   <td style="text-align:center;width: 6em; "> 5.23 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 6em; "> 2019 </td>
   <td style="text-align:center;width: 6em; "> 405.00 </td>
   <td style="text-align:center;width: 6em; "> 249.00 </td>
   <td style="text-align:center;width: 6em; "> 249.42 </td>
   <td style="text-align:center;width: 6em; "> 43.43 </td>
   <td style="text-align:center;width: 6em; "> 1.62 </td>
   <td style="text-align:center;width: 6em; "> 5.73 </td>
  </tr>
  <tr>
   <td style="text-align:left;width: 6em; "> 2020 </td>
   <td style="text-align:center;width: 6em; "> 424.69 </td>
   <td style="text-align:center;width: 6em; "> 222.67 </td>
   <td style="text-align:center;width: 6em; "> 249.73 </td>
   <td style="text-align:center;width: 6em; "> 43.58 </td>
   <td style="text-align:center;width: 6em; "> 1.70 </td>
   <td style="text-align:center;width: 6em; "> 5.11 </td>
  </tr>
</tbody>
<tfoot>
<tr><td style="padding: 0; " colspan="100%">
<sup>*</sup> 2020 data is projected based on current data.</td></tr>
<tr><td style="padding: 0; " colspan="100%">
<sup>†</sup> Data is current as of June 8, 2020.</td></tr>
</tfoot>
</table>

From both the table and the visualization it is clear that Black people are consistently disproportionately targeted by gun violence in this context. This holds up over all years for which there is data available.

### Presentation

I chose to make this plot in portrait orientation (vs landscape) because I thought it made the relationships more readily interpretable. I removed major and minor grids on the y-axis since I found them distracting and the segment connecting the two points already provides the same information. While years are technically quantitative, the way I'm using them makes them almost categorical and thus it is less important to have grid lines. I did keep grid lines for the x-axis to help interpretation of the value of the points. The color choices were deliberate and were chosen to represent skin color. A text and arrow annotation was used to indicate that the 2020 data is projected based on current data. A footnote was used to indicate how current the data is. I positioned the legend under the title such that it is readily available but doesn't occupy too much space. For the title I used a bold-face font for the title and regular font for the subtitle. I found that by using that bold font I was able to better visually separate the title and subtitle.

### How I created it

**1.  Import and wrangle the shooting data. **


```r
shootings_raw <- read_csv("fatal-police-shootings-data.csv")
glimpse(shootings_raw)
```

```
## Rows: 5,401
## Columns: 14
## $ id                      <dbl> 3, 4, 5, 8, 9, 11, 13, 15, 16, 17, 19, 21, 22,…
## $ name                    <chr> "Tim Elliot", "Lewis Lee Lembke", "John Paul Q…
## $ date                    <date> 2015-01-02, 2015-01-02, 2015-01-03, 2015-01-0…
## $ manner_of_death         <chr> "shot", "shot", "shot and Tasered", "shot", "s…
## $ armed                   <chr> "gun", "gun", "unarmed", "toy weapon", "nail g…
## $ age                     <dbl> 53, 47, 23, 32, 39, 18, 22, 35, 34, 47, 25, 31…
## $ gender                  <chr> "M", "M", "M", "M", "M", "M", "M", "M", "F", "…
## $ race                    <chr> "A", "W", "H", "W", "H", "W", "H", "W", "W", "…
## $ city                    <chr> "Shelton", "Aloha", "Wichita", "San Francisco"…
## $ state                   <chr> "WA", "OR", "KS", "CA", "CO", "OK", "AZ", "KS"…
## $ signs_of_mental_illness <lgl> TRUE, FALSE, FALSE, TRUE, FALSE, FALSE, FALSE,…
## $ threat_level            <chr> "attack", "attack", "other", "attack", "attack…
## $ flee                    <chr> "Not fleeing", "Not fleeing", "Not fleeing", "…
## $ body_camera             <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALS…
```

If we take a quick look at how it was imported, you can see that a lot of the variables were imported as character class when they should operate as factor class. I'll use `forcats::as_factor()` to turn those variables to factor class. I also need to create a new "Year" column since I am interested in the number of shootings per year.


```r
shootings_raw <- shootings_raw %>% 
  mutate(manner_of_death = as_factor(manner_of_death),
         armed = as_factor(armed),
         gender = as_factor(gender),
         race = as_factor(race),
         state = as_factor(state),
         threat_level = as_factor(threat_level),
         flee = as_factor(flee))

# Extract "Year" from "date"
shootings_raw$Year <- as.numeric(format(shootings_raw$date, '%Y'))
```

Now I'm going to use `summarise()` to calculate the number of shootings per year for each race. Then I'm going to filter the data to only look at data for Black and White victims.


```r
# Count W/B shootings per year
shootings <- shootings_raw %>% 
  group_by(Year, race) %>% 
  summarise(fatalshootings = n()) %>% 
  filter(race == "W" | race == "B")
```

```
## `summarise()` has grouped output by 'Year'. You can override using the
## `.groups` argument.
```

```r
shootings %>% head(12)
```

```
## # A tibble: 12 × 3
## # Groups:   Year [6]
##     Year race  fatalshootings
##    <dbl> <fct>          <int>
##  1  2015 W                497
##  2  2015 B                258
##  3  2016 W                468
##  4  2016 B                234
##  5  2017 W                459
##  6  2017 B                224
##  7  2018 W                454
##  8  2018 B                229
##  9  2019 W                405
## 10  2019 B                249
## 11  2020 W                185
## 12  2020 B                 97
```

In order to combine this with my census data, I'm going to need to pivot the data wider such that each row represents a single year.


```r
shootings <- shootings %>% 
  pivot_wider(names_from = race, values_from = fatalshootings)

shootings
```

```
## # A tibble: 6 × 3
## # Groups:   Year [6]
##    Year     W     B
##   <dbl> <int> <int>
## 1  2015   497   258
## 2  2016   468   234
## 3  2017   459   224
## 4  2018   454   229
## 5  2019   405   249
## 6  2020   185    97
```

**2.  Import and wrangle the census data.**  


```r
# How many B/W people are in America from 2015-2020?
census_raw <- read_xlsx("census-data.xlsx")
# Convert units from millions
census <- census_raw %>% 
  mutate(W_pop = White * 1000000,
         B_pop = Black * 1000000)

census
```

```
## # A tibble: 21 × 5
##     Year White Black      W_pop     B_pop
##    <dbl> <dbl> <dbl>      <dbl>     <dbl>
##  1  2020  250.  43.6 249726667. 43577778.
##  2  2019  249.  43.4 249420000  43433333.
##  3  2018  250.  43.8 250140000  43800000 
##  4  2017  250.  43.5 249620000  43500000 
##  5  2016  248.  43   248500000  43000000 
##  6  2015  248.  42.6 247780000  42630000 
##  7  2014  247.  42.2 246660000  42160000 
##  8  2013  246.  41.7 245590000  41710000 
##  9  2012  245.  41.3 244510000  41260000 
## 10  2011  243.  40.8 243380000  40810000 
## # … with 11 more rows
```

**3.  Combine the census data with the shooting data.**  


```r
# Combine shooting & census data
shootings <- inner_join(shootings, census, by="Year") %>% 
  rename(W_mil = White,
         B_mil = Black) %>% 
  # Calculate the number of shootings per 1 million people
  mutate(W_per = W*1000000/W_pop,
         B_per = B*1000000/B_pop)
```



**4.  For 2020: calculate the projected numbers.** I'll do this by (1) calculating the number of days represented in the data and (2) dividing my values by the number of days and multiply by 365.  


```r
# Calculate projected data for 2020
# How many days of data is represented for 2020 data? Latest date is 06/08/2020
days <- as.numeric(as.Date(as.character("2020/06/08"), format="%Y/%m/%d")-as.Date(as.character("2020/01/01"), format="%Y/%m/%d"))

# Calculate projected shootings for 2020
proj2020 <- shootings[6,]
proj2020 <- tibble(Year = "2020 proj",
                   W = pull(proj2020[1,2])*365/days,
                   B = pull(proj2020[1,3])*365/days,
                   W_mil = pull(proj2020[1,4]),
                   B_mil = pull(proj2020[1,5]), 
                   W_pop = pull(proj2020[1,6]),
                   B_pop = pull(proj2020[1,7]),
                   W_per = pull(proj2020[1,8])*365/days,
                   B_per = pull(proj2020[1,9])*365/days)  
proj2020
```

```
## # A tibble: 1 × 9
##   Year          W     B W_mil B_mil      W_pop     B_pop W_per B_per
##   <chr>     <dbl> <dbl> <dbl> <dbl>      <dbl>     <dbl> <dbl> <dbl>
## 1 2020 proj  425.  223.  250.  43.6 249726667. 43577778.  1.70  5.11
```

```r
# Add projected values to shootings data frame
shootings <- shootings %>% ungroup() %>% mutate(Year = as.character(Year))
shootings <- bind_rows(shootings, proj2020)
shootings
```

```
## # A tibble: 7 × 9
##   Year          W     B W_mil B_mil      W_pop     B_pop W_per B_per
##   <chr>     <dbl> <dbl> <dbl> <dbl>      <dbl>     <dbl> <dbl> <dbl>
## 1 2015       497   258   248.  42.6 247780000  42630000  2.01   6.05
## 2 2016       468   234   248.  43   248500000  43000000  1.88   5.44
## 3 2017       459   224   250.  43.5 249620000  43500000  1.84   5.15
## 4 2018       454   229   250.  43.8 250140000  43800000  1.81   5.23
## 5 2019       405   249   249.  43.4 249420000  43433333. 1.62   5.73
## 6 2020       185    97   250.  43.6 249726667. 43577778. 0.741  2.23
## 7 2020 proj  425.  223.  250.  43.6 249726667. 43577778. 1.70   5.11
```

**5.  Time to get the data into the format we need for ggplot.** First, I need to pivot the data longer. Then I can get rid of the 2020 data and replace it with the projected data.  


```r
# Pivot long
shootings_long <- shootings %>% 
  pivot_longer(W_per:B_per, names_to = "race", values_to = "fatal_per")

# Get rid of 2020, replace it with the projected numbers
shootings <- shootings %>% filter(Year != "2020")
shootings[6,1] = "2020"
shootings_long <- shootings_long %>% filter(Year != "2020")
shootings_long[11,1] = "2020"; shootings_long[12,1] = "2020"

shootings_long
```

```
## # A tibble: 12 × 9
##    Year      W     B W_mil B_mil      W_pop     B_pop race  fatal_per
##    <chr> <dbl> <dbl> <dbl> <dbl>      <dbl>     <dbl> <chr>     <dbl>
##  1 2015   497   258   248.  42.6 247780000  42630000  W_per      2.01
##  2 2015   497   258   248.  42.6 247780000  42630000  B_per      6.05
##  3 2016   468   234   248.  43   248500000  43000000  W_per      1.88
##  4 2016   468   234   248.  43   248500000  43000000  B_per      5.44
##  5 2017   459   224   250.  43.5 249620000  43500000  W_per      1.84
##  6 2017   459   224   250.  43.5 249620000  43500000  B_per      5.15
##  7 2018   454   229   250.  43.8 250140000  43800000  W_per      1.81
##  8 2018   454   229   250.  43.8 250140000  43800000  B_per      5.23
##  9 2019   405   249   249.  43.4 249420000  43433333. W_per      1.62
## 10 2019   405   249   249.  43.4 249420000  43433333. B_per      5.73
## 11 2020   425.  223.  250.  43.6 249726667. 43577778. W_per      1.70
## 12 2020   425.  223.  250.  43.6 249726667. 43577778. B_per      5.11
```

**6.  Build the plot in `ggplot`.** A dumbbell plot can be made in `ggplot` using a combination of `geom_segment()` and `geom_point()`. Let's look at the most basic version. Note that I'm using data from `shootings` for `geom_segment()` and data from `shootings_long` for `geom_point()`. Note that I'm already implementing some customizations. I've specified the size of line to use for the segment. I've also specified the size, shape, and outline color for the points.


```r
ggplot() +
  geom_segment(data = shootings, 
               mapping = aes(x=W_per, xend=B_per, y=Year, yend=Year), 
               size = 1) +
  geom_point(data = shootings_long, 
             mapping = aes(x = fatal_per, y = Year, fill = race), 
             size=5, shape = 21, color = "black")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-15-1.png" width="672" />

There's a lot that I want to change:  

*  I want the vizualization to be in "portrait" orientation. I'll specify the dimensions in my code chunk options within the chunk header using `fig.width=x` and `fig.height=y`. (Where x and y are numbers, in inches.)  
*  The x-axis should start at 0. I'm also going to expand on the right side so that there's roughly equal padding on both sides of the dumbbells. For this I'll use `coord_cartesian()`.  
*  I want to customize my colors. For this I'll specify the names and hex codes manually and use `scale_fill_manual()` to implement the names and values.  
*  I don't want to use the default ggplot theme. Instead I'm going to use `theme_minimal()` to strip down everything to a lighter palette. Under `theme()` I'm also going to add additional customizations, namely the legend size, location, and orientation as well as the title, subtitle, and footnote formatting. I'm also going to remove the y-axis gridlines.  
*  To make it very obvious that the 2020 data is projected data, I'm adding a text annotation (`annotate(geom = "text")`) and an arrow annotation (`annotate(geom = "curve")`).  
*  Finally, I will specify my title, subtitle, footnote, legend title, and axis labels using `labs()`.  

See the final code and visualization:


```r
mycolors <- c("W_per" = "#fed2b7", "B_per" = "#55160d")

ggplot() +
  geom_segment(data = shootings, 
               mapping = aes(x=W_per, xend=B_per, y=Year, yend=Year), 
               size = 1) +
  geom_point(data = shootings_long, 
             mapping = aes(x = fatal_per, y = Year, group = race, fill = race), 
             size=5, shape = 21, color = "black") +
  
  # Customize appearance
  coord_cartesian(xlim = c(0,8)) +
  scale_fill_manual(values = mycolors,
                    labels = c("Black","White")) +
  scale_y_discrete(expand = c(0.1,0,0,1)) + # Expand margins on top and bottom of plot
  theme_minimal() +
  theme(legend.position = c(0.115,0.98),
        legend.background = element_rect(fill = "white", color = "white"),
        legend.text = element_text(size = 10),
        legend.title = element_text(size = 10),
        legend.direction = "horizontal",
        plot.title = element_text(face = "bold", size = 15, hjust=0.2),
        plot.title.position = "plot",
        plot.subtitle = element_text(hjust = 0.1),
        plot.caption = element_text(hjust = 0),
        panel.grid.major.y = element_blank()) +
  
  # Arrow annotation to projected data
  annotate(geom = "curve", size = 1, color = "black",
           x = 6.5, y = 5.8, xend = 5.35, yend = 6.1, curvature = 0.7,
           arrow = arrow(length = unit(2.5, "mm"))) +
  
  # Text annotation to projected data
  annotate(geom = "text", x = 6.5, y = 5.63,
           label = "projected based on\ncurrent data", color = "black", 
           size = 3.5, lineheight = 0.8, hjust = 0.5) +  
  
  # Customize labels
  labs(title = "Police Shooting Fatalities by Victim Race",
       subtitle = "Racial bias is evident in the police system. Black people \nare disproportionately victimized and murdered by police.",
       fill = "Race",
       y = "Year",
       x = "Number of Fatal Shootings\n(per 1,000,000 people of indicated race)",
       caption = "*Data current as of June 8, 2020")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-16-1.png" width="432" style="display: block; margin: auto;" />
