---
layout: post
title: Common Sense Is Not So Common in NLP 
date: '2020-05-02T15:35:06.387009'
author: Sujay S Kumar
tags: 
---

One of the main drawbacks of any NLU neural model is it's lack of generalization. This topic has been explored extensively in the previous post [Empirical Evaluation of Current Natural Language Understanding (NLU)]({% post_url 2020-05-01-evaluating-nlu %}). To what can we attribute this lack of common-sense to?

The main reason for this brittleness is the fundamental lack of understanding that a model can gain from processing just text, irrespective of the amount of text it sees. For example, it is common-sense that keeping a closet door open is ok while keeping a refrigerator door open is not good. As humans, we can reason that keeping the refrigerator door open leads to spoiling of food inside since it is perishable while clothes are non-perishable and hence it is ok to keep the closet door open. But, this information is not usually written down anywhere and hence, it is difficult for a model to learn this reasoning.

Also, text is just one modal of information that humans interact with. Humans interact with the world through sight, smell and touch and this information is inherent in understanding. The NLU models lack this crucial exposure to other modalities (which form the root of common-sense). For example, we do not write down obvious things like the color of an elephant. Written text usually talk about elephants in terms of size, like *"big elephant with huge tuskers charged at the man"*, while *"dark grey elephant was spotted in Africa"* is practically non-existent. Nevertheless, since we are exposed to pictures of elephants, we know that elephants are usually dark grey in color. If a pre-trained model is asked to predict the color of an elephant, it will fail or might even say it is white since *"white elephant"* is a valid phrase that is used as a metaphor.

Another reason for lacking common-sense is that common-sense is just not written down. Consider the following example:

> S: The toy did not fit in the bag because it was too big. 
>
> Q: What does *"it"* refer to? 
>
> A: Toy

> S: The toy did not fit in the bag because it was too small.
>
> Q: What does *"it"* refer to?
>
> A: Bag

### In brief, why do models lack common-sense?

1. There is inherent bias when humans write things down. We do not tend to write down obvious things.
2. Common-sense is also not written down
3. The models are not exposed to other modalities (like images, audio or video).

### Incorporating common-sense into neural models:

1. Build a knowledge base, similar to WordNet. We can store specific common-sense information in this database. For example, an `Elephant` object can have the attribute `color: grey`.
2. Multi-modal learning: Sun et al. [VideoBERT](https://arxiv.org/pdf/1904.01766.pdf)
3. Human-in-the-loop training.

### Resources for common-sense reasoning

1. [Yejin Choi](https://homes.cs.washington.edu/~yejin/): Key researcher in the field of common reasoning. [Talk at NeurIPS 2019 LIRE workshop](https://drive.google.com/file/d/1-6YfnNDdbkXHoLVypfuHTqkUhjXjwgLW/view)
2. Maarten Sap et al., [ATOMIC: An Atlas of Machine Commonsense for If-Then Reasoning](http://arxiv.org/abs/1811.00146)
3. Antoine Bosselut et al., [COMET: Commonsense Transformers for Automatic Knowledge Graph Construction](http://arxiv.org/abs/1906.05317)
4. Keisuke Sakaguchi et al., [WinoGrande: An Adversarial Winograd Schema Challenge at Scale](http://arxiv.org/abs/1907.10641)

### Datasets

1. Winograd
2. Winogrande (AAAI 2020)
3. Physical IQA (AAAI 2020)
4. Social IQA (EMNLP 2019)
5. Cosmos QA (EMNLP 2019)
6. VCR: Visual Commonsense Reasoning (CVPR 2019)
7. Abductive Commonsense Reasoning (ICLR 2020)
8. TimeTravel: Counterfactual Reasoning (EMNLP 2019)
9. HellaSwag: Commonsense NLI (ACL 2019)
