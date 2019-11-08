---
layout: post
title:  "Bad Smell Detection - effectiveness analysis"
date:   2019-10-25
desc: ""
keywords: "bad smells detection,effectiveness analysis,precision,recall,f1 score,f-measure,R"
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
> **Note:** To achieve 100% of precision, a model cannot produce *false positives* - i.e., all of the *positive predictions* must be **actually** *positive observations*.

Following that, we need to define the **recall function**, which tells us the proportion of *actual positives* that were correctly identified.

```R
# Recall: proportion of correctly identified "positive observations"
recall <- function(tp, fn){
  
  recall <- tp/(tp+fn)
  
  return(recall)
}
```
> **Note:** To achieve 100% of recall, a model cannot produce *false negatives* - i.e., all of *positive observations* must be **predicted** as *positives*.

The third function will be the **F-measure function**, which calculates the F1 Score of a model. The F1 Score is a way to measure the model's accuracy, beeing 1 the best and 0 the worst.

In comparison to the classic accuracy formula, the F1 Score is a better way to calculate the accuracy, since unbalanced data has a different impact on the result - forcing the model to seek a balance between Precision and Recall.

And finally, we'll define a function to summarize all of these measures (precision, recall, F1 score).

```R
measures <- function(test, pred){
  
  true_positive <- 0
  true_negative <- 0
  false_positive <- 0
  false_negative <- 0
  
  for(i in 1:length(pred)){
    if(test[i] == TRUE && pred[i] == TRUE){
      true_positive <- true_positive + 1
    }else if(test[i] == FALSE && pred[i] == FALSE){
      true_negative <- true_negative + 1
    }else if(test[i] == FALSE && pred[i] == TRUE){
      false_negative <- false_negative + 1
    }else if(test[i] == TRUE && pred[i] == FALSE){
      false_positive <- false_positive + 1
    }
  }
  
  measures <- c(precision(true_positive,false_positive), 
                recall(true_positive,false_negative), 
                f_measure(true_positive,false_positive,false_negative))
  
  return(measures)
}
```

