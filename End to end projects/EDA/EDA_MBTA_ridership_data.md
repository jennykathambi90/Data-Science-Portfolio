---
title: " EDA of the MBTA ridership data"
author: "Jane Kathambi"
date: "8 June 2018"
output: 
  html_document:
    keep_md: yes
    theme: united
    toc: yes
    number_sections: true
    toc_depth: 4
    toc_float:
      collapsed: yes
      smooth_scroll: yes
---


# Introduction
The Massachusetts Bay Transportation Authority ("MBTA" or just "the T" for short) manages America's oldest subway, as well as Greater Boston's commuter rail, ferry, and bus systems.

Our goal during EDA is to develop an understanding of our data. The easiest way to do this is to use questions as tools to guide our investigation. When we ask a question, the question focuses our attention on a specific part of our dataset and helps us decide which graphs, models, or transformations to make.

Eda involves use of visualisation and transformation to explore data in a systematic way. It is an iterative cycle. We will:

1. Generate questions about our data.

2. Search for answers by visualising, transforming, and modelling our data.

3. Use what we learn to refine our questions and/or generate new questions.

There is no rule about which questions one should ask to guide research. However, two types of questions will always be useful for making discoveries within our data:

1. What type of variation occurs within our variables?

2. What type of covariation occurs between our variables?

We are going to apply exploratory data analysis on MBTA ridership data. We will use the following questions as our guidline:

1. **QUESTION:** Is out data clean?

    + Is the data tidy?
    + Are the variables of the correct type?
    + Are there any outliers and obvious errors?
2. **QUESTION:**Which is the most commonly used mode of transport?
3. **QUESTION:**Which is the least commonly used mode of transpot?

# Is our data clean?


## Is the data tidy?
Tidy data is data in which:
 
* Each variable forms a column.
* Each observation forms a row.
* Each type of observational unit forms a table.

Let us see if our data is tidy. We will begin by importing the excel data using the readxl package.

_**Import the data first**_


```r
#Load readxl package
library(readxl)

#Import the data. Skip the header row
mbta<-read_excel("data/mbta.xlsx", skip=1)
```

_**Check if the data is tidy**_


```r
# View the first 6 rows of mbta
head(mbta)
```

```
## # A tibble: 6 x 60
##    X__1 mode   `2007-01` `2007-02` `2007-03` `2007-04` `2007-05` `2007-06`
##   <dbl> <chr>  <chr>     <chr>         <dbl> <chr>     <chr>         <dbl>
## 1     1 All M~ NA        NA            1188. NA        NA           1246. 
## 2     2 Boat   4         3.6             40  4.3       4.9             5.8
## 3     3 Bus    335.819   338.675        340. 352.162   354.367       351. 
## 4     4 Commu~ 142.2     138.5          138. 139.5     139           143  
## 5     5 Heavy~ 435.294   448.271        459. 472.201   474.579       477. 
## 6     6 Light~ 227.231   240.262        241. 255.557   248.262       246. 
## # ... with 52 more variables: `2007-07` <chr>, `2007-08` <chr>,
## #   `2007-09` <dbl>, `2007-10` <chr>, `2007-11` <chr>, `2007-12` <dbl>,
## #   `2008-01` <chr>, `2008-02` <chr>, `2008-03` <dbl>, `2008-04` <chr>,
## #   `2008-05` <chr>, `2008-06` <dbl>, `2008-07` <chr>, `2008-08` <chr>,
## #   `2008-09` <dbl>, `2008-10` <chr>, `2008-11` <chr>, `2008-12` <dbl>,
## #   `2009-01` <chr>, `2009-02` <chr>, `2009-03` <dbl>, `2009-04` <chr>,
## #   `2009-05` <chr>, `2009-06` <dbl>, `2009-07` <chr>, `2009-08` <chr>,
## #   `2009-09` <dbl>, `2009-10` <chr>, `2009-11` <chr>, `2009-12` <dbl>,
## #   `2010-01` <chr>, `2010-02` <chr>, `2010-03` <dbl>, `2010-04` <chr>,
## #   `2010-05` <chr>, `2010-06` <dbl>, `2010-07` <chr>, `2010-08` <chr>,
## #   `2010-09` <dbl>, `2010-10` <chr>, `2010-11` <chr>, `2010-12` <dbl>,
## #   `2011-01` <chr>, `2011-02` <chr>, `2011-03` <dbl>, `2011-04` <chr>,
## #   `2011-05` <chr>, `2011-06` <dbl>, `2011-07` <chr>, `2011-08` <chr>,
## #   `2011-09` <dbl>, `2011-10` <chr>
```

