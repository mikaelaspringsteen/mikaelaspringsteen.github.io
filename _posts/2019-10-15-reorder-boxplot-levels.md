---
layout: post
title: "Reordering Boxplot Levels"
date: 2019-10-15
categories: R
---

{% newthought "When examining data," %} one of the first things I like to do is take a look at the distribution of the various variables, as well as their various interactions. Yet as anyone who has worked with data will be able to tell you, this is often much easier said than done.

<!--more--> Recently, while working with a particularly large dataset, I ran into a large number of factor variables, each with over 40 levels, and wanted to quickly break down the distribution of a particular continuous variable for each of these levels.

Density plots quickly become illegible with more than a few levels overlaid at once and so were immediately ruled out. {% sidenote "sn-id-1" "Granted, you could split the levels into multiple plots, but that doesn't exactly simplify comparison of distributions." %}

{% marginfigure "marginfig-1" "assets/img/posts/2019-10-15/messy_boxplot.png" "A small exerpt of the headache-inducing boxplot first generated from my data" %} Boxplots seemed the more reasonable option, but with so many levels present for each variable they could just as easily become a confusing mess if not ordered in some way. (Remember, the goal here is to get a quick overview of the relative distributions&mdash;something which becomes difficult if the eye is confused by a large number of unordered boxplots.)

Luckily, reordering boxplots is simple in ggplot2. Below, I have used sample data to detail the process.

## Reordering boxplots in ggplot2

``` R
library(tidyverse)
library(ggthemes)
data(iris)
```

Creating a boxplot will result in levels being arranged just as they are in the dataset:

``` R
ggplot(iris, aes(x = Species, y = Sepal.Width)) +
  geom_boxplot(alpha = .4, outlier.alpha = .4) +
  labs(
    title = "Distribution of Sepal Width by Iris Species",
    x = "Species",
    y = "Sepal Width") +
  theme_fivethirtyeight() +
  theme(axis.title = element_text())
```
{% maincolumn "assets/img/posts/2019-10-15/boxplot_a.png" %}

But, by including the `reorder()` function in the appropriate aesthetics argument, the order of the levels can be quickly and simply set to any function desired. Here, I've used the median:

``` R
ggplot(iris, aes(x = reorder(Species, Sepal.Width, median), y = Sepal.Width)) +
  geom_boxplot(alpha = .4, outlier.alpha = .4) +
  labs(
    title = "Distribution of Sepal Width by Iris Species",
    subtitle = "arranged by median sepal width",
    x = "Species",
    y = "Sepal Width") +
  theme_fivethirtyeight() +
  theme(axis.title = element_text())
```
{% maincolumn "assets/img/posts/2019-10-15/boxplot_b.png" %}

{% marginfigure "marginfig-2" "assets/img/posts/2019-10-15/neat_boxplot.png" "Much better" %}This quickly highlights those levels at the high and low ends of the spectrum, and allows easy comparison of those levels which are more similar.

The advantage of this method lies in allowing the order of the levels to remain unchanged in the dataset, while enabling the boxplots to be quickly reordered according to a number of different metrics, simply by including the appropriate function in the `reorder()` call.
