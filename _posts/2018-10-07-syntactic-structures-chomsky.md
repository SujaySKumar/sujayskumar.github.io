---
layout: post
title: Syntactic Structures by Chomsky (Summary)
date: '2018-10-07T12:04:12.386254'
author: Sujay S Kumar
tags: 
---

This blog is a summary of the ideas outlined in Chomsky's [Syntactic Structures](http://www.linguist.univ-paris-diderot.fr/~edunbar/ling499b_spr12/readings/syntactic_structures.pdf).

### Table of Contents

1.  [Language](#org616a0ef)
    1.  [Grammar](#org26a5156)
    2.  [Phonemes and Morphemes](#org3d07247)
    3.  [Markov Processes for language generation](#org4728aad)
    4.  [Limitations of Phrase structure description](#org95e2509)
    5.  [Grammatical Transformation](#org7347b00)
    6.  [Procedure for formalizing computational linguistics](#org361b56c)
    7.  [Explanation power of linguistics](#orgc47260a)
    8.  [Syntax and Semantics](#org9dff614)

## Language

Any language `L` is considered to be a set of sentences. Each sentence is finite in length and is constructed out of a finite set of elements (words). A sentence can be a sequence of phonemes or letters in an alphabet.


<a id="org26a5156"></a>

### Grammar

Grammar has no relation to semantic meaning of a sentence. A sentence can be grammatically correct and not have any meaning. Since the probability of grammatically correct and incorrect sentences occurring in a corpus of text is highly dependent on the corpus, we cannot leverage statistics to find out if a sentence is grammatically correct or not. (*Although we know that this is not true anymore as we can build robust language models that care capable of probabilistically generating grammatically correct sentences*). 

**Conclusion**: Grammar is autonomous and independent of meaning, and that probabilistic models give no particular insight into some of the basic problems of syntactic structure.


<a id="org3d07247"></a>

### Phonemes and Morphemes

Phonemes are smallest elements in pronunciation. Morphemes are smallest elements in words.


<a id="org4728aad"></a>

### Markov Processes for language generation

Chomsky talks of a finite state machine (finite state grammar) that can be used for generation of sentences. He argues that since since English is a non-finite state language, we cannot use a finite state machine to generate English sentences.

1.  Finite state language

    `a[3]b[4]` is finite.

2.  Infinite state language

    `a*b*` is infinite.
    
    Similarly, the following demonstrates how English is also infinite.
    
    Suppose $$S_{1}$$, $$S_{2}$$ and $$S_{3}$$ are declarative sentences, we can construct the following sentences using grammar rules.
    
    >If $$S_{1}$$, then $$S_{2}$$.
    
    >Either $$S_{3}$$ or $$S_{4}$$.
    
    Each of the above sentence is also declarative and hence can be expanded infinitely.


<a id="org95e2509"></a>

### Limitations of Phrase structure description

1.  If there are two sentences of the form `Z + X + W` and `Z + Y + W`, we can use the conjunction `and` and construct a new sentence of the form `Z-X+ and +Y-W` (Only if `X` and `Y` are constituents)

    For eg,
    
    >the scene - of the movie - was in Chicago
    
    >the scene - of the play - was in Chicago
    
    >the scene - of the movie and of the play - was in Chicago
    
    
    The above assumption does not hold true in the following case where `X` and `Y` are not constituents
    
    >the - liner sailed down the - river
    
    >the - tugboat chugged up the - river
    
    >the - liner sailed down the and the tugboat chugged up the - river
    
    The limitation in this case is that unless `X` and `Y` are constituents, we cannot apply this grammar rule. But it is not possible to incorporate this rule in any 
    phrase structure grammar. *Why?* Because we need to know the actual form of the 2 sentences and if `X` and `Y` are constituents (for which we need to know their values).

2.  Auxilary Verbs

3.  Active passive relation

    *"John admires sincerity"* <-> *"sincerity admires John"*
    
    *"sincerity frightens John*" <-> *"John frightens sincerity"*
    
    These examples demonstrate that even though these sentences are not violating any grammar rules, they are semantically incorrect and hence pose limitations to the assumption that grammar does not depend on semantics. These limitations can be overcome by having extra rules (which cannot be incorporated within the grammar itself) over the grammar.


<a id="org7347b00"></a>

### Grammatical Transformation

1.  In order to overcome the above limitations, we can assume the grammar to have 3 levels of rules:

    1.  Phrase Structure
    
    2.  Transformational Structure
    
    3.  Morphophonemics
    
    Given a sentence, we first go through the *phrase structure grammar* to build a tree and get to the leaf nodes (terminal words).
    
    We then run through the *transformational structural rules* (both optional and obligatory rules) to transform the set of terminal nodes.
    
    We then apply the *morphophonemics* to arrive at the final sentence.
    
    
    ![img-three-step](/assets/syntactic_structure/three-step-grammar.png)

<a id="org361b56c"></a>

### Procedure for formalizing computational linguistics

![img](/assets/syntactic_structure/grammar-machine.png)

Of the 3 approaches outlined in the diagram above, Chomsky proposes to choose the one in which we can come to a reasonable solution to, hence evaluation. He says, generating (discovery) of the correct grammar given a data corpus is not feasible (*we know this is not true now, because we can build language models given a huge corpus*).


<a id="orgc47260a"></a>

### Explanation power of linguistics

Reason to have a separate representation for morphemes and phonemes is demonstrated here. The phoneme sequence */eneym/* can be understood ambiguously as both "*a name*" or a "*an aim*".

If our grammar is only a single level system dealing with only phonemes, we have no way of representing this ambiguity. Therefore we need a second morphological layer.


<a id="org9dff614"></a>

### Syntax and Semantics

The three levels of parsing of grammar gives us the ability to perform the following representations:

1. Represent sentences that can be understood in more than one way ambiguously.

2. Represent two sentences that are understood in a similar manner similarly on the transformational level.

Chomsky argues that there cannot be any relation between semantics and grammar i.e the burden of proof lies on the linguist claiming that the grammar should be dependent
on semantics.

Assertions supporting dependence of grammar on meaning:

1.  Two utterances are phonemically distinct if and only if they differ in meanings

    This is refuted because of *synonyms* (utterance tokens that are phonemically different but means the same thing) and *homonyms* (utterance tokens that are phonemically
    identical but differ in meaning)

2.  Morphemes are the smallest elements that have meanings

    This is refuted because morpheme such as *gl-* in "*gleam*", "*glimmer*" and "*glow*" does not carry any meaning in itself. 

3.  Grammatical sentences are those that have semantic significance

4.  `NP - VP` -> actor-action

    "*the fighting stopped*" has no actor-action relation

5.  `Verb - NP` -> action-goal or action-object of action

    "*I missed the train*" has no action-goal relation

6.  Active sentence and corresponding passive sentence are synonymous

    "*everyone in the room knows at least two languages*" is not synonymous to "*at least two languages are known by everyone in the room*"

