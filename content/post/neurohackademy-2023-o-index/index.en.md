---
title: 'Neurohackademy 2023: Calculating an Author''s O-Index'
author: ''
date: '2023-08-19'
slug: neurohackademy-2023-o-index
categories: []
tags:
  - interactive
subtitle: ''
summary: ''
authors: []
lastmod: '2023-08-19T15:41:48-07:00'
featured: no
draft: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

[Neurohackademy](https://neurohackademy.org/) is an annual two-week long course offered at University of Washington in Seattle. The course covers some coding basics (python) and applications to neuroimaging. I attended the course in 2023 hoping to get some more neuroscience-centered coding experinece. The last few days of the course are a hackathon wherin groups of participants work on a de novo project together. My group consisted of four participants: Emily Lecy, Emily Pickup, Tania Miramontes, and myself. The goal of our project was to create an automated interface where you could type an author name, and be given an "openness-index", or "o-index". An o-index is a comprehensive rating of how open a scientist is with their publications. This index is created by weighing openness of both data, code, and manuscript availability.
This project was initially inspired by a past project, the [O-factor](https://github.com/srcole/o-factor). which gives journals an openness factor, in attempt to inspire open access initiatives.

Visit [our github](https://github.com/jnjahncke/o-index) for source code and an in-depth description of how an o-index is calculated. In short, given an author's name, our code performs a PubMed search for publications and, for any paper that is open access on the PMC database, scrapes the manuscript content for key terms to determine which components of the published body of work is accessible to the public.

As part of our project, we created a simple web app using Shiny for Python. (You can also access the app [here](https://jennifer-jahncke.shinyapps.io/o-index/).)

<iframe height="1500" width="100%" frameborder="no" src="https://jennifer-jahncke.shinyapps.io/o-index/"> </iframe>

I continue to find Shiny for Python apps easy to write and difficult to deploy. When we created this app, Shiny for Python was still in beta testing days so I remain hopeful that deployment will get easier in time.

There are a lot of caveats to the way our code calculates the o-index, many of which are outlined on [our github](https://github.com/jnjahncke/o-index) readme. For one, the PMC web scraping stage relies on my personal PubMed API key which is not ideal. If I had more time I might attempt to play around with this project some more but for now, it remains untouched since deployment during Neurohackademy 2023.