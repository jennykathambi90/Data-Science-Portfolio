---
title: "Exploratory Data Analysis: Categorical Data"
author: "Jane Kathambi"
date: "8 June 2018"
output: 
  html_document:
    keep_md: yes
    theme: united
    number_sections: true
    toc: yes
    toc_depth: 5
    toc_float:
      collapsed: yes
      smooth_scroll: yes
---

# Introduction. Data Analysis Overview.


Data analysis is a process of inspecting, cleansing, transforming, and modeling data with the goal of discovering useful information, informing conclusions, and supporting decision-making. 

Three popular data analysis approaches are:

1. Classical
2. Exploratory (EDA)
3. Bayesian

These three approaches are similar in that they all start with a general science/engineering problem and all yield science/engineering conclusions. The difference is the sequence and focus of the intermediate steps as shown below:

* For **classical analysis**, the sequence is:

    + Problem => Data => Model => Analysis => Conclusions

* For **EDA**, the sequence is:

    + Problem => Data => Analysis => Model => Conclusions

* For **Bayesian**, the sequence is:

    + Problem => Data => Model => Prior Distribution => Analysis => Conclusions

Thus for classical analysis, the data collection is followed by the imposition of a model (normality, linearity, etc.) and the analysis, estimation, and testing that follows are focused on the parameters of that model. For EDA, the data collection is not followed by a model imposition; rather it is followed immediately by analysis with a goal of inferring what model would be appropriate. 

Classical Analysis                   | Eda
------------------------------------ | -----------------------------------------
**Imposes models** (both deterministic and probabilistic) on the data. Deterministic models include, for example, regression models and analysis of variance (ANOVA) models. The most common probabilistic model assumes that the errors about the deterministic model are normally distributed--this assumption affects the validity of the ANOVA F tests. | **Does not impose** deterministic or probabilistic models on the data. On the contrary, the EDA approach allows the data to suggest admissible models that best fit the data.
The *focus is on the model*--estimating parameters of the model and generating predicted values from the model. |  The *focus is on the data*--its structure, outliers, and models suggested by the data.
Classical **techniques** are generally **quantitative** in nature. They include ANOVA, t tests, chi-squared tests, and F tests. | EDA **techniques** are generally **graphical**. They include scatter plots, character plots, box plots, histograms, bihistograms, probability plots, residual plots, and mean plots.
Classical techniques serve as the **probabilistic foundation** of science and engineering; the most important characteristic of classical techniques is that they are **rigorous, formal, and "objective"**. | EDA techniques **do not share in that rigor or formality**. EDA techniques make up for that lack of rigor by being **very suggestive, indicative, and insightful about what the appropriate model should be**. EDA techniques are **subjective** and **depend on interpretation which may differ from analyst to analyst**, although experienced analysts commonly arrive at identical conclusions.
Classical estimation techniques have the characteristic of taking all of the data and mapping the data into a few numbers ("estimates"). This is both a virtue and a vice. The virtue is that these few numbers focus on important characteristics (location, variation, etc.) of the population. The vice is that concentrating on these few characteristics can **filter out other characteristics (skewness, tail length, autocorrelation, etc.) of the same population. In this sense there is a loss of information due to this "filtering" process**. | The EDA approach, on the other hand, often makes use of (and shows) all of the available data. In this sense there is **no corresponding loss of information**.
classical tests **depend on underlying assumptions** (e.g., normality), and hence the validity of the test conclusions becomes dependent on the validity of the underlying assumptions. Worse yet, the exact underlying assumptions may be unknown to the analyst, or if known, untested. | Many EDA techniques make **little or no assumptions--they present and show the data--all of the data--as is, with fewer encumbering assumptions**

With that in mind let us now dive into EDA of Categorical Data. Remember that EDA is graphical and bar charts are good for categorical data. We can also use contingency tables. A contingency table is a table showing the distribution of one variable in rows and another in columns, used to study the correlation between the two variables. It is a useful way to represent the total counts of observations that fall into each combination of the levels of categorical variables.

We will work with the comics data set.This is a collection of characteristics on all of the superheroes created by Marvel and DC comics in the last 80 years.

# Loading the Required Libraries


Let us load the required libraries


