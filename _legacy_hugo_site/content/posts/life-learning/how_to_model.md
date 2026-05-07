---
title: "Approaching Scientific Question & How to Model"
date: 2021-08-29T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: how to model
    identifier: htm
    parent: LL
    weight: 10
math: true
------

## Marr’s 3 levels of analysis
Brain: hierarchy of complexities
- **Computational level - 1** 
  - What is the objective of the system?
  - How close is it to optimal?
- **Algorithmic level - 2**
  - What are the data structures?
  - What are the approximations?
  - What is the runtime?
- **Implementation level - 3**
  - What is the hardware? 
    - Neurons? Synapses? Molecules?

##  Diversity of modeling goals
1. **Useful:** good at solving real-world problems?
2. **Normative**: provide the optimal solutions to problems that exist in the real world?
3. **Clinically relevant:** produce insights relevant for developing or evaluating clinical interventions?
4. **Inspire experiments**: change the way we think about a problem, raising interesting new hypotheses & experiments?
5. **Microscopic realism:** describe the microscopic properties of the brain?
6. **Macroscopic realism:** describe properties of brain areas and networks?
7. **Behavioral realism:** can faithfully describe and explain behavioral phenomena?
8. **Representational:** use representations of info that are similar to representations in the brain?
9. **Compact:** can be succinctly expressed in math or code, making them easy for humans to understand?
10. **Analytically tractable:** understandable through equations instead of numerical simulations, therefore generalizable?
11. **Interpretable?**
12. **Beauty:** symmetrical, balanced, or resonate well with the way we think?

## 1. Asking your own question
- What exact aspect of data needs modeling?
    - Answer this question clearly and precisely! Otherwise you will get lost (almost guaranteed)
    - Write everything down!
    - Also identify aspects of data that you do not want to address (yet)
- Define an evaluation method!
    - How will you know your modeling is good?
    - E.g. comparison to specific data (quantitative method of comparison?)
- For computational models: think of an experiment that could test your model
    - You essentially want your model to interface with this experiment, i.e. you want to simulate this experiment

