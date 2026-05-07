---
title: "Lecture: Are People Still Smarter than Machines?"
date: 2021-08-17T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: Lecture James McClleland
    identifier: L2
    parent: LL
    weight: 10
math: true
---

## How can a neural network learn to do something cognitively interesting?
- The Biologically Inspired Approach
  - If neuron A participates in firing neuron B, strengthen the connection from A to B
- The Optimization Approach 
  - Adjust each connection to minimize the network's deviation from a desired output
- **Backpropogation algorithm** became the basis of sbsequent research in the **PDP framework**, as well as almost all of Deep Learning

## Their Early Success of the Approach
- Models that could learn to read words and generalize to pronounceable non-words, capturing human-like response patterns: they refect **statistical patterns and similarity relationships**
- Models that captured **many aspects of human semantic cognition** and the disintegration of human semantic abilities resulting from neurodegenerative disease
- Models that showed in principle how aspects of language knowledge could be captured in a simple artificial neural network

## Nowadays
- AlexNet, ResNet and so on.
- Transformer
- Pre-trained model
- Alpha Zero

## What changed?
- The scale of computing and the size of the available data sets
- Neural network architecture innovations have been essential as well

## Toward human level language understanding
- Current language model just exploit **QBA(Query-based Attention)** over a finite window of text to predict missing or upcoming words
- Human exploit language and other sources of input such as vision to form a representation of situation they are witnessing and hearing about
- They can also exploit information from the indefinite past
- The lecturer has recently proposed using mutual QBA within the neuroscience-informed architecture at below to address these human capabilities
  ![](/images/posts/life/1.JPG)

  Each oval in the neocortex denotes a BERT-like QBA system mutually querying each of the others. All query previous system states stored in the MTL system, viewed as similar to the external memory in the DNC

> Placing language in an integrated understanding system. PNAS. 10.1073/pnas.1910416117

## Flaws of Models Today
Neural networks require massive data sets, snd show poor out-of-sample generalization. 

## Thoughts of Cognitive Abilities
Fodor and others may be right that advanced cognitive abilities depend on the ability to reason by means of the application of category- and structure-sensitive rules.

However, the ability to reason in this way may be an ***acquired*** human ability, rathrer than an inherent trait of the human mind.

## What will it take to build machines that can exploit intuition and acquire systematic cognitive abilities?
We need to add to this by teaching neural networks to engage in discourse as people do, including teaching them to:
- Follow instructions to carry out specified tasks
- Adopt specified goals and/or subgoals
- Conform to specified constraints to allowable actions
- Make targeted corrections to their behavior
- Understand and provide explanations
- Connect language and symbolic expressions to an underlying conceptual framework
- Reason within the framework rather than simply manipulate symbols