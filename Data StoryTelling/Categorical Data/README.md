---
title: " Exploratory Data Analysis: Categorical Data"
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
Categorical variables represent types of data which may be divided into groups. Examples of categorical variables are eye color, gender, alive.

Exploratory Data Analysis (EDA) is an approach/philosophy for data analysis that employs a variety of techniques (mostly graphical) to: 

* maximize insight into a data set;
* uncover underlying structure;
* extract important variables;
* detect outliers and anomalies;
* test underlying assumptions;
* develop parsimonious models; and
* determine optimal factor settings.



In this post we will do exploratory analysis of the categorical data comics . This is a collection of characteristics on all of the superheroes created by Marvel and DC comics in the last 80 years.

We will follow the following steps:

1. Look at an overview of data analysis and exploratory data analysis.
2. Load the comics data set
3. Look at its structure.
4. Create contigency tables and conditional contingency tables of two varialbes and interpret them.
5. Created bar charts and stacked bar charts of two categorical variables and interpret them.
6. Repeat steps 4 and 5  for a single variable.
8. Plot a marginal pie chart of one variable.
9. Make a conclusion.


# Conclusion
It is difficult to interpret pie charts therefore let us always stick to bar charts.

Categorical data sets are best presented with conditional proportions. There are two ways of doing this:

1. stacked bar charts alongside the fill aesthetic. These show conditional proportions. 
2. Conditional contingency tables. These too show conditional proportions.


