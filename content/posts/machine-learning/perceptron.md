---
title: "Perceptron"
date: 2021-01-27T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: Perceptron
    identifier: Perceptron
    parent: machine-learning
    weight: 10
math: true
---
# 原理
> 参考：[统计学习方法|感知机原理剖析及实现](https://www.pkudodo.com/2018/11/18/1-4/)

- 输入：实例的特征向量
- 输出：实例的类别（二分类）
- 模型类别：判别模型
- 学习策略：基于误分类的损失函数，利用梯度下降法对损失函数进行极小化，求得感知机模型

