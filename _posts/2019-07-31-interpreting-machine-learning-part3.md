---
layout: post
title: Interpreting Machine Learning Models - Part 3
date: '2019-07-31T15:35:06.387009'
author: Sujay S Kumar
tags: 
---

Link to start of the blog series: [Interpretable ML]({% post_url 2019-07-10-interpreting-machine-learning %})

## Type 2 : Model Agnostic Interpretation

In the previous blog post, we explored various inherently interpretable machine learning models. In this blog post, we will explore various methods of interpretation without any dependency on the *type* of ML model. 

Given an opportunity, we can stick with only inherently ML models. Unfortunately, we have access to innumerable other ML models which are much better than the inherently interpretable ML models. We cannot abandon the former in favor of the latter. Also, having methods to induce interpretability of ML models without relying on the type of model allows us, as developers, to experiment with any number of variations of models without sacrificing interpretability.

1. **Partial Dependency Plot (PDP)**

    In layman terms, this plot illustrates the correlation between a *feature* and *target*. It illustrates how the *target* variable changes with change in *feature* variable value. 
    
    This requires us to know something called as *marginalisation*. Assume we have 4 variables $$x,y,z$$ and we have a function $$f$$. This function $$f$$ can be represented as 
    
    $$f(x,y) = \int f(x,y,z) dz$$. 
    
    If $$z$$ was a discrete variable, then integration is replaced by the summation symbol. By integrating (or summing) over all values of $$z$$, we have *marginalised* the function $$f$$ over $$z$$ and now we get a relation between $$x$$, $$y$$ and $$f$$ (i.e $$f(x,y)$$) only without any dependency on $$z$$.
    
    This concept is utilised in PDP, where $$\text{set S}$$ is the set of all features that we are interested in and $$\text{set C}$$ is the set of all features that we are not interested in. 
    
    $$S \cup C = \text{All Features}$$. 
    
    By marginalising over the features in $$\text{set C}$$, we get the relation between $$\text{set S}$$ and the ML model.
    
    To illustrate, let us assume that the features are $$a$$, $$b$$, $$c$$, $$d$$ and the ML model is $$f$$.
    
    The output of the ML model is given by,
    
    $$y = f(a,b,c,d)$$
    
    Now, we would like to plot a PDP between $$a$$ and the ML model (i.e) we would like to know the how $$a$$ affects the model output.
    
    Therefore, marginalising over all the other features,
    
    $$ f(a) = \int f(a,b,c,d) db dc dd $$
    
    Now, we have the relation between $$f(a)$$ and $$f(a,b,c,d)$$. This is nothing but the PDP plot.
    
    This works for all numerical features. When it comes to categorical features, it becomes simpler because we just need to expand on all the combinations of the categorical features. For example, is an ML model relies on *"temperature"* and *"weather"* to predict water sales, we can just set the *"weather"* variable to *"summer"*, *"spring"*, *"autumn"* and *"winter"* and record the output of the ML model. Here, we have effectively marginalised over the *"weather"* variable.
    
    In PDP, we are assuming that there is no correlation between the features. If there is, this will lead to incorrect results.

2. **Individual Conditional Expectation (ICE)**

    PDP is a *global* method. It does not focus on single, individual instances. It takes all the instances and then plots the correlation. In ICE, we do the same thing for each individual instance. We take each instance and keep $$b$$, $$c$$, $$d$$ same and vary $$a$$ (by sampling from a grid or drawing from a distribution) and see how the output ($$f$$) changes. The average of ICE of all instances gives us PDP.
    
3. **Accumulated Local Effects (ALE)**

    ALEs are a better alternative to PDPs. We already know that PDPs have a serious flaw which manifests when the features are correlated. ALEs do not suffer from any of them. How does ALE do that? We know that in PDP, we marginalise over *ALL* the values of the unwanted features. If the features are correlated, we will end up with feature vectors that are unlikely to ever occur in real life. For example, in house price prediction, if we have number of rooms and square footage area as features and we want to find out how number of rooms affect the house price, we keep the number of rooms constant and vary the square footage. It can go from 20 sqft to 200 sqft. Having 1 room and 200 sqft is highly unlikely to occur and so is 10 rooms and 20 sqft. In ALE, we take a small window to marginalise over instead of *ALL* the values that the variable can take. For eg, if one example has 3 rooms and 30 sqft, we keep 3 rooms as constant and vary square footage to 29 - 31 sqft (and not 20 - 200 sqft).
