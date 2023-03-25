---
title: 'Radar Diagrams: Electrical Properties of Brain Cells'
author: ''
date: '2020-06-03'
slug: radar-diagrams-electrical-properties-of-brain-cells
categories: []
tags:
  - data viz
  - how to
subtitle: ''
summary: ''
authors: []
lastmod: '2020-06-03T09:37:10-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: yes
projects: []
---
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />





Radar plots are also known as spider web or polar plots. These charts are useful for conveying information about multiple quantitative variables using multiple axes, arranged in a circle. In R it is [technically possible to use `ggplot2`](https://www.r-bloggers.com/the-grammar-of-graphics-and-radar-charts/) to make these kinds of charts but the `fmsb` package allows for much easier and more readily customizable charts. (See [fmsb's CRAN page](https://cran.r-project.org/web/packages/fmsb/) and [RDocumentation page](https://www.rdocumentation.org/packages/fmsb/versions/0.7.0) for more details.)

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="768" style="display: block; margin: auto;" />

# The Data: NeuroElectro Database of Electrical Properties of Brain Cells

[NeuroElectro](https://www.neuroelectro.org/api/docs/) is a great resource for electrophysiologists in neuroscience. This project extracts information about the electrophysiological properties of neurons from existing literature and integrates it into a centralized database. There are dozens of measurements documented for a large number of cell types in multiple species and preparations. 

*Citation:*
NeuroElectro: a window to the world's neuron electrophysiology data.
Frontiers in Neuroinformatics, April 2014
Tripathy SJ, Savitskaya J, Burton SD, Urban NN, and Gerkin RC
Description: A methods paper outlining the text-mining and manual curation methodology used to construct the NeuroElectro resource.

# Representaiton Description

For my visualization I am focusing on in vitro patch clamp data in mouse tissue, because that's what I work with in the lab. I decided to feature only 6 cell types which were more or less randomly chosen using my personal bias. I chose to focus on only 5 physiological properties even though there were many more in the dataset. Each radar plot here represents a single cell type. My intent is to showcase electrical properties of these cells for quick comparison. This is a lot of data in a small space. The same data could be conveyed using five bar graphs instead (in fact, if you were interested in the absolute measurements, bar graphs would be better.

For the `fmsb` package I need to include the maximum and minimum data for each variable so that it knows how to scale its axes. Here is the data that I am working with:

<table class="table" style="width: auto !important; margin-left: auto; margin-right: auto;border-bottom: 0;">
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:center;"> Input Resistance </th>
   <th style="text-align:center;"> Resting Membrane Potential* </th>
   <th style="text-align:center;"> Capacitance </th>
   <th style="text-align:center;"> Rheobase </th>
   <th style="text-align:center;"> Spike Amplitude </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Max </td>
   <td style="text-align:center;"> 221.53 </td>
   <td style="text-align:center;"> 81.64 </td>
   <td style="text-align:center;"> 614.61 </td>
   <td style="text-align:center;"> 1300.00 </td>
   <td style="text-align:center;"> 86.42 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Min </td>
   <td style="text-align:center;"> 0.00 </td>
   <td style="text-align:center;"> 0.00 </td>
   <td style="text-align:center;"> 0.00 </td>
   <td style="text-align:center;"> 0.00 </td>
   <td style="text-align:center;"> 0.00 </td>
  </tr>
  <tr grouplength="6"><td colspan="6" style="border-bottom: 1px solid;"><strong>Neurons</strong></td></tr>
<tr>
   <td style="text-align:left;padding-left: 2em;" indentlevel="1"> Neocortex pyramidal cell layer 5-6 </td>
   <td style="text-align:center;"> 162.13 </td>
   <td style="text-align:center;"> 70.35 </td>
   <td style="text-align:center;"> 89.23 </td>
   <td style="text-align:center;"> 166.11 </td>
   <td style="text-align:center;"> 79.33 </td>
  </tr>
  <tr>
   <td style="text-align:left;padding-left: 2em;" indentlevel="1"> Neocortex basket cell </td>
   <td style="text-align:center;"> 158.96 </td>
   <td style="text-align:center;"> 67.41 </td>
   <td style="text-align:center;"> 51.31 </td>
   <td style="text-align:center;"> 281.98 </td>
   <td style="text-align:center;"> 61.22 </td>
  </tr>
  <tr>
   <td style="text-align:left;padding-left: 2em;" indentlevel="1"> Hippocampus CA1 pyramidal cell </td>
   <td style="text-align:center;"> 155.91 </td>
   <td style="text-align:center;"> 66.55 </td>
   <td style="text-align:center;"> 94.55 </td>
   <td style="text-align:center;"> 75.60 </td>
   <td style="text-align:center;"> 86.42 </td>
  </tr>
  <tr>
   <td style="text-align:left;padding-left: 2em;" indentlevel="1"> Neostriatum medium spiny neuron </td>
   <td style="text-align:center;"> 121.20 </td>
   <td style="text-align:center;"> 81.64 </td>
   <td style="text-align:center;"> 80.50 </td>
   <td style="text-align:center;"> 263.62 </td>
   <td style="text-align:center;"> 81.59 </td>
  </tr>
  <tr>
   <td style="text-align:left;padding-left: 2em;" indentlevel="1"> Cerebellum Purkinje cell </td>
   <td style="text-align:center;"> 125.31 </td>
   <td style="text-align:center;"> 62.02 </td>
   <td style="text-align:center;"> 614.61 </td>
   <td style="text-align:center;"> 1300.00 </td>
   <td style="text-align:center;"> 81.32 </td>
  </tr>
  <tr>
   <td style="text-align:left;padding-left: 2em;" indentlevel="1"> Hippocampus CA3 pyramidal cell </td>
   <td style="text-align:center;"> 221.53 </td>
   <td style="text-align:center;"> 67.28 </td>
   <td style="text-align:center;"> 208.54 </td>
   <td style="text-align:center;"> 92.00 </td>
   <td style="text-align:center;"> 82.25 </td>
  </tr>
</tbody>
<tfoot><tr><td style="padding: 0; " colspan="100%">
<sup>*</sup> Note: Resting membrane potential is shown positive, but is in fact negative. This is because the radar plot struggles with negative values.</td></tr></tfoot>
</table>

# How to interpret

As I mentioned above, each radar plot represents a single cell type. Here, color serves no purpose other to indicate that each plot is a unique cell type. Each of the five axes represents a different measurement and has a different scale. The minimum is 0 in all cases. The maximum is indicated y the number at the apex of the axis. The lines indicate the percent of maximum from 0% to 100%, with each segment representing 25% of the maximum. This is indicated on the vertical axis, the only one with labels at each segment. It's easy to see, for example, that cerebellar Purkinje cells have significantly higher cell capacitance and rheobase compared to other cell types. We can also see that all of this cell types have similar restine membrane potentials.

# Presentation Tips

While `fmsb` will allow you to create as many axes as you want, I wouldn't do more than 6. Beyond 6 things start to get difficult to interpret. Additionally, it is easiest to understand radar plots when each of the axes have the same scale (which mine do not). If the axes do have different scales, try to be as explicit as possible with labels. Unfortunately only the "center" axis (the vertical one) can show labels at each segment break. While the `fmsb` package contains many variables that can be modified to customize the plot, I found that it lacks in specifying text. For example, there is no way to adjust the text justification (left, center, right) of only one text element, you can only do it for all or none. You also cannot manually move text labels; this results in a lot of overlapping text, requiring you to get creative.

# Variations and Alternatives

There is a lot of hate for radar plots in the data viz world. In most cases, a series of bar graphs or a parallel coordinate plot conveys the data in a more easily interpretable way, albeit not as cool looking.

# The Radar Plot

### The most basic radar plot

Before getting too fancy, what does `fmsb` do with basically no embelishments? For this, we'll use just one cell type. I decided to use Neocortex pyramidal cell (layer 5/6) data for this example. The top two rows of the data frame represent the axis max and min values. The next row(s) are the actual data. It should look like this:


```
## # A tibble: 3 × 5
##     rin   rmp   cap  rheo apamp
##   <dbl> <dbl> <dbl> <dbl> <dbl>
## 1  222.  81.6 615.  1300   86.4
## 2    0    0     0      0    0  
## 3  162.  70.3  89.2  166.  79.3
```

Now, feed it into `fmsb::radarchart()`:


```r
par(mar=c(0,0,1.2,0)+0.1) # Set the margins
radarchart(cortexpyramidal, title="Neocortex pyramidal cell layer 5-6") # Make the default radar plot
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="480" style="display: block; margin: auto;" />


### The embelished radar plot


*  I used `par()` to customize the output. You can do a lot with `par`. Find helpful documentation [here](https://www.rdocumentation.org/packages/graphics/versions/3.6.2/topics/par).  
*  There's a lot you can do with `fmsb::radarchart()`! For full documentation, go [here](https://www.rdocumentation.org/packages/fmsb/versions/0.7.0/topics/radarchart).  



```r
# Define colors for border and shading
bordercol=colormap(colormap=colormaps$portland, nshades=6, alpha=1)
shadingcol=colormap(colormap=colormaps$portland, nshades=6, alpha=0.3)

# Set the titles
titles <- as.character(neuron.data$NeuronName[3:8])

# Split the graphic into 6 frames
par(mar=c(0.1,0.5,0.5,0.1)+0.1) # Define margins
par(mfrow=c(2,3)) # 2 rows, 3 column layout

# Loop through each subplot to build the 6 panels
neuron.data.input <- neuron.data %>% select(-NeuronName) # Get rid of the "NeuronName" column

for(i in 1:6){

  # Build the radarChart
  radarchart(neuron.data.input[c(1,2,i+2),], axistype=3, 
  
    # Build polygon
    pcol=bordercol[i] , pfcol=shadingcol[i] , plwd=2, plty=1 , 
  
    # Define grid properties
    cglcol="grey", cglty=1, axislabcol="black", 
    # The weird spaces below are because you cannot define text alignment for individual compondents of the graph and text was overlapping.
    paxislabels = c(NA,"\n-80mV        ","615pF","1300pA","\n        85mV"), 
    cglwd=0.8, 
    caxislabels = c("0%","25%","50%","75%","220MOhm"),
    
    # Add titles
    title=titles[i],
    
    # Customize labels
    vlabels=c(expression('R'['in']), expression("V"["m"]), "Capacitance", "Rheobase", "Spike\nAmp"),
    vlcex=1.2, palcex=0.9, calcex=0.9,
    )
}
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="768" style="display: block; margin: auto;" />