```r
# View the entire data set or View(mbta)
View(mbta)
```

We can see that:

1. There is a unnecessary column. i.e the first column which lists the observations.
2. There are unnecessary rows:

    + All mode by Qtr row. This 1st row is a quarterly average of weekday MBTA ridership. Since this dataset tracks monthly average ridership, this row does not belong to this data frame. Furthermore, this explains why it has missing data.
    + Pct Chg / Yr row. This 7th row is not an observation but an analysis
    + TOTAL row. This 11th row is not an observation but an analysis. 

3. Observations(values) i.e months are stored as columns rather than as rows.

So the data is not tidy.

_**Let us tidy the data**_


```r
#Load tidyr
library(tidyr)

# Remove the unnecessary column
mbta1=mbta[,-1]
View(mbta1)

#remove unnecessary rows
mbta2=mbta1[-c(1,7,11), ]
View(mbta2)

#gather all columns except mode and store them as month, thou_riders columns
mbta3=mbta2%>%
  gather(month, thou_riders, -mode)
View(mbta3)
```

The data i.e mbta3 is now tidy. Let us move on to the next question.

## Are the variables of the correct type?

_**Let us check if the variables are of the correct type**_


```r
#load dplyr
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
glimpse(mbta3)
```

```
## Observations: 464
## Variables: 3
## $ mode        <chr> "Boat", "Bus", "Commuter Rail", "Heavy Rail", "Lig...
## $ month       <chr> "2007-01", "2007-01", "2007-01", "2007-01", "2007-...
## $ thou_riders <chr> "4", "335.819", "142.2", "435.294", "227.231", "4....
```

We can see that month and thou_riders are not of the correct type. Mode too as a categorical variable should bea factor.

_**Let us convert month and thou_riders to the correct type**_


```r
#Loading the zoo package to use on month
library(zoo)
```

```
## 
## Attaching package: 'zoo'
```

```
## The following objects are masked from 'package:base':
## 
##     as.Date, as.Date.numeric
```

```r
#Let us view the day format
head(mbta3$month)# It is in the format year-month i.e "2007-03"
```

```
## [1] "2007-01" "2007-01" "2007-01" "2007-01" "2007-01" "2007-01"
```

```r
#convert the month column into date
mbta3$month<-as.yearmon(mbta3$month,"%Y-%m") 

#convert thou_riders to numeric
mbta3$thou_riders<-as.numeric(mbta3$thou_riders) 

#convert mode to factor
mbta3$mode<-as.factor(mbta3$mode) 

#vewing the str of mbta3
glimpse(mbta3)
```

```
## Observations: 464
## Variables: 3
## $ mode        <fct> Boat, Bus, Commuter Rail, Heavy Rail, Light Rail, ...
## $ month       <S3: yearmon> Jan 2007, Jan 2007, Jan 2007, Jan 2007, Ja...
## $ thou_riders <dbl> 4.000, 335.819, 142.200, 435.294, 227.231, 4.772, ...
```

The variable types are now correct. 

4. **QUESTION:** What if we want to look at ridership during every January (for example), the month and year are together in the same column, which makes it a little tricky. Let us separate them.

_**Separating month into new columns month and year**_


```r
# Separating month into new columns year and month
mbta4=mbta3%>%
      separate(month, c("month", "year"))

#View mbta4
View(mbta4)
```

We are now going to separate the month column into distinct month and year columns to make life easier. We will use the tidyr separate columns function.

Viewing mbta4 shows that month has beed separated into new columns year and month. But month comes before year which is not usually the norm, let us reorder the columns so that year comes before month. Mode should come after month and thou_riders after mode.

_**Reordering Columns: year should come before month**_


