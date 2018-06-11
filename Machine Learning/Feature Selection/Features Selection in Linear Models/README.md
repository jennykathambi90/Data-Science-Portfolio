---
title: "Feature Selection for Linear Models using LASSO"
author: "Jane Kathambi"
date: "8 June 2018"
output: 
  html_document:
    keep_md: yes
    theme: united
    toc: yes
    toc_depth: 4
    toc_float:
      collapsed: yes
      smooth_scroll: yes
---

# .

# Introduction
Machine learning models are always faced with the problem of data overfitting in case of high-dimensional data or with data including many correlated predictors. Overfitting includes irrelevant features and leads to poor predictions or inference. To address this problem I have used the lasso algorithm.

The lasso does:

* Coefficient shrinkage and 
* Variable selection, by ensuring that certain coefficients are actually equal to zero leaving those that are highly correlated to the output.

We are going to fit a lasso model using two algorithms of the glmnet package:

1. Glmnet: 
glmnet() is a R package which can be used to fit Regression models,lasso model and others. Alpha argument determines what type of model is fit. When alpha=0, Ridge Model is fit and if alpha=1, a lasso model is fit.
By default, glmnet() will perform Ridge or Lasso regression for an automatically selected range of lambda which may not give the lowest test MSE. Cv.glmnet is better.

2. Cv.glmnet: 
cv.glmnet() performs cross-validation, by default 10-fold which can be adjusted using nfolds. A 10-fold CV will randomly divide your observations into 10 non-overlapping groups/folds of approx equal size. The first fold will be used for validation set and the model is fit on 9 folds. Bias Variance advantages is usually the motivation behind using such model validation methods. In the case of lasso and ridge models, CV helps choose the value of the tuning parameter lambda.
The chosen optimal lamda/tuning parameter has corresponding non-zero coefficients of the important features/varialbes hence feature selection.