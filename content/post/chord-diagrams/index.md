---
title: 'Chord Diagrams: Religious Composition of Romantic Partners'
author: ''
date: '2020-05-28'
slug: chord-diagrams
categories: []
tags:
  - data viz
  - how to
subtitle: ''
summary: ''
authors: []
lastmod: '2020-05-28T09:31:03-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: yes
projects: []
---

<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/kePrint/kePrint.js"></script>
<link href="{{< blogdown/postref >}}index_files/lightable/lightable.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/htmlwidgets/htmlwidgets.js"></script>
<script src="{{< blogdown/postref >}}index_files/d3/d3.min.js"></script>
<script src="{{< blogdown/postref >}}index_files/d3-tip/index.js"></script>
<link href="{{< blogdown/postref >}}index_files/chorddiag/chorddiag.css" rel="stylesheet" />
<script src="{{< blogdown/postref >}}index_files/chorddiag/chorddiag.js"></script>

Also known as “radial network diagrams”, chord diagrams are useful for representing connections between groups (“nodes”). The nodes are circularly arranged and relationships are represented using “chords” connecting two nodes. The chords can carry directional relationships or non-directional relationships. This is a very visually pleasing way to represent relationships and is a powerful method of visualizing large datasets. There is, however, a steep learning curve when it comes to creating chord diagrams. Below I will outline two methods, first I will use the `circlize` package to create a static diagram and then I will use the `chorddiag` package to create an interactive diagram. For more information on how to build chord diagrams using `circlize`, see [Circular Visualization in R](https://jokergoo.github.io/circlize_book/book/) by Zuguang Gu (the creator of the `circlize` package). The documentation for `circlize` can be found [here](https://github.com/jokergoo/circlize). There is some useful information on the [R Graph Gallery chord diagram page](https://www.r-graph-gallery.com/chord-diagram) (more [here](https://www.r-graph-gallery.com/123-circular-plot-circlize-package-2.html) and [here](https://www.r-graph-gallery.com/122-a-circular-plot-with-the-circlize-package.html)). For more information on the `chorddiag` package, you can find the documentation [here](https://github.com/mattflor/chorddiag). For an explanation of all of the variables you can customize, see [additional documentation here](https://github.com/mattflor/chorddiag/blob/master/R/chorddiag.R).

<img src="featured.png" style="width:80.0%" />

# The Data: How Couples Meet and Stay Together

The datset is from Stanford’s [How Couples Meet and Stay Together](https://data.stanford.edu/hcmst) research project. The dataset contains responses from 4,000 individuals and describes the relationships in their lives. The researchers then followed up on the respondents over several years to track their relationships over time. There are 300+ variables in this dataset. Here I focused on the religious identities of the couples surveyed.

### A glimpse of the data: What are the religious compositions of partners?

<table class="table table-condensed" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
Respondent
</th>
<th style="text-align:left;">
Partner
</th>
<th style="text-align:right;">
Count
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:left;">
Baptist
</td>
<td style="text-align:right;">
224
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:left;">
Buddhist
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:left;">
Catholic
</td>
<td style="text-align:right;">
112
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:left;">
Jewish
</td>
<td style="text-align:right;">
2
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:left;">
Mormon
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:left;">
Muslim
</td>
<td style="text-align:right;">
3
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:left;">
None
</td>
<td style="text-align:right;">
72
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:left;">
Other
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:left;">
Other Christian
</td>
<td style="text-align:right;">
67
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:left;">
Pentecostal
</td>
<td style="text-align:right;">
26
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:left;">
Protestant
</td>
<td style="text-align:right;">
147
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Buddhist
</td>
<td style="text-align:left;">
Catholic
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Buddhist
</td>
<td style="text-align:left;">
None
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Buddhist
</td>
<td style="text-align:left;">
Other
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Buddhist
</td>
<td style="text-align:left;">
Other Christian
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Buddhist
</td>
<td style="text-align:left;">
Protestant
</td>
<td style="text-align:right;">
4
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Catholic
</td>
<td style="text-align:left;">
Catholic
</td>
<td style="text-align:right;">
415
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Catholic
</td>
<td style="text-align:left;">
Eastern Orthodox
</td>
<td style="text-align:right;">
5
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Catholic
</td>
<td style="text-align:left;">
Hindu
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Catholic
</td>
<td style="text-align:left;">
Jewish
</td>
<td style="text-align:right;">
40
</td>
</tr>
</tbody>
</table>

# Representation Description

One of the variables in the dataset is the religion of the responent and their partner when they were 16. Each node in my chord diagram represents a religion. Chords connecting the nodes illustrate the religion of the respondent on one end of the chord and their partner on the other end of the chord. In many cases the respondent had the same religious identity as their partner, in which case the chord loops back on itself and looks more like a bump. The thickness of the chord corresponds to the number of partners of that particular religous identity composition. Each node is assigned a color to aid in interpretation. The color of a chord corresponds to the node from which the chord stems from. Transparency was used to aid in the perception of overlapping chords.

# How to interpret

Chord diagrams are best used to communicate broad concepts rather than specifics. From this representation it becomes immediately apparent that a very common religious pairing occurs between Catholics and Protestants. It is also common for partners to share the same religious identity. A side effect of this representation is that we can learn something about the religious composition of the participants that were surveyed; most respondents were Protestant, Baptist, or Catholic.

# Presentation Tips

Many aspects of the diagram can be customized if you’re using the `circlize` package. The package creator has an entire book on the topic available online: [Circular Visualization in R](https://jokergoo.github.io/circlize_book/book/). You can indicate directionality of the chords using arrowheads, which I disabled in my diagrams. You can also change the appearance of within-node chords such that they look like little bumps instead of a chord turned in on itself, which I find easier to understand. Depending on the level of detail that you are trying to convey with your diagram, you might opt to eliminate smaller chords by setting a threshold for chord thickness.

# Variations and Alternatives

Another way to represent relationships between groups is through a Sankey or Alluvial diagram. These are very similar to chord diagrams except that the relationship is conveyed through a line connecting two columns and can thus show a larger number of relationships due to the linear layout. Alternatively, network diagrams and arc diagrams can also be useful for communicating relationships.

## The Chord Diagrams

### Static Chord Diagram Using `circlize`

``` r
# Define colors
gridcolors <- c(Baptist = "#F8766D",
                Buddhist = "#E18A00",
                Catholic = "#BE9C00",
                `Eastern Orthodox` = "#8CAB00",
                Hindu = "#24B700",
                Jewish = "#00BE70",
                Mormon = "#00C1AB",
                Muslim = "#00BBDA",
                None = "#00ACFC",
                Other = "#8B93FF",
                `Other Christian` = "#D575FE",
                Pentecostal = "#F962DD",
                Protestant = "#FF65AC")

chordcolors <- religion_16_summary %>% mutate(Color = case_when(Respondent == "Baptist" ~ "#F8766D",
                                                                Respondent == "Buddhist" ~ "#E18A00",
                                                                Respondent == "Catholic" ~ "#BE9C00",
                                                                Respondent == "Eastern Orthodox" ~ "#8CAB00",
                                                                Respondent == "Hindu" ~ "#24B700",
                                                                Respondent == "Jewish" ~ "#00BE70",
                                                                Respondent == "Mormon" ~ "#00C1AB",
                                                                Respondent == "Muslim" ~ "#00BBDA",
                                                                Respondent == "None" ~ "#00ACFC",
                                                                Respondent == "Other" ~ "#8B93FF",
                                                                Respondent == "Other Christian" ~ "#D575FE",
                                                                Respondent == "Pentecostal" ~ "#F962DD",
                                                                Respondent == "Protestant" ~ "#FF65AC"))
chordcolors <- chordcolors$Color %>% unlist()


# Create the chord diagram
circos.clear()
par(mar = c(0, 0, 0.5, 0)) # left, right, top, bottom: add margin around circle
circos.par(cell.padding = c(0, 0, 0, 0),
           gap.degree = 1,
           canvas.ylim = c(-0.6, 0.8), # change the y limits of the canvas
           canvas.xlim = c(-1.1, 1.1)) # change the x limits of the canvas
chordDiagram(religion_16_summary,
             transparency = 0.5, 
             grid.col = gridcolors, 
             link.lwd = 0.5, # border line width
             link.lty = 1, #  border line type
             link.border = chordcolors, # border line color
             link.sort = TRUE, link.decreasing = TRUE, # Control the positioning of the sector links to minimize crossings
             self.link = 1, # Make self-links humps, not chords ( = 2 for chords)
             annotationTrack = "grid", # We'll plot the labels later
             annotationTrackHeight = 0.02, # Height for the annotation "grid"
             preAllocateTracks = 1, # Pre allocate a track and later the sector labels will be added
             directional = FALSE, # There is no directionality to the connections
             order = religion_16_summary$Respondent) # Order according the the respondent first, partner second

# Since each respondent is a sector, we need to use `draw.sector` to add annotation grids for regions which go across several religions
first = tapply(religion_16_summary$Respondent, religion_16_summary$Partner, function(x) x[1])
last = tapply(religion_16_summary$Respondent, religion_16_summary$Partner, function(x) x[length(x)])
for(i in seq_along(first)) {
    start.degree = get.cell.meta.data("cell.start.degree", sector.index = first[i], track.index = 1)
    end.degree = get.cell.meta.data("cell.end.degree", sector.index = last[i], track.index = 1)
    rou1 = get.cell.meta.data("cell.bottom.radius", sector.index = first[i], track.index = 1)
    rou2 = get.cell.meta.data("cell.top.radius", sector.index = last[i], track.index = 1)
    draw.sector(start.degree, end.degree, rou1, rou2, border = NA, col = "white")
}

# Since default text facing in `chordDiagram` is fixed, we need to manually add text in track 1
for(si in get.all.sector.index()) {
    xlim = get.cell.meta.data("xlim", sector.index = si, track.index = 1)
    ylim = get.cell.meta.data("ylim", sector.index = si, track.index = 1)
    circos.text(mean(xlim), ylim[1], si, facing = "clockwise", adj = c(0, 0.5),
    niceFacing = TRUE, cex = 1.25, col = "black", sector.index = si, track.index = 1)
}
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="768" />

### Interactive Chord Diagram Using `chorddiag`

For `chorddiag` the data needs to be organized as a matrix, but the subsequent coding for the diagram is much simpler.

<table class="table table-condensed" style="margin-left: auto; margin-right: auto;">
<thead>
<tr>
<th style="text-align:left;">
</th>
<th style="text-align:right;">
Baptist
</th>
<th style="text-align:right;">
Buddhist
</th>
<th style="text-align:right;">
Catholic
</th>
<th style="text-align:right;">
Eastern Orthodox
</th>
<th style="text-align:right;">
Hindu
</th>
<th style="text-align:right;">
Jewish
</th>
<th style="text-align:right;">
Mormon
</th>
<th style="text-align:right;">
Muslim
</th>
<th style="text-align:right;">
None
</th>
<th style="text-align:right;">
Other
</th>
<th style="text-align:right;">
Other Christian
</th>
<th style="text-align:right;">
Pentecostal
</th>
<th style="text-align:right;">
Protestant
</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;font-weight: bold;">
Baptist
</td>
<td style="text-align:right;">
224
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
56
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
33
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
38
</td>
<td style="text-align:right;">
14
</td>
<td style="text-align:right;">
77
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Buddhist
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Catholic
</td>
<td style="text-align:right;">
56
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
415
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
19
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
81
</td>
<td style="text-align:right;">
13
</td>
<td style="text-align:right;">
63
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
172
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Eastern Orthodox
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Hindu
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Jewish
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
21
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
32
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
13
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Mormon
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
6
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
38
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
9
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Muslim
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
None
</td>
<td style="text-align:right;">
39
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
92
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
134
</td>
<td style="text-align:right;">
20
</td>
<td style="text-align:right;">
50
</td>
<td style="text-align:right;">
10
</td>
<td style="text-align:right;">
75
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Other
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
7
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Other Christian
</td>
<td style="text-align:right;">
29
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
52
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
2
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
42
</td>
<td style="text-align:right;">
8
</td>
<td style="text-align:right;">
106
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
47
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Pentecostal
</td>
<td style="text-align:right;">
12
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
1
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
7
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
17
</td>
<td style="text-align:right;">
8
</td>
</tr>
<tr>
<td style="text-align:left;font-weight: bold;">
Protestant
</td>
<td style="text-align:right;">
70
</td>
<td style="text-align:right;">
3
</td>
<td style="text-align:right;">
166
</td>
<td style="text-align:right;">
4
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
15
</td>
<td style="text-align:right;">
5
</td>
<td style="text-align:right;">
0
</td>
<td style="text-align:right;">
46
</td>
<td style="text-align:right;">
9
</td>
<td style="text-align:right;">
39
</td>
<td style="text-align:right;">
14
</td>
<td style="text-align:right;">
319
</td>
</tr>
</tbody>
</table>

``` r
# Define colors
diagcolors <- c("#F8766D", "#E18A00", "#BE9C00", "#8CAB00", "#24B700", "#00BE70", "#00C1AB", "#00BBDA", "#00ACFC", "#8B93FF", "#D575FE", "#F962DD", "#FF65AC")

# Create the chord diagram
chorddiag(religionmatrix, 
          type = "directional",
          groupColors = diagcolors, 
          groupnamePadding = 5, groupnameFontsize = 18,
          chordedgeColor = "",
          showTicks = FALSE,
          showZeroTooltips = FALSE)
```

<div class="chorddiag html-widget html-fill-item-overflow-hidden html-fill-item" id="htmlwidget-1" style="width:864px;height:768px;"></div>
<script type="application/json" data-for="htmlwidget-1">{"x":{"matrix":[[224,0,56,0,0,0,3,1,33,5,38,14,77],[1,0,2,0,0,0,0,0,3,1,1,0,1],[56,3,415,2,1,19,9,0,81,13,63,8,172],[0,0,3,3,1,1,0,0,0,1,2,0,0],[0,0,0,0,5,0,0,1,0,0,0,0,1],[2,0,21,0,2,32,1,0,9,2,1,0,13],[4,0,6,0,0,0,38,0,4,0,3,0,9],[2,0,1,0,0,0,0,3,3,0,1,1,1],[39,4,92,3,0,5,3,0,134,20,50,10,75],[4,0,7,0,0,1,1,0,8,3,2,1,7],[29,0,52,1,1,4,2,1,42,8,106,9,47],[12,0,3,0,1,0,0,0,7,0,9,17,8],[70,3,166,4,0,15,5,0,46,9,39,14,319]],"options":{"type":"directional","width":null,"height":null,"margin":100,"showGroupnames":true,"groupNames":["Baptist","Buddhist","Catholic","Eastern Orthodox","Hindu","Jewish","Mormon","Muslim","None","Other","Other Christian","Pentecostal","Protestant"],"groupColors":["#F8766D","#E18A00","#BE9C00","#8CAB00","#24B700","#00BE70","#00C1AB","#00BBDA","#00ACFC","#8B93FF","#D575FE","#F962DD","#FF65AC"],"groupThickness":0.1,"groupPadding":0.0349065850398866,"groupnamePadding":[5,5,5,5,5,5,5,5,5,5,5,5,5],"groupnameFontsize":18,"groupedgeColor":null,"chordedgeColor":"","categoryNames":null,"categorynamePadding":100,"categorynameFontsize":28,"showTicks":false,"tickInterval":10,"ticklabelFontsize":10,"fadeLevel":0.1,"showTooltips":true,"showZeroTooltips":false,"tooltipNames":["Baptist","Buddhist","Catholic","Eastern Orthodox","Hindu","Jewish","Mormon","Muslim","None","Other","Other Christian","Pentecostal","Protestant"],"tooltipFontsize":12,"tooltipUnit":"","tooltipGroupConnector":" &#x25B6; ","precision":"null","clickAction":null,"clickGroupAction":null}},"evals":[],"jsHooks":[]}</script>
