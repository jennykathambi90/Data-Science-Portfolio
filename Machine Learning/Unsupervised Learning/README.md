---
title: "Unsupervised Learning"
author: "Jane Kathambi"
date: "18 July 2018"
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
Unsupervised learning refers to a set of statistical techniques for exploring and discovering knowledge, from a multivariate data, without building a predictive models.

It makes it possible to visualize the relationship between variables, as well as, to identify groups of similar individuals (or observations).

The most popular unsupervised learning methods, include:

* Principal component methods, which consist of summarizing and visualizing the most important information contained in a multivariate data set.

* Cluster analysis for identifying groups of observations with similar profile according to a specific criteria. These techniques include: 
    + hierarchical clustering and 
    + k-means clustering.

# Principal component methods
Principal component methods allows us to summarize and visualize the most important information contained in a multivariate data set.

The type of principal component methods to use depends on *variable types* contained in the data set. These methods include:

* *Principal Component Analysis (PCA)*, which is one of the most popular multivariate analysis method. The goal of PCA is to summarize the information contained in a *continuous (i.e, quantitative) multivariate data* by reducing the dimensionality of the data without loosing important information.

* *Correspondence Analysis (CA)*, which is an extension of the principal component analysis for analyzing *a large contingency table formed by two qualitative variables (or categorical data)*.

* *Multiple Correspondence Analysis (MCA)*, which is an adaptation of CA to *a data table containing more than two categorical variables*.

## R packages for PCA
Several functions from different packages are available in the R software for computing PCA:

* prcomp() and princomp() [built-in R stats package],
* PCA() [FactoMineR package],
* dudi.pca() [ade4 package],
* and epPCA() [ExPosition package]

No matter what function you decide to use, you can easily extract and visualize the results of PCA using R functions provided in the factoextra R package.

In this study we will use:
* FactoMineR for computing principal component methods
* factoextra for visualizing the output of FactoMineR.(an extension to ggplot2)

Install the two packages as follow:

install.packages(c("FactoMineR", "factoextra"))

# Cluster analysis
Cluster analysis is used to identify groups of similar objects in a multivariate data sets collected from fields such as marketing, bio-medical and geo-spatial. 

There are different types of clustering methods, including:

* Partitioning clustering: Subdivides the data into a set of k groups.
* Hierarchical clustering: Identify groups in the data without subdividing it.

*Distance measures*
The classification of observations into groups requires some methods for computing the distance or the (dis)similarity between each pair of observations. The result of this computation is known as a dissimilarity or distance matrix. There are different methods for measuring distances, including:

* Euclidean distance
* Correlation based-distance

*What type of distance measures should we choose?* The choice of distance measures is very important, as it has a strong influence on the clustering results. For most common clustering software, the default distance measure is the Euclidean distance.

Depending on the type of the data and the researcher questions, other dissimilarity measures might be preferred.

* If we want to identify clusters of observations with the *same overall profiles* regardless of their magnitudes, then we should go with *correlation-based distance* as a dissimilarity measure. 

    + This is particularly the case in gene expression data analysis, where we might want to consider genes similar when they are UP and Down together. 
    
    + It is also the case, in marketing if we want to identify group of shoppers with the same preference in term of items, regardless of the volume of items they bought.
    
* If *Euclidean distance* is chosen, then observations with *high values of features* will be clustered together. The same holds true for observations with *low values of features*.

# Data standardization*
Before PCA and cluster analysis, it's recommended to scale (or normalize) the data, to make the variables comparable. This is particularly recommended when variables are measured in different scales (e.g: kilograms, kilometers, centimeters); otherwise, the dissimilarity measures obtained will be severely affected.

The goal is to avoid some variables to become dominant just because of their large measurement units. It makes variable comparable.

The standardization of data is an approach widely used in the context of gene expression data analysis before PCA and clustering analysis. We might also want to scale the data when the mean and/or the standard deviation of variables are largely different.

Note that, by default, the function PCA() [in FactoMineR], standardizes the data automatically during the PCA; so you don't need do this transformation before the PCA.

R function for scaling the data: scale(), applies scaling on the column of the data (variables).

Required R packages for clustering:

* cluster for cluster analysis.
* factoextra for cluster visualization.





