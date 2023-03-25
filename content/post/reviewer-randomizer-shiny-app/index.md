---
title: Reviewer Randomizer Shiny App
author: ''
date: '2022-12-04'
slug: reviewer-randomizer-shiny-app
categories: []
tags:
  - interactive
subtitle: ''
summary: ''
authors: []
lastmod: '2022-12-04T13:15:35-08:00'
featured: no
draft: no
image:
  caption: ''
  focal_point: 'Top'
  preview_only: yes
projects: []
---



Last month I wrote a simple python script to help my graduate program to randomly assign reviewers to applicants during application season. Since writing that script I have (1) updated the script to make sure each applicant has at most one trainee reviewer and make sure that the division of labor is distributed equally and (2) built a python shiny app that I've deployed on shinyapps.io to make running my script more user friendly. Below is my shiny app (also usable at https://jennifer-jahncke.shinyapps.io/reviewer_randomizer/). And below that is a writeup of how I underwent making the app.

<iframe height="1500" width="100%" frameborder="no" src="https://jennifer-jahncke.shinyapps.io/reviewer_randomizer/"> </iframe>

# The making of my shiny app

To view the final files, see my [github](https://github.com/jnjahncke/reviewer_randomizer/tree/main/rr_shiny_app). See sections below for a how-to.

## Updates to Reviewer Randomizer

First, load the packages we'll need:


```python
from random import randrange
from math import floor
import re
```

For the app the inputs are a list of reviewers, a list of applicants, and the number of reviewers to be assigned to each applicant ("eyes"). With the new rules I needed, the randomization will fail at times when it backs itself into a corner where it can no longer make reviewer assignment that adhere to the limitations that (1) the same reviewer isn't assigned to an appliant multiple times, (2) at most one trainee is assigned as a reviewer per applicant, and (3) the division of labor for reviewers is evenly distributed. Because of this, I wrote the randomizer as a function and included a break if it is stuck in a loop. Here is that function:


```python
def assign_reviewer(reviewers, applicants, eyes):
    
    # make it so that any form of white space is permitted
    # split at each newline
    reviewers = reviewers.replace("\t", " ").split("\n")
    applicants = applicants.replace("\t"," ").split("\n")
    
    # remove white space on either side of each entry
    reviewers = [x.rstrip().lstrip() for x in reviewers]
    applicants = [x.rstrip().lstrip() for x in applicants]

    ## ---------------------------- ##
    ## Calculate assignment lengths ##
    ## ---------------------------- ##
    reviewer_num = len(reviewers) # count number of reviewers
    applicant_num = len(applicants) # count number of applicants
    
    # what is the minimum number of applicants to be assigned to each reviewer?
    min_num = floor(applicant_num * eyes / reviewer_num)
    # and how many reviewers are going to be assigned one more than the min num?
    remainder = (applicant_num * eyes) % reviewer_num
    
    # how many reviewers will be assigned the max_num, how many min_num?
    assign_max = remainder
    assign_min = reviewer_num - remainder

    ## ---------------------------- ##
    ## Build up empty lists/dicts   ##
    ## ---------------------------- ##
    reviewer_counts = {} # keep track of how many students each reviewer is assigned
    reviewer_dict = {} # keep a list of each applicant assigned to each reviewer
    reviewer_list = [] # list of all reviewers
    faculty_list = [] # subset list of faculty reviewers
    trainee_list = [] # subset list of trainee (non-faculty) reviewers
    
    # parse reviewer names, roles; build up reviewer lists/dictionaries
    for reviewer in reviewers:
        # parse name, role
        for found in re.finditer(r"^([\S\s]+)\s(\S+)$",reviewer):
            # found.group(1) = reviewer name
            # found.group(2) = reviewer role
            reviewer_counts[found.group(1).rstrip()] = 0
            reviewer_dict[found.group(1).rstrip()] = []
            reviewer_list.append(found.group(1).rstrip())
            
            # assign to faculty_list or trainee_list
            if found.group(2).lower().lstrip().rstrip() == "faculty":
                faculty_list.append(found.group(1).rstrip())
            else:
                trainee_list.append(found.group(1).rstrip())

    # assign reviewer to either be assigned the max or min number of applicants
    # based on calculations above
    max_list = reviewer_list[:assign_max]
    min_list = reviewer_list[assign_max:]
    
    applicant_dict = {} # keep a list of each reviewer assigned to each applicant
    applicant_counts = {} # keep track of how many reviewers have been assigned
    for applicant in applicants:
        applicant_counts[applicant] = 0
        applicant_dict[applicant] = []

    ## ------------------------------ ##
    ## Assign applicants to reviewers ##
    ## ------------------------------ ##
    
    # trainees first
    i = 0 # use i to keep track of the number of attempts the randomizer makes
    for trainee in trainee_list:
        rev_list = []
        if trainee in min_list:
            assign_num = min_num
        elif trainee in max_list:
            assign_num = min_num + 1
        while len(reviewer_dict[trainee]) < assign_num: 
            i += 1
            temp = applicants[randrange(applicant_num)]
            while temp in rev_list or applicant_counts[temp] != 0:
                i += 1
                temp = applicants[randrange(applicant_num)]
                # if randomizer is stuck in a loop, exit the function
                if i > applicant_num * len(trainee_list) * eyes * 10:
                   return(False) 
            # once an applicant has the number of reviewers they need, remove them from the list of applicants
            if applicant_counts[temp] == eyes:
                applicants.remove(temp)
                applicant_num = len(applicants)
            rev_list.append(temp)
            applicant_counts[temp] += 1
            reviewer_counts[trainee] += 1
            applicant_dict[temp].append(trainee)
            reviewer_dict[trainee].append(temp)

    # faculty
    i = 0
    for faculty in faculty_list:
        rev_list = []
        if faculty in min_list:
            assign_num = min_num
        elif faculty in max_list:
            assign_num = min_num + 1
        while len(reviewer_dict[faculty]) < assign_num: 
            i += 1
            temp = applicants[randrange(applicant_num)]
            while temp in rev_list or applicant_counts[temp] == eyes:
                i += 1
                temp = applicants[randrange(applicant_num)]
                if i > applicant_num * len(reviewer_list) * eyes * 10:
                   return(False)
            if applicant_counts[temp] == eyes:
                applicants.remove(temp)
                applicant_num = len(applicants)
            rev_list.append(temp)
            applicant_counts[temp] += 1
            reviewer_counts[faculty] += 1
            applicant_dict[temp].append(faculty)
            reviewer_dict[faculty].append(temp)
    return(applicant_dict, reviewer_dict)
```

This function will return `False` if it gets stuck in a loop and will return `applicant_dict` and `reviewer_dict` if successful. Therefore, to run the function such that it'll be successful every time, I call:


```python
attempt = False
while attempt == False:
    attempt = assign_reviewer(reviewers, applicants, eyes)
applicant_dict, reviewer_dict = attempt
```

I can then format `applicant_dict` and `reviewer_dict` nicely as tables or .txt files. These two dictionaries contain the same information except `applicant_dict` is in the format `{applicant:[assigned reviewers]}` and `reviewer_dict` is in the format `{reviewer:[assigned applicants]}`

## Installing Shiny for Python

This was my first time using shiny for python so I had to install it and link it to my shinyapps.io account using `rsconnect`. Steps for installing shiny can be found on [the shiny for python docs page](https://shiny.rstudio.com/py/docs/get-started.html).

Install shiny a create an app from the command line:


```eval
pip install shiny
shiny create rr_shiny_app
```

This will make a directory called `rr_shiny_app` that contains a python file named `app.py`. If you navigate into that directory (`cd rr_shiny_app`) you can run the app locally using `shiny run --reload`.

More on linking to your shinyapps.io account once we get to the deployment step.

## Writing the Shiny App

This was my second time using shiny and my first time using shiny for python so I had some (re)learning to do. I found the [gallery of apps](https://shiny.rstudio.com/py/gallery/) hosted on shinylive helpful to get me going.

First, load the packages we'll need:


```python
from shiny import * 
from random import randrange
from math import floor
import pandas as pd
import re

# import the randomizer function I wrote in reviewer_randomizer.py, saved in the same directory
from reviewer_randomizer import *
```

And design the shiny app page layout, inputs, and outputs:


```python
app_ui = ui.page_fixed(
    # title
    ui.h2("Reviewer Randomizer"),
    
    # introductory paragraph
    ui.p("Enter a list of reviewers (and their role), a list of applicants, and the number of reviewers to be assigned to each applicant. Applicants will be randomly assigned such that they only have at maximum one non-faculty reviewer."),
    
    # layout: one row with three columns of widths 5, 5, and 2 (must add up to 12)
    ui.row(
      
        # column 1: entry form for reviewers
        ui.column(5,ui.input_text_area("reviewers","Reviewers:","""Kevin Wright\tFaculty
Kelly Monk\tFaculty
Jennifer Jahncke\tStudent""",rows=20)),

        # column 2: entry form for applicants
        ui.column(5, ui.input_text_area("applicants","Applicants:","""Beyonce Knowles
Taylor Swift
Ryan Reynolds""",rows=20)),
        
        # column 3: number of reviewers to be assigned to each applicant
        ui.column(2,ui.input_text_area("eyes","Number of Reviewers per Applicant:",3,rows=1))),
        
    # text to appear above the "go" button
    ui.p("FYI: This can take a few minutes."),
    
    # the "go" button
    ui.input_action_button("go", "Go!", class_="btn-success"),
    
    # define outputs: (1) applicants:reviewer table and (2) reviewer:applicants text block
    # these will be called as functions app_rev() and rev_app() below server()
    ui.output_table("app_rev"),
    ui.output_text_verbatim("rev_app")
)
```

Now, use the inputs to feed into the randomizer function and generate outputs:


```python
# define server() function
def server(input, output, session):
    
    # use reactive.Calc to manipulate inputs and store output,
    # here I use it to call the randomizer and define the applicant_dict and reviewer_dict dictionaries
    @reactive.Calc
    # make it so it doesn't run until you hit the go button
    @reactive.event(lambda: input.go(), ignore_none=False)
    def assignment():
        reviewers = input.reviewers()
        applicants = input.applicants()
        eyes = int(input.eyes())

        attempt = False
        while attempt == False:
            attempt = assign_reviewer(reviewers, applicants, eyes)
        applicant_dict, reviewer_dict = attempt
        return(applicant_dict, reviewer_dict)

    # define the first output: applicant:dict table
    @output
    @render.table
    def app_rev():
        
        applicant_dict = assignment()[0]
        eyes = int(input.eyes())

        students = list(applicant_dict.keys())

        # create list of table column names as Reviewer 1, Reviewer 2, etc.
        revs = ["Reviewer "] * eyes
        nums = [str(x) for x in range(1,eyes+1)]
        rev_names = [x+y for x,y in zip(revs,nums)]
        
        col_name_dict = {}
        for x in range(eyes):
            col_name_dict[x] = rev_names[x]

        # create table from the applicant_dict dictionary
        result = pd.DataFrame(applicant_dict.items(),
                columns = ["Applicant", "Reviewers"])
        # split column containing reviewer list into multiple columns
        result = pd.concat([result, pd.DataFrame(result.Reviewers.tolist())],
                axis = 1)
        # name said columns using names created above
        result = result.rename(col_name_dict, axis=1)
        # remove the column containing the reviewer list
        result = result.drop(labels="Reviewers", axis=1)

        return(result)
    
    @output
    @render.text
    def rev_app():

        reviewer_dict = assignment()[1]
        
        # iterate through reviewer_dict and print results to text block
        result = ""
        for reviewer in reviewer_dict:
            result += reviewer + "\n"
            for applicant in reviewer_dict[reviewer]:
                result += "\t" + applicant + "\n"

        return(result)
```

And finally, the most important part: run the app


```python
app = App(app_ui, server)
```

## Deploying the Shiny App

In the directory containing my `app.py` file I will need three files for my app:

1. `app.py` - the script containing my shiny app
2. `reviewer_randomizer.py` - the script containing my custom randomizer function
3. `requirements.txt` - a text file containing a list of python packages that shinyapps.io is going to have to import for my app to run.

The `requirements.txt` file took me a while to get the correct list of packages. I first only listed `jinja2`, which is required by pandas but not automatically installed. However when I deployed my app the logs told me it couldn't find `shiny` so I added `shiny` to the list. But *then* when I deployed my app it told me it couldn't find `pandas` so I then added `pandas` after `jinja2`. This wound up being the winning combination. So now my final `requirements.txt` document reads:


```python
shiny
jinja2
pandas
```

Okay but how do you actually go about deploying the app? First, you need to install `rsconnect`:


```python
pip install rsconnect-python
```

Next, you have to connect to your shinyapps.io account (I already had one, you may need to make one). And you'll need the information listed in your tokens. To find your token, click on your name in the top right of the webpage and click "tokens". Create a token for you machine I click "show". Go to the "with python" tab and make sure you hit "show secret" to show the secret part. Copy that text and run it on that command line. Mine looks like this


```python
rsconnect add \
	  --account jennifer-jahncke \
	  --name jennifer-jahncke \
	  --token stringofnumbersandletters \
	  --secret stringofnumbersandletters
```

And NOW we can deploy the app from the command line using `rsconnect deploy shiny /path/to/app_name --name [NAME] --title app_name`. For me, this looks like:


```python
rsconnect deploy shiny ./rr_shiny_app --name jennifer-jahncke --title reviewer_randomizer
```

Make sure the only things in your app folder are the files needed for the app. For me that's the three fileblogs listed above. Honestly this took me a long time of messing around to figure out. Don't get discouraged and good luck!
