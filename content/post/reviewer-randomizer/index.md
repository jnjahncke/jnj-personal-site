---
title: 'Application Season: Randomly Assigning Reviewers'
author: ''
date: '2022-11-01'
slug: reviewer-randomizer
categories: []
tags: []
subtitle: ''
summary: ''
authors: []
lastmod: '2022-11-01T12:16:25-07:00'
featured: no
draft: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

My PI recently proposed a little side project for me: could I use my recently refreshed python skills to write a little code that would take in a list of applicants and a list of reviewers and randomly assign 3 reviewers to each applicant? He had been doing this manually and figured I would be able to automate it. This sounded like a simple project but I knew I would also need to figure out how he could run it using a web interface since he has never coded and I didn't want him to have to install python and run code from the command line. I went with a jupyter notebook that can be freely edited using Google Colab. Let's get into it:

This is supposed to be quick and easy. Not fancy. So I'm hard coding applicants and reviewers as well as the number of reviewers to be assigned to each applicant. The idea is that my PI can just copy-paste names from an excel file into the jupyter notebook and run the code.

First, we load packages:


```python
from random import randrange # for generating random assignments
from math import ceil # for calculating the number of applicants/reviewer
from google.colab import files # to enable file downloads so we can export .csv files
```




Next, input applicants, reviewers, and number of eyes we want on each application:


```python
# Number of people assigned to each applicant.
eyes = 3

# Input list of reviewers. Need 3 quotation marks at beginning and end of list only. Each reviewer should be on a new line.
reviewers = """Gary
Tianyi
Kevin
Larry
Henrique
Eric
Kelly
Haining
John"""
reviewers = reviewers.replace("\t", " ").split("\n")

# Input list of applicants. Need 3 quotation marks at beginning and end of list only. Each reviewer should be on a new line.
applicants = """A	B
C	D
E	F
G	H
I	J
K	L
M	N
O	P
Q	R
S	T
U	V
W	X
Y	Z"""
applicants = applicants.replace("\t", " ").split("\n")
```

Now we can run through the script. Output tables will be printed to standard out and written to a .csv file that will download from the browser.


```python
reviewer_num = len(reviewers) # number of reviewers
applicant_num = len(applicants) # number of applicants
max_num = ceil(applicant_num * eyes / reviewer_num) # the maximum number of applicants a reviewer should have to review

# build up empty lists/dictionaries to be fleshed out in the for loop
reviewer_counts = {} # keep track of how many students each reviewer is assigned
reviewer_dict = {} # keep a list of each applicant assigned to each reviewer
applicant_dict = {} # keep a list of each reviewer assigned to each applicant
for reviewer in reviewers: # build reviewer dictionaries
    reviewer_counts[reviewer] = 0
    reviewer_dict[reviewer] = []

# save table of applicant: reviewers
with open("applicants_reviewers.csv","w") as output:
  output.write("Applicant,Reviewers" + ","*(eyes-1) + "\n") # column headers
  print("Applicant\tReviewers")
  for applicant in applicants: # loop through applicants
      applicant_dict[applicant] = [] # make a dictionary entry for the applicant
      rev_list = [] # assign a temporary "reviewer list" variable
      line = applicant # assign a temporary string variable to be written at the end of the loop
      for x in range(eyes):
          temp = reviewers[randrange(reviewer_num)] # pull a random reviewer
          # if the reviewer is already in the reviewer list
          # or if the reviewer is already reviewing the max number of applicants
          # try a new reviewer
          while temp in rev_list or reviewer_counts[temp] >= max_num:
              temp = reviewers[randrange(reviewer_num)]
          # once we're happy with the list:
          rev_list.append(temp) # add the reviewer to the temporary reviewer list
          reviewer_counts[temp] += 1 # increase the reviewer's work load by 1
          reviewer_dict[temp].append(applicant) # add the applicant to the reviewer's dictionary entry
          applicant_dict[applicant].append(temp) # add the reviewer to the applicant's dictionary entry
          line += "," + temp # add the reviewer to the output line to be printed/written
      output.write(line + "\n")
      print(line.replace(",","\t"))
files.download("applicants_reviewers.csv")
```


```
## Applicant	Reviewers
```

```
## A B	Gary	Kelly	Eric
## C D	Tianyi	Haining	Gary
## E F	Kevin	Kelly	Gary
## G H	Haining	Kevin	John
## I J	Larry	Gary	Tianyi
## K L	Henrique	John	Kelly
## M N	Tianyi	Larry	Henrique
## O P	John	Kelly	Tianyi
## Q R	Larry	John	Eric
## S T	Tianyi	John	Henrique
## U V	Gary	Kelly	Haining
## W X	Kevin	Eric	Henrique
## Y Z	Kevin	Larry	Haining
```

Check the distribution of labor - how many applicants is each reviewer reviewing?:


```python
for reviewer, count in reviewer_counts.items():
    print(f"{reviewer}\t{count}")
```

```
## Gary	5
## Tianyi	5
## Kevin	4
## Larry	4
## Henrique	4
## Eric	3
## Kelly	5
## Haining	4
## John	5
```

Finally, maybe we want the reviewer-applicant table in a different format:


```python
# save table of reviewer: applicants
with open("reviewer_applicants.csv","w") as output:
  output.write("Reviewer,Applicants" + ","*(max_num-1) + "\n")
  print("Reviewer\tApplicants")
  for reviewer in reviewer_dict:
      line = reviewer
      for applicant in reviewer_dict[reviewer]:
        line += "," + applicant
      while line.count(",") < max_num:
        line += ","
      output.write(line + "\n")
      print(line.replace(",","\t"))
files.download("reviewer_applicants.csv")
```


```
## Reviewer	Applicants
```

```
## Gary	A B	C D	E F	I J	U V
## Tianyi	C D	I J	M N	O P	S T
## Kevin	E F	G H	W X	Y Z	
## Larry	I J	M N	Q R	Y Z	
## Henrique	K L	M N	S T	W X	
## Eric	A B	Q R	W X		
## Kelly	A B	E F	K L	O P	U V
## Haining	C D	G H	U V	Y Z	
## John	G H	K L	O P	Q R	S T
```

When you run the code from the jupyter notebook your browser will prompt you to download the two output files. The downside of using an editable notebook is that if the user doesn't know what they're doing and breaks the notebook they are unlikely to be able to fix it. To help with this, everything is [hosted on my github](https://github.com/jnjahncke/reviewer_randomizer) - including a copy of the notebook that is not publicly editable. I'm sure tweaks will be made down the line but this first draft gets the job done!