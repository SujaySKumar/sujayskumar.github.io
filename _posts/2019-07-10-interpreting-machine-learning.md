---
layout: post
title: Interpreting Machine Learning Models - Introduction
date: '2019-07-10T12:35:38.545715'
author: Sujay S Kumar
tags: 
---

One of the main impediments to the wide adoption of machine learning, especially deep learning, in critical (and commercial) applications is the apparent lack of trust accorded to these machine learning applications. This distrust mainly stems from the inability to reason about the outputs spit out by these models. This phenomenon is not just relegated to those who are outside the machine learning domain either. Even seasoned machine learning practitioners are flummoxed by the apparent failings of machine learning models. In fact, I would go so far as to say that longer you tinker around machine learning, more skeptical you become of its' abilities. 

Extensive research and development has been done in machine learning domain in the past decade and almost all of it has concentrated on achieving that elusive 100% accuracy (or whatever other metrics are used) and most of it were still in the research labs, with nary a thought given to the issue of how exactly this would be applied in the real world. With billions of dollars being poured into this domain and after years in research labs, it was time to bring this technology out in the open for commercial use. This was when the true drawback of integrating machine learning into our everyday lives became apparent. It is not so easy to trust a machine learning model.

If we delve deeper into why it is hard for us to trust a machine learning model, it helps to look into how we handle decisions taken by others that affect us. We, as humans, tend to give more importance to how a decision was arrived at rather than the decision itself. Taking an advisor-investor relationship as an example, if the advisor recommends a particular stock to the investor, he needs to explain why he came to that decision. Even if the investor is not as savvy as the advisor and might not grasp all the decision making factors of the advisor, he will be wary of investing without any type of explanation. And if the advisor insists on not providing any explanation, it would erode the trust between them. Similarly, all the machine learning models are "decision-making" models and we need to be able to "see" how a model arrived at an output even if it doesn't make complete sense to us.

Another reason why we have so little trust in machine learning models is because of the paucity of understanding in how exactly these models interpret the real world. Even if we have a state-of-the-art model that achieves 99.9% accuracy, we do not know why it fails in the other 0.1%. If we knew, we would fix it and achieve 100%. We cannot trust any machine learning output completely because we do not know when it is going to fail. This drawback became apparent with the introduction of adversarial networks and their runaway success in breaking almost all the state-of-the-art models at the time. Turns out, we do not exactly know how an object detector model exactly detects an object even though we know enough to push it to 99.9%.

Now that we are faced with the reality of deploying these models in the real world and asking people to use them and consequently trust them (sometimes with our lives, as is the case in self driving automobiles), we have to invest more in interpreting the outputs of these models. We are seeing more and more research being done on interpretability of machine learning models in recent times.  This is not cure-all for widespread adoption of machine learning, but it is definitely a good starting point. 

The purpose of this blog series is to explore some of the ways we can achieve machine learning interpretability and keep this in mind before developing any new machine learning applications.

Resources:
[Interpretable ML Book](https://christophm.github.io/interpretable-ml-book/)
