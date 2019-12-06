---
layout: post
title: NLP Summary - Jacob Eisenstein
date: '2019-12-01T15:35:06.387009'
author: Sujay S Kumar
tags: 
---

This is a running summary of the [NLP textbook](https://github.com/jacobeisenstein/gt-nlp-class/blob/master/notes/eisenstein-nlp-notes.pdf) by [Jacob Eisenstein](https://jacobeisenstein.github.io/). This blog post adds a bit of my personal take on his ideas.

## Three main themes in NLP:

### 1. Learning and Knowledge
- On one extreme, some advocate for free form ML modeling. Feed raw textual inputs to train end-to-end systems and train it to produce desired outputs. Given enough data, we can sufficiently train any network. There is no need for any feature engineering or domain knowledge.
- On the other extreme, we have advocates of transforming text into hierarchical structures (sub word units called morphemes, followed by POS tags, followed by tree-like parsing).
- End-to-end systems found success with the advent of deep learning innovations (including speech recognition). Reinforced by advances in CV.
- We can leverage linguistic structure in model building. We know that sentences are compositional: meaning of larger units are gradually constructed from the meanings of smaller constituents. [Dyer et al. 2016](https://arxiv.org/abs/1602.07776)
    
### 2. Search and Learning
- Most NLP problems can be written as:

    $$\hat{y} = \text{argmax }\psi(x,y;\theta)$$

    - $$x$$ is the input
    - $$y$$ is the output
    - $$\psi$$ is the scoring function
    - $$\theta$$ is the parameters of the model
    - $$\hat{y}$$ is the predicted output selected after maximising the scoring function

- Given this formulation, we can divide the problem into two:
    - **Search**: This module is for determining the the $$argmax$$ of the function $$\psi$$ and finding $$\hat{y}$$. Example: bottom-up dynamic programming and beam search.
    - **Learning**: This module is for training the model $$\theta$$. This is done through optimization algorithms.

- Why do we divide into these problems? We know that in its roots, we need to solve 2 problems, search and learning, which are individually solved already. Therefore, identify relevant algorithms for search, optimization and learning and use their combinations to solve your problem.
    
### 3. Relational, compositional, and distributional perspectives
- Any language element (word, phrase or a sentence) has multiple semantic relationships to other elements. This is captured in semantic ontologies such as WordNet. 
    
## Learning - Linear Text Classification

### 1. Naive Bayes with Bag-of-Words
- Maximum Likelihood Estimation
- Joint probability learning (generative model)
- Smoothing: Add 1 smoothing, Laplace smoothing
- Batch Learning

### 2. Perceptron
- Discriminative model
- Online model (weights are updated after each example), unlike Naive Bayes.

### 3. Loss Function
- A function which takes in the model weights $$\theta$$ and the input $$x$$ and the output is a positive number that indicates how bad/good the model is to this particular instance. The objective is to minimise the sum of losses across all training instances.
- `negative` of Maximum Log Likelihood can be a loss function.

### 4. Logistic Regression

## Learning - Non-Linear Text Classification

### Why neural networks for NLP?
- Rapid advances in Deep Learning.
- Word Embeddings
- GPU speeds >> CPU speeds
    
### Neural network hyper-parameter selection
- **Activation Functions**: Sigmoid, tanh, relU and leaky relU
- **Architecture**: You can either make the network wide or deep. This tradeoff is not well understood.
- Since sqaushing activations such as sigmoid and tanh bounds output to $$(0,1)$$, the gradients propagating to lower levels might be insignificant and lower layers stop learning (vanishing gradients). You can by-pass certain connections from a lower layer to a higher layer (residual networks and highway networks).
- Regularization and dropout to prevent over-fitting.
- Model initialization: tanh -> Uniform distribution (Glorot and Bengio, 2010), relU -> zero-mean Guassian distribution.
- Gradient clipping
- Batch normalization.
- Other optimization algorithms to improve stochastic gradient descent: Adagrad

### Convolutional Neural Networks
- Bag-of-Words model disregards word order. 
- A 1-D convolutional filter can be used to extract word order semantics.

### Linguistic Applications of Classification

1. Sentiment Classification
2. Word sense Disambiguation - Wordnet has sense annotations

### Things to note while designing a classifier

1. **Tokenization:** Most of the NLP constructs look at words as a single entity (like word embeddings). Tokenization might seem straightforward, but each tokenization technique has significant effect on the performance of the model. We cannot just rely on tokenizing on white-spaces. There are instances of hyphenated phrases (prize-winning, out-of-the-box etc) or punctuations (Ph.D, U.S etc). This is also highly language specific. For example, Malayalam, like German, can coalesce multiple words into a single word and cannot be reliably split into independent words. In such cases, we might have to train classifiers to predict if each character is a word boundary or not.
2. **Text Normalization:** One normalization technique is to lowercase every word (in English, at least). But, this might ignore subtleties important for classifications. *apple* and *Apple* have different meanings based on the case of the letter. This consideration is applications dependent and if it makes sense to lowercase everything for your application, then it should be done. There is also the cases of lemmas and stems in English. NLTK does a pretty good job of stemming and lemmatising most of the English words.
