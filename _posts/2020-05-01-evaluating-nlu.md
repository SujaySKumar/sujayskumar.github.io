---
layout: post
title: Empirical Evaluation of Current Natural Language Understanding (NLU)
date: '2020-05-01T15:35:06.387009'
author: Sujay S Kumar
tags: 
---

Evaluating language understanding is as difficult as elucidating the meaning of the word *"understanding"* itself. Before getting into evaluating computer models on language understanding, let's explore how we evaluate human *"understanding"* of natural language. How do we evaluate whether a person understands a particular language? Do we emphasize on the person's memory of the meanings of different words? Or do we emphasize on the person's ability to construct sequences of words/tokens that make sense to another human? Or is the ability of a person to read a passage and be able to answer questions on that passage considered to be a good indicator of his/her *"understanding"* of a language? 

It is apparent that trying to quantify a person's language understanding is a non-trivial pursuit. But, we have tried to quantify it nonetheless with multiple language proficiency tests across the world. The tests generally evaluate on multiple proxies (like sentence completions, fill-in-the-missing-word, passage question answering, essay writing) and the proficiency of a person is evaluated on how well he/she performs on ALL of these tasks. Important point to note here is that excelling on one section (task) and failing in another, points to a failure in language understanding.

We can draw similarity between the language proficiency tests and the natural language understanding evaluation in NLP. There are multiple datasets and benchmarks that are a proxy to the sections in tests. For example, we have the SQuaD (for passage question answering), GLUE (for next sentence predictions) etc.

Similar to the proficiency tests, we can consider a model to *"understand"* a language when it performs reasonably well across all the benchmarks without special training. We have seen multiple models achieving SOTA on individual benchmarks (surpassing human-levels). Does this mean we have achieved language understanding in computers? There are multiple ways we can evaluate generalizing capacity of any model. We have multiple datasets for the same task but on different domains, like having question answering datasets over multiple domains and multiple languages and any model is expected to perform well on all of them to prove understanding. While recent language models, trained on huge datasets, generalize well over multiple tasks, they still require significant fine-tuning to perform well in these tasks. There are questions raised as to whether this fine-tuning just overfits to the quirks of the individual benchmarks. Another approach is to train a model on ALL of these tasks simultaneously, proposed by McCann et al. [The Natural Language Decathlon: Multitask Learning as Question Answering](https://arxiv.org/pdf/1806.08730.pdf). They propose converting all tasks (summarization, translation, sentiment analysis etc) into a question answering problem. 

This blog post covers the current state of natural language understanding and explores where/if we are lacking and is a review of the Dani Yogatama et al. [Learning and Evaluating General Linguistic Intelligence](https://arxiv.org/pdf/1901.11373.pdf).

In order to claim language understanding, a model must be evaluated on it's abilities to 

1. Deal with the full complexity of natural language across a variety of tasks.
2. Effectively store and reuse representations, combinatorial modules (e.g, which compose words into representations of phrases, sentences,and documents), and previously acquired linguistic knowledge to avoid catastrophic forgetting.
3. Adapt to new linguistic tasks in new environments with little experience (i.e., robustness to domain shifts)

Dani Yogatam et al., evaluate BERT (based on Transformer) and ELMo (based on recurrent networks) on their general lingustic understanding. The main categories of tasks evaluated against are:

1. **Reading Comprehension:** This is a question answering dataset. We have 3 datasets, SQuaD (constructed from Wikipedia), TriviaQA (written by trivia enthusiasts) and QuAC (where a student asks questions about a Wikipedia article and a teacher answers with a short excerpt from the article). These are the same tasks but with different domains and distributions

2. **Natural Language Inference:** This is a sentence pair classification problem. Given two sentences, we need to predict whether the two sentences ENTAIL, CONTRADICT, NEUTRAL each other. We have two variants of this task: MNLI (Multi Genre Natural Language Inference) and SNLI (Stanford Natural Language Inference)

### Results from Dani Yogatama et al.:

1. *On both SQuAD and MNLI, both models are able to approach their asymptotic errors after seeing approximately 40,000 training examples, a surprisingly high number of examples for models that rely on pretrained module*: We still need significant amount of training examples to perform well on a tasks even if the model is pretrained.

2. *Jointly training BERT on SQuAD and TriviaQA slightly improves final performance. The results show that pretraining on other datasets and tasks slightly improve performance in terms of final exact match and F1 score*: If you want your model to perform well on multiple domains with the same task, it is better to jointly train the model on both datasets.

3. *Our next set of experiments is to analyze generalization properties of existing models. We investigate whether our models overfit to a specific dataset (i.e., solving the dataset) it is trained on or whether they are able to learn general representations and modules (i.e., solving the task). We see that high-performing SQuAD models do not perform well on other datasets without being provided with training examples from these datasets. These results are not surprising since the examples come from different distributions, but they do highlight the fact that there is a substantial gap between learning a task and learning a dataset*: Just training these models on one dataset and expecting it to perform well on another dataset will not work. However, jointly training it on both datasets provide good results as mentioned in the previous point.

4. *Catastrophic Forgetting; An important characteristic of general linguistic intelligence models is the ability to store and reuse linguistic knowledge throughout their lifetime and avoid catastrophic forgetting. First,we consider a continual learning setup, where we train our best SQuAD-trained BERT and ELMo on a new task, TriviaQA or MNLI. the performance on both models onthe first supervised task they are trained on (SQuAD) rapidly degrades.*: Taking a SOTA model from one task, training it on another task will degrade it's performance in the first task. This indicates that the model parameter updates from the second task is not compatible with the pre-trained model parameters for the first task. The model tends to forget what is has been trained for in the first place. This indicates that the model hasn't achieved generalized language understanding and has merely fit to the task (and dataset) it has trained on.

tl;dr We still have a long way to reach generalized language understanding in computers even though we are achieving SOTA in each task.  