```r
#Reordering Columns: year should come before month
mbta5=mbta4[, c(3,2,1,4)]

#view mbta5
View(mbta5)
```

Our data frame, mbta5, is now in the correct ordering of columns, i.e year, month, mode, thou_riders.


## Are there any outliers and obvious errors?
Before you write up any analysis, it's a good idea to screen the data for any obvious mistakes and/or outliers.

There are many valid exploratory techniques for doing this, which include;

    + Summary
    + Histogram
    + Box plot
    + Scatter plot

We will use summary and boxplot.

_**checking for outliers: Summary**_


```r
#use summary
summary(mbta5)
```

```
##      year              month                      mode    
##  Length:464         Length:464         Boat         : 58  
##  Class :character   Class :character   Bus          : 58  
##  Mode  :character   Mode  :character   Commuter Rail: 58  
##                                        Heavy Rail   : 58  
##                                        Light Rail   : 58  
##                                        Private Bus  : 58  
##                                        (Other)      :116  
##   thou_riders     
##  Min.   :  2.213  
##  1st Qu.:  5.695  
##  Median : 80.711  
##  Mean   :155.672  
##  3rd Qu.:281.533  
##  Max.   :554.932  
## 
```


_**checking for outliers: Boxplot**_


```r
# Load library ggplot2
library(ggplot2)

#plot a box plot of thou_riders and mode
mbta5%>%
  ggplot(aes(x=mode, y=thou_riders) )+
  geom_boxplot()
```

