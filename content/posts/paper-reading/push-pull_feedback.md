---
title: "[BIC] Push-pull Feedback Implements Hierarchical Information Retrieval Efficiently"
date: 2021-07-19T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: Push-pull feedback
    identifier: ppf
    parent: PR
    weight: 10
math: true
---
## Abstract
- In addition to feedforward connections, there exist abundant **feedback connections** in a neural pathway.
- This paper investigate the role of feedback in **hierarchical information retrieval**.
- a **hierarchical network** storing the **hierarchical categorical information of objects**:
  - **Mechanism**: information retrieval goes from **rough to fine**, aided by **dynamical** push-pull feedback from higher to lower layers. 
  - **Function**: 我们阐明，最佳反馈应该是动态的，随着时间的推移从正（推）到负（拉）而变化，它们分别抑制了来自不同类别和相同类别的模式关联所带来的干扰。   

## **A model for Hierarchical Information Representation**


