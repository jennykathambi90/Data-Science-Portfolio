---
title: "Numerical Summaries"
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

# Introduction
We are going to look at numerical summaries.
We will explore the four characteristics of distribution which are:

1. Measures of center, 
2. Measures of variability(spread),
3. The shape of the distribution,
4. Outliers

We will use data from gapminder, which tracks demographic data in countries of the world over time. The gapminder package can be downloaded and installed from CRAN.

# Loading the Required Libraries


Let us load the required libraries


```r
library(data.table)
library(ggplot2)
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:data.table':
## 
##     between, first, last
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
library(gapminder)
```

# Let us explore the data a bit.


```r
# View the size of the data and the variable types
glimpse(gapminder)
```

```
## Observations: 1,704
## Variables: 6
## $ country   <fct> Afghanistan, Afghanistan, Afghanistan, Afghanistan, ...
## $ continent <fct> Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia, Asia...
## $ year      <int> 1952, 1957, 1962, 1967, 1972, 1977, 1982, 1987, 1992...
## $ lifeExp   <dbl> 28.801, 30.332, 31.997, 34.020, 36.088, 38.438, 39.8...
## $ pop       <int> 8425333, 9240934, 10267083, 11537966, 13079460, 1488...
## $ gdpPercap <dbl> 779.4453, 820.8530, 853.1007, 836.1971, 739.9811, 78...
```

```r
# view head
head(gapminder)
```

```
## # A tibble: 6 x 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Afghanistan Asia       1952    28.8  8425333      779.
## 2 Afghanistan Asia       1957    30.3  9240934      821.
## 3 Afghanistan Asia       1962    32.0 10267083      853.
## 4 Afghanistan Asia       1967    34.0 11537966      836.
## 5 Afghanistan Asia       1972    36.1 13079460      740.
## 6 Afghanistan Asia       1977    38.4 14880372      786.
```

```r
#variable names
names(gapminder)
```

```
## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"
```

```r
#view whole data
View(gapminder)
```


We will focus on how the life expectancy differs from continent to continent. This requires that we conduct our analysis not at the country level, but aggregated up to the continent level. This is made possible by the combination of group_by() and summarize(), a very powerful syntax for carrying out the same analysis on different subsets of the full dataset.

# Measure of center

Below are measures of tendancy and an explanation of which  work well with what data. 
**Mean**-> Balance point of the data. Very sensitive to extreme values i.e affected by skewed data so not a good choice.

**Median**-> Divides the data into a lower half and a upper half. Better than mean when working with skewed data.

**Mode**-> Most common entries.

So which one to use?

1. For highly skewed data median is the best, but mean for lowly skewed data.
2. For non skewed data all of them.

We will create a dataset called gap2007 that contains only data from the year 2007.
Using gap2007, we will:

1. calculate the mean and median life expectancy for each continent. 
2. Confirm the trends that we see in the medians by generating side-by-side box plots of life expectancy for each continent.


```r
# Create dataset of 2007 data
gap2007 <- filter(gapminder, year==2007)

# Compute groupwise mean and median lifeExp
gap2007 %>%
  group_by(continent) %>%
  summarize(mean(lifeExp), median(lifeExp))
```

```
## # A tibble: 5 x 3
##   continent `mean(lifeExp)` `median(lifeExp)`
##   <fct>               <dbl>             <dbl>
## 1 Africa               54.8              52.9
## 2 Americas             73.6              72.9
## 3 Asia                 70.7              72.4
## 4 Europe               77.6              78.6
## 5 Oceania              80.7              80.7
```

```r
# Generate box plots of lifeExp for each continent
gap2007 %>%
  ggplot(aes(x = continent, y = lifeExp)) +
  geom_boxplot()
```

