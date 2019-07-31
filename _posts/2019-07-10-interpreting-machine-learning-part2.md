---
layout: post
title: Interpreting Machine Learning Models - Part 2
date: '2019-07-15T16:47:08.948084'
author: Sujay S Kumar
tags: 
---

Link to start of the blog series: [Interpretable ML]({% post_url 2019-07-10-interpreting-machine-learning %})

## Type 1 : Interpretable Machine Learning Models

In this post, we will be going over some of the machine learning models that can be interpreted intrinsically. This will not be an in-depth review of the models themselves, rather an exploration of how these models lend themselves to interpretability.

1. **Linear Regression**

    A linear regression is one of the simpler (and widely used) ML models for regression. Let's explore how we can interpret a linear regression model and justify whether it is indeed an intrinsically interpretable ML model. 
    
    Linear regression is accomplished with a hyperplane that splits the vector space into two and can be expressed using the following equation. 

    $$y=\beta_{0}+\beta_{1}x_{1}+\ldots+\beta_{p}x_{p}+\epsilon\$$

    As can be seen from the above equation, each feature is assigned a learned parameter which estimates the relative importance given to that particular feature. Since it is also a linear equation, humans can easily comprehend the degree to which a feature affects the output compared to others.

    Depending on the type of feature $$x_{k}$$, we can interpret the corresponding weight $$\beta_{k}$$ as follows:

    1. If $$x_{k}$$ is a numerical feature, then every unit change in $$x_{k}$$ results in $$\beta_{k}$$ change in the output $$y$$, *given all other features remain constant*.

    2. If $$x_{k}$$ is a categorical feature, depending on the encoding method used, changing $$x_{k}$$ from the reference category to the other category results in $$\beta_{k}$$ change in the output $$y$$, *given all other features remain constant*. Determining this *reference category* is a very tricky business and hence this type of interpretation is tricky.

    3. If $$x_{k}$$ is a binary feature, presence of $$x_{k}$$ results in $$\beta_{k}$$ change in the output $$y$$, *given all other features remain constant*.

    If you have noticed, every interpretation has a condition associated i.e *given all other features remain constant*. Encountering this situation where only a certain feature changes while all other features remain constant is highly unlikely. This is one of the disadvantages of using these models for interpretability (along with inherent drawbacks of linear regression itself like features should be independent and follow normal distribution).

2. **Logistic Regression**

    Logistic regression is the most commonly used model for classification. Let's explore how logistic regression can be considered an intrinsically interpretable ML model.
    
    The logical jump from a linear regression to logistic regression is pretty straight-forward. Here, we pass the output of the linear regression through a non-linear function to get the probabilities.
    
    The linear regression equation is,
    
    $$\hat{y}^{(i)}=\beta_{0}+\beta_{1}x^{(i)}_{1}+\ldots+\beta_{p}x^{(i)}_{p}$$
    
    The logistic regression equation is,
    
    $$P(y^{(i)}=1)=\frac{1}{1+exp(-(\beta_{0}+\beta_{1}x^{(i)}_{1}+\ldots+\beta_{p}x^{(i)}_{p}))}$$
    
    Now that the simple linear equation has been passed through a non-linear function, it becomes a bit difficult for us to interpret the learned weights of logistic regression. So, let us play around with the equation till it is more palatable.
    
    Let us get the linear term on the right hand side,
    
    $$log\left(\frac{P(y=1)}{1-P(y=1)}\right)=log\left(\frac{P(y=1)}{P(y=0)}\right)=\beta_{0}+\beta_{1}x_{1}+\ldots+\beta_{p}x_{p}$$
    
    On the left hand side (LHS), we have the ratio of probability of the event happening to the probability of the event not happening (we can call this "the odds"). "log()" of this can be called as the "log odds".
    
    Applying exp() on both sides, we get,
    
    $$\frac{P(y=1)}{1-P(y=1)}=odds=exp\left(\beta_{0}+\beta_{1}x_{1}+\ldots+\beta_{p}x_{p}\right)$$
    
    Although this equation makes more sense than the previous ones, it is still not that interpretable. So, let us think about it in this way. What effect would changing $$x_{j}$$ by $$1$$ have on the prediction probability?
    
    Taking the ratio,
    
    $$\frac{odds_{x_j+1}}{odds}=\frac{exp\left(\beta_{0}+\beta_{1}x_{1}+\ldots+\beta_{j}(x_{j}+1)+\ldots+\beta_{p}x_{p}\right)}{exp\left(\beta_{0}+\beta_{1}x_{1}+\ldots+\beta_{j}x_{j}+\ldots+\beta_{p}x_{p}\right)}$$
    
    Since, $$\frac{exp(a)}{exp(b)}=exp(a-b)$$, we can simplify further to get,
    
    $$\frac{odds_{x_j+1}}{odds}=exp\left(\beta_{j}(x_{j}+1)-\beta_{j}x_{j}\right)=exp\left(\beta_j\right)$$
    
    From the above equation, it becomes pretty clear that a unit change in a feature changes the odds ratio by a factor of $$\exp(\beta_j)$$.

3. **Decision Trees**

    Now, decision trees are one of the most understandable machine learning models out there. This is partly because we, as humans, tend to follow this structure when making decisions. 
    
    In simpler terms, a decision tree can be explained as follows: Starting from the root node, you go to the next nodes and the edges tell you which subsets you are looking at. Once you reach the leaf node, the node tells you the predicted outcome. All the edges are connected by "AND". If feature $$x$$ is [smaller/bigger] than threshold $$c$$ AND … then the predicted outcome is the mean value of $$y$$ of the instances in that node.
    
    - *Feature Importance:* The feature that gives us the most reduction in entropy (or variance) is the most important feature. It is beautiful how this can be expressed both mathematically and intuitively.
    
    - *Interpreting a single prediction:* A single prediction can be interpreted by visualising exactly the decision path taken to arrive at the output. We can observe each node it went through, the thresholds of these nodes as well as the ultimate leaf node it was assigned to. Since a particular feature can be found any number of times in the tree, we can also estimate how important a feature was in predicting the outcome of this particular prediction.
