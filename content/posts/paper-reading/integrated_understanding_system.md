---
title: "[DL & CN] Placing language in an integrated understanding system: Next steps toward human-level performance in neural language models"
date: 2021-08-18T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: Integrated understanding system
    identifier: ius
    parent: PR
    weight: 10
math: true
---
> McClelland, James L., et al. "Placing language in an integrated understanding system: Next steps toward human-level performance in neural language models." Proceedings of the National Academy of Sciences 117.42 (2020): 25966-25974.

## Abstract
For humans, language is a part of a system for understanding and communication about situations. There are some domain-general principles of biological neural network:
- connection-based learning
- distributed representation 
- context-sensitive 
- mutual constraint satisfaction

What they propose: the organization of the brain's distributed understanding system, which includes **a fast learning system that addresses the memory problem**.

## Principles of Neural Computation
A certain principles in PDP(parallel distributed processing) framework is the idea that cognition depends on mutual constraint satisfaction. We can regard it as a learning process which can construct context-sensitive representations. And **BERT** can do it really well, depend on QBA model. 
> QBA model 上下文阴影词表征之间的相似性关系可以用来重建一个句子的句法结构描述。而这不需要structure build in.

As far as I know, BERT works in parallel, using mutual QBA simultaneously on all of the words in an input text block. But this block has limit size, indicating that we can not use all the context information. Humans appear to exploit past context and a limited window of subsequent context,suggesting a hybrid strategy.
> 推荐阅读：Z. Yang et al., “Xlnet: Generalized autoregressive pretraining for language understanding” in Advances in Neural Information Processing Systems, H. Wallach et al., Eds. (Curran Associates, Inc., Red Hook, NY, 2019), vol. 32, pp. 5753–5763.

## Language in an Integrated Understanding System
The author argues that part of the solution will come from treating language as **part of a larger system for understanding and communicating**, and the targets of understanding are **situations**. 
**Situations** are collections of entities, their properties and relations, and patterns of change in them. What it means to understand a situation is to construct a representation of it that captures aspects of the participating objects, their properties, relationships and interactions, and resulting outcomes.

## Model of Understanding
- Assumptiom: Only focus on concrete situations involving animate beings and physical objects.

![](/images/posts/paper/5.JPG)

- **Visual Subsystem**: subserves the formation of a visual representation of the given situation. 
- **Aduitory Subsystem:** subserves the formation of an auditory representation capturing the spatiotemporal structure of the co-occurring sopken language.
- **Object Subsystem**: an intermodal area, receiving visual, language, and other information aobut objects. This area is the hidden layer of an interactive, recurrent network with bidirectional connections to other layers representing different type of object properties.
- **Language Subsytem**: The understanding of microsituations depends jointly on this system and context system.
- **Context Subsystem**：There is a network of areas in the brain that capture the spatiotemporal context.

Within each subsystem, and between each connected pair of subsystems, the neurons are reciprocally interconnected via learning-dependent pathways, allowing **mutual constraint satisfaction** among all of the elements of each of the representation types.
### **MTL(Integrated System State)**
随机的新信息在很短的时间内出现，当前的brain system很难去处理并在未来任意时间去从记忆中使用。这一点正是这个单元想去解决的问题。MTL的功能就是支持新的任意关联的形成，将一个experience的elements链接在一起，including the objects and language encountered in a situation and the co-occurring spatiotemporal context.
> GPT-3当遇到新的context时，之前this word 的 representation 信息会被覆盖掉，相当于没有记忆机制。作者在原文中如此写道：Further research should explore whether augmenting a model like GPT-3 with an MTL-like memory would enable more human-like extension of a novel word encountered just once to a wider range of contexts.

## Implementation of Model
- **Input of model**: sequences of microsituation each consisting of **a picture-description(language)(PD)pair** grouped into scenes that in turn form story-length narratives, with the language conveyed by text rather than speech.
### Process
1. Each PD pair is processed by interacting object and language subsystems, receiving visual and text input at the same time.
2. Each subsystem must learn to restore missing or distorted elements by using mutual QBA as in BERT.
3. The context subsystem encodes a sequence of compressed represenation of the **previous PD pair** within the current scene.
4. Within the processing of a pair, the context system would engage in mutual QBA with the object and language subsystems, allowing the language and object subsystems to indirectly exploit information from past pairs within the scene.(下属的subsystem可以得到历史信息，这样以便于他们地道道更好的representation，实现所谓的mutual constraint satisfaction)
5. 一个具有学习过的连接权重的神经网络在处理完每个PD对之后，构建了对物体、语言和语境子系统的状态以及视觉和文本子系统的状态的可逆的还原描述。这个压缩向量被存储在MTL-like memory里。

## Enhancing Uderstanding by Using Reinforcement leaarning
和强化学习结合，从而在情境中去得到语言、图片信息，从而去做出相应的选择。并且这种选择经过一些训练可以学习为一种固定的模式，再得到类似情景的输入时就会去做相同的事情，有了认知的行为。

可以拓展阅读这几篇论文：
- K. M. Hermann et al., **Grounded language learning in a simulated 3D world**. arXiv:1706.06551 (20 June 2017).
- R. Das, M. Zaheer, S. Reddy, A. McCallum, **“Question answering on knowledge bases and text using universal schema and memory networks”** in Proceedings of the 55th Annual Meeting of the Association for Computational Linguistic, R. Barzilay, M.-Y. Kan, Eds. (Association for Computational Linguistics, Stroudsburg, PA, 2017), pp. 358–365.
- D. S. Chaplot, K. M. Sathyendra, R. K. Pasumarthi, D. Rajagopal, R. Salakhutdinov, **“Gated-attention architectures for task-oriented language grounding”** in Proceedings of the Thirty-Second AAAI Conference on Artificial Intelligence, (AAAI-18), the 30th Innovative Applications of Artificial Intelligence (IAAI-18), and the 8th AAAI Symposium on Educational Advances in Artificial Intelligence (EAAI-18), S. A. McIlraith, K. Q. Weinberger, Eds. (AAAI Press, Cambridge, MA, 2018), pp. 2819–2826.
- J. Oh, S. Singh, H. Lee, P. Kohli, **“Zero-shot task generalization with multi-task deep reinforcement learning”** in Proceedings of the 34th International Conference on Machine Learning-Volume 70, D. Precup, Y. W. Teh, Eds. (JMLR.org, 2017), pp. 2661–2670.
- F. Hill et al., **“Environmental drivers of systematicity and generalization in a situated agent”** in International Conference on Learning Representations (ICLR, 2020).