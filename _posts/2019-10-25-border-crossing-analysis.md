---
layout: post
title:  "Border Crossing analysis"
date:   2019-10-25
desc: "Exploratory data analysis over a US incoming crossing border dataset"
keywords: "exploratory analysis,R,border crossing,Mexico to USA,Canada to USA"
categories: [R]
tags: [Exploratory Analysis,R]
icon: icon-html
---

> Note: if you want to execute the code as you follow this post, you can do so on this [Google Colab Notebook](https://colab.research.google.com/drive/1XIS1_3Vr6MOC1PXCVa9Bz9xbwO8e-uEb).

First of all, I chose the [Border Crossing dataset](https://www.kaggle.com/akhilv11/border-crossing-entry-data), which provides summary statistics for inbound crossings at the U.S.-Canada and the U.S.-Mexico border at the port level.

And to start off the coding, let's import some libraries.

```R
library(dplyr)
library(ggplot2)
library(tidyr)
```

After that, I imported the dataset (`Border_Crossing_Entry_Data`) from my Google Drive and stored it into the following variable:

```R
dataset = read.csv(sprintf("https://docs.google.com/uc?id=%s&export=download", "1Xmdgj4VA9VMtS_yn3jgYHjMcArI-T_Xr"))
```

Then I ran the following code to know more about the dataset structure:

```R
summary(dataset)
```
>
```
                  Port.Name               State          Port.Code   
 Eastport              :  5541   North Dakota: 57071   Min.   : 101  
 Buffalo-Niagara Falls :  3348   Washington  : 44677   1st Qu.:2304  
 Calais                :  3348   Montana     : 38154   Median :3013  
 Calexico East         :  3348   Maine       : 38136   Mean   :2456  
 Champlain-Rouses Point:  3348   Texas       : 35754   3rd Qu.:3402  
 El Paso               :  3348   Minnesota   : 23135   Max.   :4105  
 (Other)               :324452   (Other)     :109806                 
              Border                           Date       
 US-Canada Border:266187   05/01/2010 12:00:00 AM:  1356  
 US-Mexico Border: 80546   06/01/2010 12:00:00 AM:  1356  
                           07/01/2010 12:00:00 AM:  1356  
                           08/01/2010 12:00:00 AM:  1356  
                           09/01/2010 12:00:00 AM:  1356  
                           10/01/2010 12:00:00 AM:  1356  
                           (Other)               :338597  
                        Measure           Value        
 Personal Vehicles          : 30219   Min.   :      0  
 Personal Vehicle Passengers: 30196   1st Qu.:      0  
 Trucks                     : 29856   Median :     90  
 Truck Containers Empty     : 29757   Mean   :  28188  
 Truck Containers Full      : 29694   3rd Qu.:   2483  
 Buses                      : 28822   Max.   :4447374  
 (Other)                    :168189                    
                  Location     
 POINT (-83.04 42.32) :  3069  
 POINT (-102.55 49)   :  3060  
 POINT (-106.45 31.76):  3060  
 POINT (-110.94 31.33):  3060  
 POINT (-111.96 49)   :  3060  
 POINT (-115.39 32.67):  3060  
 (Other)              :328364  
 ```
 
The output above is kinda messy, right? So we can try to run some alternative commands: `str()` or `glimpse()`, which gives us the quantity and the types of the variables on the dataset.
 
```R
str(dataset)
```
>
```
'data.frame':	346733 obs. of  8 variables:
 $ Port.Name: Factor w/ 116 levels "Alcan","Alexandria Bay",..: 19 108 73 65 106 57 74 84 80 22 ...
 $ State    : Factor w/ 15 levels "Alaska","Arizona",..: 3 5 3 2 10 5 11 13 11 10 ...
 $ Port.Code: int  2507 108 2506 2604 715 109 3401 2309 3403 712 ...
 $ Border   : Factor w/ 2 levels "US-Canada Border",..: 2 1 2 2 1 1 1 2 1 1 ...
 $ Date     : Factor w/ 279 levels "01/01/1996 12:00:00 AM",..: 72 72 72 72 72 72 72 72 72 72 ...
 $ Measure  : Factor w/ 12 levels "Bus Passengers",..: 12 7 12 9 4 12 1 10 6 12 ...
 $ Value    : int  34447 428 81217 62 16377 179 1054 1808 6685 24759 ...
 $ Location : Factor w/ 224 levels "POINT (-100.05 49)",..: 75 142 88 54 162 143 198 205 17 163 ...
```

```R
glimpse(dataset)
```
>
```
Observations: 346,733
Variables: 8
$ Port.Name <fct> Calexico East, Van Buren, Otay Mesa, Nogales, Trout River, Madawaska, Pembina, Progreso, Port...
$ State     <fct> California, Maine, California, Arizona, New York, Maine, North Dakota, Texas, North Dakota, N...
$ Port.Code <int> 2507, 108, 2506, 2604, 715, 109, 3401, 2309, 3403, 712, 3317, 3404, 3430, 209, 3409, 3323, 20...
$ Border    <fct> US-Mexico Border, US-Canada Border, US-Mexico Border, US-Mexico Border, US-Canada Border, US-...
$ Date      <fct> 03/01/2019 12:00:00 AM, 03/01/2019 12:00:00 AM, 03/01/2019 12:00:00 AM, 03/01/2019 12:00:00 A...
$ Measure   <fct> Trucks, Rail Containers Full, Trucks, Trains, Personal Vehicle Passengers, Trucks, Bus Passen...
$ Value     <int> 34447, 428, 81217, 62, 16377, 179, 1054, 1808, 6685, 24759, 235, 584, 1612, 80, 509, 122, 648...
$ Location  <fct> POINT (-115.48433000000001 32.67524), POINT (-67.94271 47.16207), POINT (-117.05333 32.57333)...
```

You may be thinking: "ok, that's better. But... What does it tell us?"

Well, the outputs above tell us the types of the variables on the dataset, and looking back to the first output we can make some observations - e.g., we have data obtained from two borders ("US-Canada Border" and "US-Mexico Border"), at 15 different states, at 116 different ports, at 224 distinct geographical points, in 279 different days.

We can also look at how many observations were made on each border, using the following commands:

 ```R
table(dataset$Border)
```
>
```
US-Canada Border US-Mexico Border 
          266187            80546 
```

We have 266187 observations made on US-Canada and 80546 on US-Mexico border.

So... What about we take a closer look at the data from the Mexico border?

Let's do it!


 ```R
mexico_dataset = droplevels(dataset %>% filter(Border == "US-Mexico Border"))
str(mexico_dataset)
```
>
```
'data.frame':	80546 obs. of  8 variables:
 $ Port.Name: Factor w/ 27 levels "Andrade","Boquillas",..: 5 17 16 19 23 26 21 15 2 17 ...
 $ State    : Factor w/ 4 levels "Arizona","California",..: 2 2 1 4 2 2 4 1 4 2 ...
 $ Port.Code: int  2507 2506 2604 2309 2504 2505 2310 2603 2410 2506 ...
 $ Border   : Factor w/ 1 level "US-Mexico Border": 1 1 1 1 1 1 1 1 1 1 ...
 $ Date     : Factor w/ 279 levels "01/01/1996 12:00:00 AM",..: 72 72 72 72 72 72 72 72 72 72 ...
 $ Measure  : Factor w/ 12 levels "Bus Passengers",..: 12 12 9 10 1 10 10 10 3 5 ...
 $ Value    : int  34447 81217 62 1808 7779 1993 740 139 4362 573597 ...
 $ Location : Factor w/ 52 levels "POINT (-100.49917 28.70889)",..: 31 40 20 43 38 34 49 18 5 40 ...
 ```
 
 Here I used the `%>%` pipeline operator to pass the `dataset` data to the `filter()` function. Then I wrapped it all with the `droplevels()` function, to drop unused levels from the new dataset.
 
 And the final result tells us that the US-Mexico border only has 4 out of the 15 states, 27 ports, and 52 different locations.
