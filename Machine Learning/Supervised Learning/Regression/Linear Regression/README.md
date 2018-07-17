---
title: "Regression"
author: "Jane Kathambi"
date: "17 July 2018"
output: 
  html_document:
    keep_md: yes
    number_sections: yes
    theme: united
    toc: yes
    toc_depth: 4
    toc_float:
      collapsed: yes
      smooth_scroll: yes
---
# Introduction

A regression problem is one that requires us to predict a continuous outcome variable (y) based on the value of one or multiple predictor variables (x).

The goal of regression model is to build a mathematical equation that defines y as a function of the x variables. Next, this equation can be used to predict the outcome (y) on the basis of new values of the predictor variables (x).

# Two categories of regression models
1. Linear regression.
    + It assumes a linear relationship between the outcome and the predictor variables.
2. Non linear regression models.
    + These assume a non-linear relationship between the outcome and the predictor variables.

## Linear regression 
Linear regression is the most simple and popular technique for predicting a continuous variable. It assumes a linear relationship between the outcome and the predictor variables.

The linear regression equation can be written as y = b0 + b*x, where:

* b0 is the intercept,
* b is the regression weight or coefficient associated with the predictor variable x.

Technically, the linear regression coefficients are detetermined so that the error in predicting the outcome value is minimized. This method of computing the beta coefficients is called the Ordinary Least Squares method.

When you have multiple predictor variables, say x1 and x2, the regression equation can be written as y = b0 + b1*x1 + b2*x2. In some situations, there might be an **interaction effect** between some predictors, that is for example, increasing the value of a predictor variable x1 may increase the effectiveness of the predictor x2 in explaining the variation in the outcome variable.

Note also that, linear regression models can incorporate both continuous and categorical predictor variables.

When you build the linear regression model, you need to diagnostic whether linear model is suitable for your data. 

## Non liner Regression Models
In some cases, the relationship between the outcome and the predictor variables is not linear. In these situations, you need to build a non-linear regression, such as:

* Polynomial regression. This is the simple approach to model non-linear relationships. It add polynomial terms or quadratic terms (square, cubes, etc) to a regression.

* Spline regression. Fits a smooth curve with a series of polynomial segments. The values delimiting the spline segments are called Knots.

* Generalized additive models (GAM). Fits spline models with automated selection of knots.

* Log transformation. When you have a non-linear relationship, you can also try a logarithm transformation of the predictor variables. 

# Model Selection
When you have multiple predictors in the regression model, you might want to select the best combination of predictor variables to build an optimal predictive model. Linear model selection approaches include:

* Model selection:
    + best subsets regression and 
    + stepwise regression.
* principal component-based methods:
    + principal component regression and 
    + partial least squares regression.
* penalized regression:
    + ridge regression and 
    + the lasso regression
    
Best subset regression and stepwise regression compare multiple models containing different sets of predictors in order to select the best performing model that minimize the prediction error.

With principal component-based methods a large multivariate data set containing some correlated predictors can be summarized into few new variables (called principal components) that are a linear combination of the original variables. This few principal components can be used to build a linear model, which might be more performant for your data. 

Penalized regression penalizes the model for having too many variables.

# Accessing Model Accuracy
You can apply all these different regression models on your data, compare the models and finally select the best approach that explains well your data. To do so, you need some statistical metrics to compare the performance of the different models in explaining your data and in predicting the outcome of new test data.

The best model is defined as the model that has the lowest prediction error. The most popular metrics for comparing regression models, include:

* Root Mean Squared Error, which measures the model prediction error. It corresponds to the average difference between the observed known values of the outcome and the predicted value by the model. RMSE is computed as:

    + RMSE = mean((observeds - predicteds)^2) %>% sqrt(). 
    
  The lower the RMSE, the better the model.
  
* Adjusted R-square, representing the proportion of variation (i.e., information), in your data, explained by the model. This corresponds to the overall quality of the model. 
  The higher the adjusted R2, the better the model.