![](Numerical_summaries_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

**Interpreting the above results.**
1. Oceania has the highest life  expectancy of 80.7 while Africa has the lowest life expectancy of 52.9

# Measure of Variability

**sd**-> Most commonly used. But does not work well with highly skewed data i.e extreme values

**Variance**-> shows how much the data varies from the measure of center. Not widely used because it is not in the same units with the original data so sd is better.

**IQR**-> best used with skewed data as it does not change

**Range**->Not often used because it does not work well with skewed data i.e extreme values.

So which one to use?

1. For highly skewed data IQR is the best, but sd for lowly skewed data.
2. For non skewed data all of them.

so median and IQR measure the central tendency and spread, respectively, but are robust to outliers and non-normal data.

Let's extend the powerful group_by() and summarize() syntax to measures of spread. If you're unsure whether you're working with symmetric or skewed distributions, it's a good idea to consider a robust measure like IQR in addition to the usual measures of variance or standard deviation. NB: You can also draw a density plot of the variables to determine if their distribution is skewed or not.

For each continent in gap2007, we will summarize life expectancies using the sd(), the IQR(), and the count of countries, n(). No need to name the new columns produced here. The n() function within your summarize() call does not take any arguments.

Graphically compare the spread of these distributions by constructing overlaid density plots of life expectancy broken down by continent.


```r
# Compute groupwise measures of spread
gap2007 %>%
  group_by(continent) %>%
  summarize(sd(lifeExp),
            IQR(lifeExp),
            n())
```

```
## # A tibble: 5 x 4
##   continent `sd(lifeExp)` `IQR(lifeExp)` `n()`
##   <fct>             <dbl>          <dbl> <int>
## 1 Africa            9.63          11.6      52
## 2 Americas          4.44           4.63     25
## 3 Asia              7.96          10.2      33
## 4 Europe            2.98           4.78     30
## 5 Oceania           0.729          0.516     2
```

```r
# Generate overlaid density plots
gap2007 %>%
  ggplot(aes(x = lifeExp, fill = continent)) +
  geom_density(alpha = 0.3)
```

![](Numerical_summaries_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

**Interpreting the above results.**
1. The life expectancy of Africa varies the most while that of Oceania varies least.

# The shape of the distribution

The shape of the distribution can be described in terms of the modality and the skew.

## Modality

The number of prominent humps that show up in the distribution. If:

1. Single mode i.e bell curve, the modality is called unimodal.
2. Two prominent modes, bimodal.
3. Three or more modes, multimodal.
4. No distinct mode because the distribution is flat across all values, uniform.
 

## Skew

If a distribution has a:

1. Long tail that stretches out to the right, the distribution is referred to as right-skewed.
2. Long tail that stretches out to the left, the distribution is referred to as left-skewed.
3. None of the tails is longer than the other, the distribution is referred to as symmetric.

## Transformations

Highly skewed distributions can make it very difficult to learn anything from a visualization. Transformations can be helpful in revealing the more subtle structure. One can use logarithm or square root fucntions to transform the heavily skewed data.

We will focus on the population variable, which exhibits strong right skew, and transform it with the natural logarithm function (log() in R).

Using the gap2007 data:

Create a density plot of the population variable.
Mutate a new column called log_pop that is the natural log of the population and save it back into gap2007.
Create a density plot of your transformed variable.


```r
# Create density plot of old variable
gap2007 %>%
  ggplot(aes(x = pop)) +
  geom_density()
```

![](Numerical_summaries_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
# Transform the skewed pop variable
gap2007 <- gap2007 %>%
  mutate(log_pop=log(pop) )

# Create density plot of new variable
gap2007 %>%
  ggplot(aes(x = log_pop)) +
  geom_density()
```

![](Numerical_summaries_files/figure-html/unnamed-chunk-5-2.png)<!-- -->


# Outliers
Oultiers are observations that have extreme values. They are often interesting cases, and it is good to know about them before proceeding with analysis. 

Oultliers cause distributions to be skewed. Highly skewed distributions can make it very difficult to learn anything from a visualization. Visualizing the data without the outliers can be helpful in revealing the more subtle structure. 

Box plots are good at displaying outliers.

## Identfying Outliers
Draw a box plot of the variable.

Let us look at the distribution of the life expectancies of the countries in Asia by drawing a box plot. 


```r
gap2007%>%
  filter(continent=="Asia")%>%
  ggplot(aes(x=1, y=lifeExp ) )+
  geom_boxplot()
```

![](Numerical_summaries_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

The box plot identifies one clear outlier: a state with a notably low life expectancy that is below 50.
So which country is this? Let us use filter and select to tell this:


```r
gap2007%>%
  filter(continent=="Asia")%>%
  filter(lifeExp<50)%>%
  select(country)
```

```
## # A tibble: 1 x 1
##   country    
##   <fct>      
## 1 Afghanistan
```

## Dealing with outliers

Ouliers are often interesting cases, and it is good to know about them before proceeding with analysis. 

It is often useful to consider oultiers separately from the rest of the data. 

We can create a new logical variable for outliers by applying a particular threshhold. This variable will state whether or not a given observation is an outlier.

we can then plot the data with outliers and without outliers for comparisons. Use filter to filter rows that have outliers or not.

We are going to:

1. Apply a filter so that it only contains observations from Asia, then create a new variable called is_outlier that is TRUE for countries with life expectancy less than 50. Assign the result to gap_asia.

2. Filter gap_asia to remove all outliers, then create another box plot of the remaining life expectancies.


```r
# Filter for Asia, add column indicating outliers
gap_asia <- gap2007 %>%
  filter(continent=="Asia") %>%
  mutate(is_outlier = lifeExp<50)

# Remove outliers, create box plot of lifeExp
gap_asia %>%
  filter(!is_outlier) %>%
  ggplot(aes(x = 1, y = lifeExp)) +
  geom_boxplot()
```

![](Numerical_summaries_files/figure-html/unnamed-chunk-8-1.png)<!-- -->
 
 The End...