```r
library(ggplot2)
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

# Loading the comics data


We will use read.csv() of the utils package i.e base r package.


```r
comics<-read.csv("data/comics.csv")
```


# EDA of the Comics dataset


## Initial exploration of the comics data set



```r
# Print the first 6 rows of the data
head(comics)
```

```
##                                    name      id   align        eye
## 1             Spider-Man (Peter Parker)  Secret    Good Hazel Eyes
## 2       Captain America (Steven Rogers)  Public    Good  Blue Eyes
## 3 Wolverine (James \\"Logan\\" Howlett)  Public Neutral  Blue Eyes
## 4   Iron Man (Anthony \\"Tony\\" Stark)  Public    Good  Blue Eyes
## 5                   Thor (Thor Odinson) No Dual    Good  Blue Eyes
## 6            Benjamin Grimm (Earth-616)  Public    Good  Blue Eyes
##         hair gender  gsm             alive appearances first_appear
## 1 Brown Hair   Male <NA> Living Characters        4043       Aug-62
## 2 White Hair   Male <NA> Living Characters        3360       Mar-41
## 3 Black Hair   Male <NA> Living Characters        3061       Oct-74
## 4 Black Hair   Male <NA> Living Characters        2961       Mar-63
## 5 Blond Hair   Male <NA> Living Characters        2258       Nov-50
## 6    No Hair   Male <NA> Living Characters        2255       Nov-61
##   publisher
## 1    marvel
## 2    marvel
## 3    marvel
## 4    marvel
## 5    marvel
## 6    marvel
```

```r
#view the variable names of comics
names(comics)
```

```
##  [1] "name"         "id"           "align"        "eye"         
##  [5] "hair"         "gender"       "gsm"          "alive"       
##  [9] "appearances"  "first_appear" "publisher"
```

```r
#view the internal structure of comics
glimpse(comics)
```

```
## Observations: 23,272
## Variables: 11
## $ name         <fct> Spider-Man (Peter Parker), Captain America (Steve...
## $ id           <fct> Secret, Public, Public, Public, No Dual, Public, ...
## $ align        <fct> Good, Good, Neutral, Good, Good, Good, Good, Good...
## $ eye          <fct> Hazel Eyes, Blue Eyes, Blue Eyes, Blue Eyes, Blue...
## $ hair         <fct> Brown Hair, White Hair, Black Hair, Black Hair, B...
## $ gender       <fct> Male, Male, Male, Male, Male, Male, Male, Male, M...
## $ gsm          <fct> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
## $ alive        <fct> Living Characters, Living Characters, Living Char...
## $ appearances  <int> 4043, 3360, 3061, 2961, 2258, 2255, 2072, 2017, 1...
## $ first_appear <fct> Aug-62, Mar-41, Oct-74, Mar-63, Nov-50, Nov-61, N...
## $ publisher    <fct> marvel, marvel, marvel, marvel, marvel, marvel, m...
```

```r
# Check levels of align
levels(comics$align)
```

```
## [1] "Bad"                "Good"               "Neutral"           
## [4] "Reformed Criminals"
```

```r
# Check the levels of gender
levels(comics$gender)
```

```
## [1] "Female" "Male"   "Other"
```

## Distribution of two variables


### The alignment versus gender **contingency table**


As said earlier a contingency table is a useful way to represent the total counts of observations that fall into each combination of the levels of categorical variables.

We will show the cotingency table of comics alignment versus gender. Save it as contab.


```r
# Create a 2-way contingency table: contab
contab<-table(comics$align,comics$gender)

#show contab
contab
```

```
##                     
##                      Female Male Other
##   Bad                  1573 7561    32
##   Good                 2490 4809    17
##   Neutral               836 1799    17
##   Reformed Criminals      1    2     0
```

### Dropping levels


The contingency table revealed that there are some levels that have very low counts. To simplify the analysis, it often helps to drop such levels.

In R, this requires two steps: 
  
  1. first filtering out any rows with the levels that have very low counts, then 2. removing these levels from the factor variable with droplevels(). This is because the droplevels() function would keep levels that have just 1 or 2 counts; it only drops levels that don't exist in a dataset.

#### Explore the levels to find out which ones have the lowest entries.


Let us print contab to find out which level of align has the fewest total entries.

```r
#print contab
contab
```

```
##                     
##                      Female Male Other
##   Bad                  1573 7561    32
##   Good                 2490 4809    17
##   Neutral               836 1799    17
##   Reformed Criminals      1    2     0
```

From the above coningency level it is the _**"reformed criminals level"**_ which has the fewest total entries.

#### Drop the levels that have the lowest entries.


Use filter() to filter out all rows of comics with that level, then drop the unused level with droplevels(). Save the simplifed dataset over the old one as comics.


```r
# Load dplyr. We have already loaded it.

# Remove "Reformed Criminals" level of the align variable
comics <- comics %>%
filter(align != "Reformed Criminals") %>%
droplevels()
```

Confirm that the _**"reformed criminals level"**_ has been dropped using the contingency table as show below:


```r
# Create a 2-way contingency table: contab
contab<-table(comics$align,comics$gender)

#show contab
contab
```

```
##          
##           Female Male Other
##   Bad       1573 7561    32
##   Good      2490 4809    17
##   Neutral    836 1799    17
```


The _**"reformed criminals level"**_ has been dropped.

### Side-by-side bar charts

While a contingency table represents the counts numerically, it's often more useful to represent them graphically.

Here we'll construct two side-by-side barcharts of the comics data. This shows that there can often be two or more options for presenting the same data. Passing the argument position = "dodge" to geom_bar() says that we want a side-by-side (i.e. not stacked) barchart.


```r
# Load ggplot2. Already loaded

# Create side-by-side barchart of gender by alignment
ggplot(comics, aes(x = align, fill = gender)) + 
  geom_bar(position = "dodge")
