---
title: Introduction to hypothesis testing
author: George Kanellos
date: '2022-01-23'
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

<span style="color:green"> Welcome to another post in my statistics series!</span>  
So far in this series we have been obtaining weapons ⚔️ in our arsenal in order to make estimations and establish the reliability of those estimations. In this post we are going to discuss hypothesis testing which is an area of statistics that makes use of all this previously acquired knowledge.

### Definition

***Hypothesis testing is the process of comparing two conflicting hypotheses, the null hypothesis (H0) and the alternative hypothesis (H1) to draw conclusions about the data.***  

As always, statistics concepts become clearer through examples so let's pick one for hypothesis testing.  

![Coin toss](steve-smith-Zvvu4zRKijE-unsplash.jpg)
Imagine you are given a coin (ok no awards for originality here) and you have to assess whether it is biased. We have seen in previous posts how to turn this question into a statistical model through the Bernoulli distribution. By assigning heads to the outcome of 1 and tails to the outcome of 0 we can model the outcome of a coin toss as a Bernoulli distribution with parameter p. We have: $$ X \thicksim Ber(p) $$

What are the two hypotheses that we want to compare? 

* Null hypothesis (H0): **The coin is unbiased -> p = 0.5**
* Alternative hypothesis (H1): **The coin is biased -> p <> 0.5**

It is important to note that the **two hypotheses are not symmetric**.  
In particular, what we are looking for is whether we can find enough evidence in order reject the null hypothesis in favor of the alternative hypothesis or we fail to reject the null hypothesis. In practice, this means that we can never say that the null hypothesis is true, the only thing we can say in hypothesis testing is whether we reject the null hypothesis or we fail to reject it.

A very useful analogy from the "All of statistics" book is to imagine hypothesis testing as a trial. The null hypothesis is that the defendant is innocent and the burden of proof is on the prosecution to provide evidence for the contrary. The analogy is even stronger if you consider that the verdict in trials is one of the two:
1. Guilty <=> Sufficient evidence to reject the null hypothesis in favor of the alternative one
2. Not guilty <=> Insufficient evidence for rejecting the null hypothesis. <span style="color:red"> Does not mean that the defendant is innocent, nor that the null hypothesis is true!! </span>


### Test categories

Depending on how we define the hypotheses that we would like to test, they fall into the following categories:


#### One sample vs Two sample tests

Tests that aim to draw conclusions about a single population by comparing a sample from it against a constant are called one sample tests.  
Tests that aim to draw conclusions about the differences between two populations by comparing samples from both are called two sample tests.    

Examples: 

<span style="color:purple">1. Is the average waiting time in train station A greater than the average waiting time in train station B?</span>

H0: Waiting time A <= Waiting time B  
H1: Waiting time A > Waiting time B

**Two sample test** as we need to draw samples from two populations to compare, one from train station A and one from train station B.

<span style="color:purple">2. Is this coin biased?</span>

H0: Coin is not biased  
H1: Coin is biased

**One sample test** as we only need to work with one population.

<span style="color:purple">3. Do the patients who took the drug have lower blood pressure compared to the ones who took the placebo?</span>

H0: Blood pressure drug patients = Blood pressure placebo patients  
H1: Blood pressure drug patients < Blood pressure placebo patients

**Two sample test** as we need to compare the population of the drug patients against the population of the placebo patients.

<span style="color:purple">4. Does the duration of commercials in the cinema exceed 30 minutes?</span>

H0: Duration of commercials in the cinema <= 30  
H1: Duration of commercials in the cinema > 30

**One sample test** as we need to draw samples from a single population to compare a constant.

#### Two sided vs One sided tests

The following definitions apply one to One sample tests, so tests when we compare a sample against a certain constant.

Tests where the alternative hypothesis can take values either greater or smaller than the constant but not both hypothesis are called one sided tests.  
Tests where the alternative hypothesis can take values both greater and smaller than the constant are called two sided tests.

Coming back to our original coin toss example, let's see a couple of examples:

<span style="color:purple">1. Is the coin biased?</span>

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />
For this type of test we can see that the alternative hypothesis can be either greater or smaller than the constant 0.5. Therefore **this is a two sided test**.

<span style="color:purple">2. Is the coin more likely to come heads?</span>

H0: The probability that the coin comes heads is less than or equal to 0.5  
H1: The probability that the coin comes heads is greater than 0.5


<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

In this scenario the alternative hypothesis can only be greater than the constant 0.5 therefore **this is a one sided test**.






Type A lamps last longer than type B lamps
Commercial time in the cinema duration exceeds 30 minutes







