---
layout: post
title:  "Bad Smell Detection - effectiveness analysis"
date:   2019-10-25
desc: ""
keywords: "bad smells detection,effectiveness analysis,R"
categories: [R]
tags: [Effectiveness Analysis,R]
icon: icon-html
---

First things first. We need to start off by importing some machine learning libraries:

```R
library(RWeka)
library(e1071)
library(gmodels)
library(caret)
library(irr)
library(randomForest)
```

Now we need to define some functions to help with the evaluation and comparison processes.

The first function will be the precision function, which tells us the proportion of "positive predictions" that were actually correct.