```

![](exploring_categorical_data_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

```r
# Create side-by-side barchart of alignment by gender
ggplot(comics, aes(x = gender, fill = align)) + 
  geom_bar(position="dodge") +
  theme(axis.text.x = element_text(angle = 90))# Rotate the x axis text to 90 degrees for readability
```

![](exploring_categorical_data_files/figure-html/unnamed-chunk-8-2.png)<!-- -->

#### Bar chart interpretation


In general, there is an association between gender and alignment.

There are more male characters than female characters in this dataset.
press 4

Among characters with "Neutral" alignment, males are the most common.

### Counts versus proportions


Sometimes counts of cases can be useful but often it is the proportions that are more interesting.

Proportions can be presented with:

1. Contingency tables by using the prop.table() function as follows:

    + Joint proportions: prop.table(contingency_table)
    
    + Conditional Proportions. The condition can be 1 or 2, 1 represnting rows and 2 represetning columns.i.e prop.table(contingency_table, 2)  which is Conditional on columns
    
2. A stacked bar chart: by asigning position of the geom bar to fill.

#### Contingency table and proportions


##### Joint contingency table proportions



```r
prop.table(contab) # Joint proportions
```

```
##          
##                 Female         Male        Other
##   Bad     0.0822096791 0.3951604474 0.0016724156
##   Good    0.1301348385 0.2513327062 0.0008884708
##   Neutral 0.0436918574 0.0940211142 0.0008884708
```

##### Interpreting the joint proportions above


The largest category is males who are bad.

**Problem**
What if we want to know which  gender has the best or worst charaters. Based on the previous contigency table of counts it is evident that males are more than females so this joint proportions table will not give us the correct answer. 

**What is the solution now?**
The solution is to introduce a condition column wise i.e on the gender.


##### Conditional contingency table proportions



```r
prop.table(contab, 2)#Conditional on columns i.e gender
```

```
##          
##              Female      Male     Other
##   Bad     0.3210859 0.5336298 0.4848485
##   Good    0.5082670 0.3394029 0.2575758
##   Neutral 0.1706471 0.1269673 0.2575758
```

##### Interpreting the conditional proportions above


From the conditional table above we can see that the female gender has more good characters than bad characters while the male gender has more bad characters than good charactes.

#### Stacked Bar charts and proportions


Stacked bar charts are the ones that show proportions.

Lets represent the above conditional contingency table, that is conditional on the gender, as a sctacked bar chart.


```r
# Plot proportion of align, conditional on gender
ggplot(comics, aes(x = gender,fill = align)) + 
  geom_bar(position = "fill") +
  ylab("proportion")
```

![](exploring_categorical_data_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

##### Interpreting the conditional proportions above


From the above stacked bar chart we can see that the female gender has more good characters than bad characters while the male gender has more bad characters than good charactes.

## Distribution of one variable


### Marginal barchart


Marginal because it tabulates the count of alignments across all the variables.

If we are interested in the distribution of alignment of all superheroes, it makes sense to construct a barchart for just that single variable.

we can improve the interpretability of the plot, though, by implementing some sensible ordering. Superheroes that are "Neutral" show an alignment between "Good" and "Bad", so it makes sense to put that bar in the middle.Marginal barchart

#### Change the order of the levels in align



```r
# Change the order of the levels in align
comics$align <- factor(comics$align, 
                      levels = c("Bad", "Neutral", "Good"))
```

#### Create a marginal barchart of align


```r
# Create plot of align
ggplot(comics, aes(x = align)) + 
  geom_bar()
```

![](exploring_categorical_data_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

#### Interpreting the marginal barchat of align

The bad characters are the majority.


### Conditional barchart--Facetting


Now, if we want to break down the distribution of alignment based on gender, we're looking for conditional distributions.

we could make these by creating multiple filtered datasets (one for each gender) or by faceting the plot of alignment based on gender.

We will work with facetting.


```r
# Plot of alignment broken down (facetted) by gender
ggplot(comics, aes(x = align)) + 
  geom_bar() +
  facet_wrap(~ gender)
```

![](exploring_categorical_data_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


#### Interpreting the conditional barchat of align facetted by align


For the the female gender the good characters are more than the bad characters.

For the male gender the bad characters are more than the good characters.


### Pie charts
A pie chart (or a circle chart) is a circular statistical graphic which is divided into slices to illustrate numerical proportion. 

In a pie chart, the arc length of each slice (and consequently its central angle and area), is proportional to the quantity it represents.

The piechart is a very common way to represent the distribution of a single categorical variable, but they can be more difficult to interpret than barcharts.

#### Marginal Pie charts


```r
# Create a piechart of align
ggplot(comics, aes(x =1, fill= align)) + 
  geom_bar()+ 
  coord_polar("y", start=0)
```

![](exploring_categorical_data_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

# Conclusion
It is difficult to interpret pie charts therefore let us always stick to bar charts.

Categorical data sets are best presented with conditional proportions. There are two ways of doing this:

1. stacked bar charts alongside the fill aesthetic. These show conditional proportions. 
2. Conditional contingency tables. These too show conditional proportions.