Note that, the above mentioned metrics should be computed on a new test data that has not been used to train (i.e. build) the model. If you have a large data set, with many records, you can randomly split the data into training set (80% for building the predictive model) and test set or validation set (20% for evaluating the model performance).

One of the most robust and popular approach for estimating a model performance is k-fold cross-validation. It can be applied even on a small data set. k-fold cross-validation works as follow:

1. Randomly split the data set into k-subsets (or k-fold) (for example 5 or 10 subsets)
    + This can be easily checked by creating a scatter plot of the outcome variable vs the predictor variable.
2. Reserve one subset and train the model on all other subsets
3. Test the model on the reserved subset and record the prediction error
4. Repeat this process until each of the k subsets has served as the test set.
5. Compute the average of the k recorded errors. This is called the cross-validation error serving as the performance metric for the model.

Taken together, the best model is the model that has the lowest cross-validation error, RMSE.

# Linear model assumptions
In addition to the linearity assumptions, the linear regression method makes many other assumptions about your data . You should make sure that these assumptions hold true for your data.

Potential problems, include:
* the presence of influential observations in the data i.e outliers.
    + Remove outliers before modelling your data.
    
* non-linearity between the outcome and some predictor variables.
    + use polynomial regression and smoothing splines in this case instead of linear regression.
    
* The presence of strong correlation between predictor variables.   
    + Select the best features before fitting the final model.
    
## simple workflow to build a predictive regression model
A simple workflow to build a predictive regression model is as follow:

1. Diagnose whether linear model is suitable for your data i.e there is a linear relationship between the predictor(s) and the response.
    + This can be easily checked by creating a scatter plot of the outcome variable vs the predictor variable.
2. Randomly split your data into training set (80%) and test set (20%)
3. Build the regression model using the training set
4. Make predictions using the test set and compute the model accuracy metrics

# Examples of data set
We'll use three different data sets: marketing [datarium package], the built-in R swiss data set, and the Boston data set available in the MASS R package.

## marketing data
The marketing data set [datarium package] contains the impact of three advertising medias (youtube, facebook and newspaper) on sales. It will be used for predicting sales units on the basis of the amount of money spent in the three advertising medias.

Data are the advertising budget in thousands of dollars along with the sales. The advertising experiment has been repeated 200 times with different budgets and the observed sales have been recorded.

First install the datarium package:

if(!require(devtools)) install.packages("devtools")
devtools::install_github("kassambara/datarium")


Then, load marketing data set as follows:

data("marketing", package = "datarium")
head(marketing, 3)
 
The marketing data has the following variabes:

* youtube
* facebook
* newspaper
* sales

## swiss data
The swiss describes 5 socio-economic indicators observed around 1888 used to predict the fertility score of 47 swiss French-speaking provinces.

Load and inspect the data:

data("swiss")
head(swiss, 3)
    
The data contain the following variables:

* Fertility Ig: common standardized fertility measure
* Agriculture: % of males involved in agriculture as occupation
* Examination: % draftees receiving highest mark on army examination
* Education: % education beyond primary school for draftees.
* Catholic: % 'catholic' (as opposed to 'protestant').
* Infant.Mortality: live births who live less than 1 year.

## Boston data
Boston [in MASS package] will be used for predicting the median house value (mdev), in Boston Suburbs, using different predictor variables:

* crim, per capita crime rate by town
* zn, proportion of residential land zoned for lots over 25,000 sq.ft
* indus, proportion of non-retail business acres per town
* chas, Charles River dummy variable (= 1 if tract bounds river; 0 otherwise)
* nox, nitric oxides concentration (parts per 10 million)
* rm, average number of rooms per dwelling
* age, proportion of owner-occupied units built prior to 1940
* dis, weighted distances to five Boston employment centres
* rad, index of accessibility to radial highways
* tax, full-value property-tax rate per USD 10,000
* ptratio, pupil-teacher ratio by town
* black, 1000(B - 0.63)^2 where B is the proportion of blacks by town
* lstat, percentage of lower status of the population
* medv, median value of owner-occupied homes in USD 1000's

Load and inspect the data:

data("Boston", package = "MASS")
head(Boston, 3)