![](EDA_MBTA_ridership_data_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

We see that boat and trackless trolley have missing values.
Let us draw a zoomed in box plot of boat and trackless trolley.


```r
mbta5%>%
  filter(mode%in%c("Boat", "Trackless Trolley"))%>%
  ggplot(aes(x=mode, y=thou_riders) )+
  geom_boxplot()
```

![](EDA_MBTA_ridership_data_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

Let us look at the summaries of the two modesof transport


```r
#Summary of boat
mbta5%>%
  filter(mode=="Boat")%>%
  summarize(min(thou_riders), median(thou_riders), mean(thou_riders), max(thou_riders))
```

```
## # A tibble: 1 x 4
##   `min(thou_riders)` `median(thou_ride~ `mean(thou_rider~ `max(thou_rider~
##                <dbl>              <dbl>             <dbl>            <dbl>
## 1               2.98               4.29              5.07               40
```

```r
#summary of Trackless trolley
mbta5%>%
  filter(mode=="Trackless Trolley")%>%
  summarize(min(thou_riders), median(thou_riders), mean(thou_riders), max(thou_riders))
```

```
## # A tibble: 1 x 4
##   `min(thou_riders)` `median(thou_ride~ `mean(thou_rider~ `max(thou_rider~
##                <dbl>              <dbl>             <dbl>            <dbl>
## 1               5.78               12.6              12.1             15.1
```

Evaluating the statistics for the Boat mode, it seems the 40 value is an obvious error, could be a typo error where a zero was added after 4. So we will replace 40 with 4.

The outliers for the trackless trolley, are overwrapping with the values of boat so dealing with them is not intuitive so we are going to spread the mode column so that we can deal with the outliers easily.

_**Spread the mode column over the thou_riders column**_


```r
#Spread the mode column over the thou_riders column
mbta6=mbta5%>%
      spread(mode, thou_riders)

#view the first 6 rows of mbta6
head(mbta6)
```

```
## # A tibble: 6 x 10
##   year  month  Boat   Bus `Commuter Rail` `Heavy Rail` `Light Rail`
##   <chr> <chr> <dbl> <dbl>           <dbl>        <dbl>        <dbl>
## 1 2007  Apr    4.3   352.            140.         472.         256.
## 2 2007  Aug    6.57  355.            142.         462.         235.
## 3 2007  Dec    2.98  313.            142.         448.         233.
## 4 2007  Feb    3.6   339.            138.         448.         240.
## 5 2007  Jan    4     336.            142.         435.         227.
## 6 2007  Jul    6.52  358.            142.         472.         243.
## # ... with 3 more variables: `Private Bus` <dbl>, RIDE <dbl>, `Trackless
## #   Trolley` <dbl>
```

```r
#view internal structure
glimpse(mbta6)
```

```
## Observations: 58
## Variables: 10
## $ year                <chr> "2007", "2007", "2007", "2007", "2007", "2...
## $ month               <chr> "Apr", "Aug", "Dec", "Feb", "Jan", "Jul", ...
## $ Boat                <dbl> 4.300, 6.572, 2.985, 3.600, 4.000, 6.521, ...
## $ Bus                 <dbl> 352.162, 355.479, 312.920, 338.675, 335.81...
## $ `Commuter Rail`     <dbl> 139.500, 142.364, 141.585, 138.500, 142.20...
## $ `Heavy Rail`        <dbl> 472.201, 461.605, 448.268, 448.271, 435.29...
## $ `Light Rail`        <dbl> 255.557, 234.907, 233.379, 240.262, 227.23...
## $ `Private Bus`       <dbl> 4.542, 3.946, 3.708, 4.417, 4.772, 3.936, ...
## $ RIDE                <dbl> 5.400, 5.308, 5.062, 5.000, 4.900, 5.253, ...
## $ `Trackless Trolley` <dbl> 13.444, 13.142, 12.410, 12.913, 12.757, 13...
```

_**Dealing with the Boat outlier**_
As seen earlier on the boat outlier is a typo error where a zero was added to 4, solet us replace 40 with 4. We cab verify this by running a summary of the Boat variable which will give us the distribution of the Boat variable.The max value which is 40, is very far from the median and the mean.


```r
#summary of Boat variable
summary(mbta6$Boat)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   2.985   3.494   4.293   5.068   5.356  40.000
```

```r
# Find the row number of the incorrect value: i
i<-which(mbta6$Boat==40)

# Replace the incorrect value with 4
mbta6$Boat[i]<-4

# Generate a boxplot of Boat column to verify that the change occurred
mbta6%>%
  ggplot(aes(x=1, y=Boat) )+
geom_boxplot()
```

![](EDA_MBTA_ridership_data_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

_**Dealing with the Trackless Trolley outliers**_

A boxplot on Trackless Trolley Reveals that the outliers are those values which are below 10. We will convert them to NAs.


```r
#summary of Boat variable
summary(mbta6$`Trackless Trolley`)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   5.777  11.679  12.598  12.125  13.320  15.109
```

```r
# Generate a boxplot of Trackless Trolley column to look out for the outliers
mbta6%>%
  ggplot(aes(x=1, y=`Trackless Trolley`) )+
geom_boxplot()
```

![](EDA_MBTA_ridership_data_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

```r
# Replace the outliers with NAs
mbta6$`Trackless Trolley`=ifelse(mbta6$`Trackless Trolley`<10, NA, mbta6$`Trackless Trolley`)

# Generate a boxplot of Trackless Trolley column to verify that the change occurred
mbta6%>%
  ggplot(aes(x=1, y=`Trackless Trolley`) )+
geom_boxplot()
```

```
## Warning: Removed 7 rows containing non-finite values (stat_boxplot).
```

![](EDA_MBTA_ridership_data_files/figure-html/unnamed-chunk-14-2.png)<!-- -->

# Which is the most commonly used mode of transport?
To answer this question well we need to gather the modes of transport into a single column mode. We also need to convert this column to factor. We will also convert the year column to integer.


```r
# gather columns Boat to Trackless Trolley
mbta7=mbta6%>%
      gather(mode, thou_riders, -c(year, month) )

#view the first 6 rows of mbta7
head(mbta7)
```

```
## # A tibble: 6 x 4
##   year  month mode  thou_riders
##   <chr> <chr> <chr>       <dbl>
## 1 2007  Apr   Boat         4.3 
## 2 2007  Aug   Boat         6.57
## 3 2007  Dec   Boat         2.98
## 4 2007  Feb   Boat         3.6 
## 5 2007  Jan   Boat         4   
## 6 2007  Jul   Boat         6.52
```

```r
View(mbta7)

#convert types
mbta7$year=as.integer(mbta7$year)
mbta7$mode=as.factor(mbta7$mode)

#view internal structure
glimpse(mbta7)
```

```
## Observations: 464
## Variables: 4
## $ year        <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 20...
## $ month       <chr> "Apr", "Aug", "Dec", "Feb", "Jan", "Jul", "Jun", "...
## $ mode        <fct> Boat, Boat, Boat, Boat, Boat, Boat, Boat, Boat, Bo...
## $ thou_riders <dbl> 4.300, 6.572, 2.985, 3.600, 4.000, 6.521, 5.800, 4...
```

_**Checking for the most commonly used means of transport**_



```r
ggplot(mbta7, aes(x = year, y = sqrt(thou_riders), col = mode )) + geom_smooth( se=FALSE) + 
scale_x_continuous(name = "Year", breaks = c(2007, 2008, 2009, 2010, 2011)) +  
scale_y_continuous(name = "Avg Weekday Ridership Log(thousands)")
```

```
## `geom_smooth()` using method = 'loess'
```

```
## Warning: Removed 7 rows containing non-finite values (stat_smooth).
```

![](EDA_MBTA_ridership_data_files/figure-html/unnamed-chunk-16-1.png)<!-- -->
The most commonly used mode of transport is Heavy rail


# Which is the least commonly used mode of transpot?

_**Checking for the least commonly used means of transport**_


```r
suppressWarnings(ggplot(mbta7, aes(x = year, y = sqrt(thou_riders), col = mode )) + geom_smooth( se=FALSE) + 
scale_x_continuous(name = "Year", breaks = c(2007, 2008, 2009, 2010, 2011)) +  
scale_y_continuous(name = "Avg Weekday Ridership Log(thousands)"))
```

```
## `geom_smooth()` using method = 'loess'
```

```
## Warning: Removed 7 rows containing non-finite values (stat_smooth).
```

![](EDA_MBTA_ridership_data_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

The least commonly used mode of transport is Private bus


# How do all the modes of ridership vary every January?


```r
suppressWarnings(mbta7%>%
  filter(month=="Jan")%>%
ggplot( aes(x = year, y = sqrt(thou_riders), col = mode )) + geom_smooth( se=FALSE) + 
scale_x_continuous(name = "Year", breaks = c(2007, 2008, 2009, 2010, 2011)) +  
scale_y_continuous(name = "Avg Weekday Ridership Log(thousands)"))
```

```
## `geom_smooth()` using method = 'loess'
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : span too small. fewer data values than degrees of freedom.
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : pseudoinverse used at 2007
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : neighborhood radius 2.02
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : reciprocal condition number 0
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : There are other near singularities as well. 4.0804
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : span too small. fewer data values than degrees of freedom.
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : pseudoinverse used at 2007
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : neighborhood radius 2.02
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : reciprocal condition number 0
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : There are other near singularities as well. 4.0804
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : span too small. fewer data values than degrees of freedom.
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : pseudoinverse used at 2007
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : neighborhood radius 2.02
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : reciprocal condition number 0
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : There are other near singularities as well. 4.0804
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : span too small. fewer data values than degrees of freedom.
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : pseudoinverse used at 2007
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : neighborhood radius 2.02
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : reciprocal condition number 0
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : There are other near singularities as well. 4.0804
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : span too small. fewer data values than degrees of freedom.
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : pseudoinverse used at 2007
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : neighborhood radius 2.02
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : reciprocal condition number 0
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : There are other near singularities as well. 4.0804
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : span too small. fewer data values than degrees of freedom.
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : pseudoinverse used at 2007
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : neighborhood radius 2.02
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : reciprocal condition number 0
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : There are other near singularities as well. 4.0804
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : span too small. fewer data values than degrees of freedom.
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : pseudoinverse used at 2007
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : neighborhood radius 2.02
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : reciprocal condition number 0
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : There are other near singularities as well. 4.0804
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : span too small. fewer data values than degrees of freedom.
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : pseudoinverse used at 2007
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : neighborhood radius 2.02
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : reciprocal condition number 0
```

```
## Warning in simpleLoess(y, x, w, span, degree = degree, parametric =
## parametric, : There are other near singularities as well. 4.0804
```

![](EDA_MBTA_ridership_data_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

The ridership for the different modes of transport dont seem to vary much every January.
