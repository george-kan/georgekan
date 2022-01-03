---
title: The Normal Distribution
date: '2021-12-13'
slug: []
categories:
  - statistics
tags:
  - statistics
subtitle: ''
excerpt: ''
series: ~
layout: single
---



### Introduction

This is the fourth post in my statistics series and this time we are going to focus on the normal distribution.  
It is arguable the most important probability distribution in statistics and is the key to understanding and modelling many phenomena. In this post, we will see what the normal is, how it can be manipulated and talk about how to calculate the area under it and its quantiles.

### (Brief) History lesson

The first person to "discover" the normal distribution was Abraham de Moivre, an 18th century statistician. He was trying to find a good way to estimate probabilities in gambling and he came across the normal distribution as the limiting curve for the binomial.  
In simpler terms, he noticed that if he plotted the number of heads that came out he could spot a curve above the distribution. What he probably noticed was something like this:



```r
binom_dt = data.table(x = 0:20)
binom_dt[, y := dbinom(x, 20, prob=0.5)]

ggplot(binom_dt) +
  geom_point(aes(x=x, y=y), size=4, color="blue") +
  geom_segment(aes(x=x, xend=x, y=0, yend=y), size=1.5, color="blue") +
  geom_function(fun = dnorm, args= list(mean=10, sd=2.24), size=1.2, color = muted("green")) +
  scale_y_continuous(expand = expansion(mult = 0.02)) +
  scale_x_continuous(labels = number_format(1), breaks = 0:20) + 
  labs(title ="Curve above a binomial distribution", x="Number of heads in 20 coin tosses") +
  theme(axis.title.y = element_blank(),
        panel.grid.minor = element_blank())
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />

After that first occurrence, it was observed in various calculations but it was formalised by Adrain and Gauss at the beginning of the 19th century. And this is how we came to this lovely formula:

$$ \LARGE f_X(x) = \frac 1 {\sigma \sqrt{2\pi}}e^{-\frac 1 2 (\frac {x-\mu} {\sigma})^2} $$

This is the formula for a generic normal distribution $$ Ν(\mu, \sigma^2) $$ fdsafdsa












### Resources used for this post

https://onlinestatbook.com/2/normal_distribution/history_normal.html

