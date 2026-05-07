---
title: "KNN"
date: 2021-04-21T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: KNN
    identifier: KNN
    parent: machine-learning
    weight: 10
math: true
---

## 原理

K近邻算法简单、直观，大致步骤为：
- 输入：训练集
$$T = \{(x_1,y_1), (x_2, y_2), \dots, (x_N, y_N)\}$$
- 输出：
  - 根据给定的距离度量，在训练集T中找出与$x$最近邻的$k$个点，涵盖这$k$个点的邻域记作$N_K(x)$；
  - 找出最近邻的$k$个点的一个方法是搜索$kd$树；
  - 在$N_k(x)$中根据分类决策规则（多数表决）决定$x$的类别$y$:
    $$y = arg \max_{c_j} \sum_{x_i \in N_k(x)}I(y_i=c_j)$$ 

### kd树
