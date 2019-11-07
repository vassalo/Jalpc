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

The first function will be the **precision function**, which tells us the proportion of *positive predictions* that were actually correct.

```R
# Precision: proportion of correct "positive predictions"
precision <- function(tp, fp){
  
  precision <- tp/(tp+fp)
  
  return(precision)
}
```
> To achieve 100% of precision, a model cannot produce *false positives* - i.e., all of the *positive predictions* must be **actually** *positive observations*.

Following that, we need to define the **recall function**, which tells us the proportion of *actual positives* that were correctly identified.

```R
# Recall: proportion of correctly identified "positive observations"
recall <- function(tp, fn){
  
  recall <- tp/(tp+fn)
  
  return(recall)
}
```
> To achieve 100% of recall, a model cannot produce *false negatives* - i.e., all of *positive observations* must be **predicted** as *positives*.
