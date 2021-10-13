---
title: "[CN] Few-Shot Class-Incremental Learning"
date: 2021-09-23T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: Few-Shot Class-Incremental Learning
    identifier: FSCIL
    parent: PR
    weight: 10
math: true
---
> Xiaoyu Tao, Xiaopeng Hong, Xinyuan Chang, Songlin Dong, Xing Wei, Yihong Gong; Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2020, pp. 12183-12192

## Abstract
- ***FSCIL***: It requires CNN models to incrementally learn new
classes from **very few labelled samples**, without forgetting the previously learned ones.
- ***TOPIC***: Topology-preserving knowledge incrementer framework, which is proposed to **mitigates the forgetting of the old classes** by stabilizing NG’s topology and improves the representation learning for few-shot new classes by growing and adapting NG to new training samples.

## Few-Shot Class-Incremental Learning
- Task Design
  - a stream of training sets $D^{(1)}, D^{(2)}, \dots$, and for each training set $D^{(t)}$, $L^{(t)}$ is the set of classes of $D^{(t)}$.
  - 