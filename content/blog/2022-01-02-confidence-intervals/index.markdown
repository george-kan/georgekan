---
title: Confidence intervals
date: '2022-01-02'
slug: []
categories:
  - statistics
tags:
  - statistics
subtitle: ''
excerpt: ''
draft: yes
series: ~
layout: single
---



### Introduction

Welcome to another post in my statistics series!  
In this post we are going to focus on confidence intervals and try to understand what is their purpose, how to construct them and how to interpret them.

### Definition 

In a previous post we saw how we can construct estimators to estimate quantities of interest. For example, we might have data from a normal distribution and we would like to create an estimator for the mean of that normal.  
But this is usually not enough. Being able to say that my estimate for the mean of the normal is 2 will not suffice. More often than not the degree of certainty for that estimate is needed. How certain/confident are we in that estimate?  

This is where the confidence interval comes into play. A 1-α confidence interval for a parameter θ is an interval Cn = (A, B) where A = A(X1,...,Xn) and B = B(X1,...,Xn) are functions of the data such that $$ P_{\theta}(\theta ∈ Cn) ≥ 1 − α , \forall θ ∈ Θ$$.
In words, (A, B) traps θ with probability 1 − α.

Let's try to dig deeper into some of the components of the definition.

<span style="color:purple"> Cn = (A, B) where A = A(X1,...,Xn) and B = B(X1,...,Xn) </span>

We define the interval in terms of A and B, which *are both functions of the samples (X1,...,Xn)* we take from the unknown distribution.  


<span style="color:purple"> 1-α confidence interval <span>

It is important to note that we can construct many confidence intervals. The degree of certainty that the interval has is quantified through the number 1-α. 
For example, if I am 5% certain in my estimate, weeeeell this is probably not a very good estimate 😅.  
Some of the usual values that α takes are 5% (confidence interval 1-α=95%) or 2.5% (confidence interval 1-α=97.5%).

### How does one construct a confidence interval?

We have established so far that in order to construct a confidence interval we need to decide on the level of confidence (1-α) and then calculate the upper and lower limits based on some function of the samples of the unknown distribution. But which function(s) should one take? Let's have a look through an example. 

#### Uniform distribution [θ-1/2,θ+1/2]


```r
xvals <- data.frame(x = c(0, 1)) #Range for x-values
  
ggplot(xvals) +
  geom_segment(aes(x = 0, xend=1, y=1, yend=1), size = 1, color="blue") +
  geom_segment(aes(x = 0, xend=0, y=0, yend=1), size = 1, color="blue") +
  geom_segment(aes(x = 1, xend=1, y=0, yend=1), size = 1, color="blue") +
  scale_y_continuous(breaks = c(0, 1), labels= c("0", "1"), limits = c(0,1.25)) + 
  scale_x_continuous(breaks = c(0, 1), labels = c("\u03B8 - 1/2", "\u03B8 + 1/2"), limits = c(-0.25,1.25)) +
  labs(title = "Uniform distribution with unknown \u03B8") + 
  theme(panel.grid.minor = element_blank(),
        axis.title = element_blank(),
        plot.title = element_text(hjust = 0.5))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />


We have the following scenario, a uniform distribution with an unknown parameter Θ. We can obtain a number of samples from the distribution: $$X_1, X_2, ..., X_n \thicksim U[\theta-1/2,\theta+1/2]\ i.i.d. $$
Furthermore, since this is a uniform we know:
$$ E[X] = E[X_1] = ... = E[X_n] = \frac {(\theta-1/2+\theta+1/2)} 2 = \theta $$ 
$$ Var(X)=Var(X_1)=...=Var(X_n) = \frac {(\theta+1/2-(\theta-1/2))^2} {12} = \frac {1} {12} $$
In this scenario the unknown around which we would like to create a confidence interval is the value θ which is the right end of the of the uniform. We will also pick a 95% confidence interval and therefore we are trying to find a, b such that:

$$ P_{\theta}(b \ge \theta \ge a) ≥ 0.95 , \forall θ ∈ Θ$$

It needs to be stressed again that *a and b are not values they are functions of the samples* obtained from the original unknown distribution, which is why we can make probabilistic statements like the one above. 

### Step 1: Find a (good!) estimator whose distribution we can calculate

We need to find a good estimator for θ (see my previous post about [estimators](https://georgekan.com/blog/2021-12-12-estimators/) for more details) and we need to make sure that we have information about the distribution of this estimator (either directly or asymptotically) as this is crucial in order to make probabilistic statements. 

Since E[X] = Θ, a good estimator for the expectation is the sample mean. The sample mean is an unbiased estimator, it is consistent based on the [Law of Large Numbers](https://georgekan.com/blog/2021-12-08-the-law-of-large-numbers/). 

*Can we say something about the distribution of the sample mean?* Yes! Based on the [Central Limit Theorem](https://georgekan.com/blog/2021-12-09-the-central-limit-theorem/) as the sample size grows larger the normalised sample mean follows a standard normal distribution.

So now we have an estimator for the unknown quantity θ and we would like to construct an interval around that estimator which will be the confidence interval we are looking for. 

$$ C = [\overline{X_n}-d, \overline{X_n}+d] $$

### Step 2: Calculate the interval limits based on the distribution of the estimator 

Based on the definition of the confidence interval and the results of step 1 we want to find d such that:


$$ P_{\theta}(\overline{X_n}+d \ge \theta \ge \overline{X_n}-d) ≥ 0.95 , \forall θ ∈ Θ $$
In order to find d, we need to make use of the CLT and in particular we need to rearrange the inequality inside the probability as follows:

$$ P_{\theta}(\overline{X_n}+d \ge \theta \ge \overline{X_n}-d) = P_{\theta}(d \ge \theta-\overline{X_n} \ge -d)=P_{\theta}(d \ge \overline{X_n}-\theta \ge -d) = P_{\theta}(\frac {\sqrt{n}d} {\sqrt{1/12}}\ge \sqrt{n} \frac {\overline{X_n}-\theta} {\sqrt{1/12}} \ge \frac {-\sqrt{n}d} {\sqrt{1/12}})$$

Just as a reminder from the CLT we know: $$ \sqrt{n} \frac {\overline{X_n}-\mu} {\sigma} \xrightarrow[n\rarr \infty]{(d)} N(0,1) $$

and this is exactly what we constructed above. We subtracted the expectation from the sample mean (θ), divided by the square root of the variance and multiplied by square root n.

Now we can substitute the middle part $$ \sqrt{n} \frac {\overline{X_n}-\theta} {\sqrt{1/12}} $$ by the standard normal distribution Ζ:

$$ P(\frac {\sqrt{n}d} {\sqrt{1/12}}\ge Z \ge \frac {-\sqrt{n}d} {\sqrt{1/12}}) $$

We would like this probability to be greater or equal to 1-α but in order to construct the tightest interval possible, we will solve for equality.

$$ P(\frac {\sqrt{n}d} {\sqrt{1/12}}\ge Z \ge \frac {-\sqrt{n}d} {\sqrt{1/12}}) = 1-α $$
To simplify calculations, we set $$ \frac {\sqrt{n}d} {\sqrt{1/12}} = e $$ and we have:
$$ P(e \ge Z \ge -e) = 1-α \hArr \Phi(e) - \Phi(-e)=1-α \hArr 2\Phi(e)-1=1-α \hArr \Phi(e)=1-\frac α 2 \hArr e=\Phi^{-1}(1-\frac α 2)$$






