---
layout: post
title:  "Border Crossing analysis"
date:   2019-11-06
desc: "Exploratory data analysis over a US incoming crossing border dataset"
keywords: "exploratory analysis,R,border crossing,Mexico to USA,Canada to USA"
categories: [R]
tags: [Exploratory Analysis,R]
icon: icon-html
---

First of all, I downloaded the following dataset: https://www.kaggle.com/akhilv11/border-crossing-entry-data, which provides summary statistics for inbound crossings at the U.S.-Canada and the U.S.-Mexico border at the port level.

```R
glimpse(dataset)
```
>
    Observations: 150
    Variables: 6
    $ Id            <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16...
    $ SepalLengthCm <dbl> 5.1, 4.9, 4.7, 4.6, 5.0, 5.4, 4.6, 5.0, 4.4, 4.9, 5.4...
    $ SepalWidthCm  <dbl> 3.5, 3.0, 3.2, 3.1, 3.6, 3.9, 3.4, 3.4, 2.9, 3.1, 3.7...
    $ PetalLengthCm <dbl> 1.4, 1.4, 1.3, 1.5, 1.4, 1.7, 1.4, 1.5, 1.4, 1.5, 1.5...
    $ PetalWidthCm  <dbl> 0.2, 0.2, 0.2, 0.2, 0.2, 0.4, 0.3, 0.2, 0.2, 0.1, 0.2...
    $ Species       <fct> Iris-setosa, Iris-setosa, Iris-setosa, Iris-setosa, I...
