---
title: " Cleaning MBTA ridership data"
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

We will clean the data and do some visualizations with ggplot2 there after.

We are going to clean this data using the following guidelines:

1. **Understand the structure of the data.**
    + Look at the data strucutre and remove any unecessary data.
2. **Tidy the data** by making sure that the three principles of tidy data are followed.We will make sure that:

    + Each variable forms a column.
    + Each observation forms a row.
    + Each type of observational unit forms a table.

We will use tidyr functions which include:

* **Spread:** If column names are stored a sobservations spread them into key-value pairs
* **Gather:** If observations are stored as column names gather them into Key-Value Pairs.
* **Unite** to unite multiple columns into one column.
* **Separate** to separate one column into multiple columns

3. **Prepare the data for analysis.** We will make sure that:

    + Variables are of the correct data type
        + Dates are well formatted. Use lubridate or zoo package.
        + Column types are correct. We will use coersions.
    + Missing values are dealt with
    + Extreme values are dealt with
    + Unexpected values are dealt with


The Massachusetts Bay Transportation Authority ("MBTA") is stored as an Excel spreadsheet called mbta.xlsx in the data folder that is inside the data cleaning folder. We will use the read_excel() function from Hadley Wickham's readxl package to import it.