---
layout: post
title:  "Effectiveness analysis with R"
date:   2019-11-01
desc: ""
keywords: "effectiveness analysis,precision,recall,f1 score,f-measure,R"
categories: [R]
tags: [Effectiveness Analysis,R]
icon: icon-clipboard
---

The most common question on the machine learning world is "what is the best model to use?", and the answer to that is another question: "what kind of problem are you trying to solve?".

What I mean by that, is that the model depends on the problem. Each model will have a different effectiveness on each problem. And because of that we need to know how to measure the model effectiveness, so we can properly tell if the model fits or not the problem.


## Confusion Matrix

![alt text](https://miro.medium.com/max/712/1*Z54JgbS4DUwWSknhDCvNTQ.png "Confusion Matrix")

The confusion matrix is a form of visualize the performance of an algorithm.

- **True Positives (TP)**: the amount of "hits" of the algorithm. The amount of times the algorithm correctly predicted positive values.
- **True Negatives (TN)**: the amount of times the algorithm correctly predicted negative values.
- **False Positives (FP)**: the amount of times the algorithm incorrectly predicted positive values - i.e., the model predicted *positive* when it should've predicted *negative*.
- **False Negatives (FN)**: the amount of times the algorithm incorrectly predicted negative values - i.e., the model predicted *negative* when it should've predicted *positive*


## Coding

Now we'll define some functions to help with the effectiveness measuring processes.

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
