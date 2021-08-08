---
title: "[BIC] A brain-inspired computational model for spatio-temporal information processing"
date: 2021-07-20T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: [BIC] reservoir decision making network
    identifier: rdmn
    parent: PR
    weight: 10
math: true
---
## Abstract
- **Current method**: Explicit feature extraction, which requires lots of labeled data.
- **Novel brain-inspired computational model**: 
  - **Reservoir Decision-making Network (RDMN)**
  - A reservoir model: projects *complex spatio-temporal patterns* into *spatially separated neural representations* via its **recurrent dynamics**. (regarded it as SVM)
  - A decision-making model: reads out neural representations via integrating information over time.
- **Tasks**: 
  - Looming pattern discrimination
  - Gait recognition
  - Event-based gait recognition

## The model
### Overview
- **Summary**: A spatio-temporal pattern is first processed by the reservoir module and then read out by the decision-making module.
### The decision-making model
> **Summary**: The decision-making model consists of several **competing neurons**, with each of them representing **one category**(pattern). Each decision-making neuron receives inputs from the reservoir module and they compete with each other via **mutual inhibition**, with the winner reporting the recognition result.

- **The dynamics of the module**:
$$
\begin{equation}
x_i(t) = J_Es_i+\sum_{j\neq i}^{N_{dm}}J_Ms_j+I_i
\end{equation} \\
\begin{equation}
r_i(t) = \frac{\beta}{\gamma}\ln{\left[ 1+ exp\left( \frac{x_i - \theta}{\alpha}\right)\right]}
\end{equation} \\ 
\begin{equation}
\tau_s \frac{ds_i}{dt} = - s_i + \gamma(1-s_i)r_i
\end{equation}
$$
Eq.(3) describes the slow dynamics of the synaptic current due to the activity-dependent NMDA receptors.

- **Parameters**:
    - $x_i$: synaptic inputs received by the $i$th neuron.
    - $r_i$: neural activities.
    - $s_i$: the synapic current due to NMDA receptors.
    - $J_E$: represents the excitatory interactions between neurons encoding the same category.
    - $J_M$: indicates mutual inhibition.
    - $I_i$: is the feedforward input from the reservoir module, whose form is optimized through learning.

- **The mechanism of decision-making**
  - **Three types of stationary state**
    - **LAS**: Low active state, all neurons are at the same low-level activity
    - **DMS**: Decision-making state, in which one neuron is at high activity and the other at low activity.
    - **EAS**: Explosively active state, in which all neurons are at the same high-level activity.
    Apparently, only the parameter regime for DMS is suitable for decision-making. The optimal region is at the bifurcation boundary between LAS and DMS, which is called **DM-boundary**.

论文在后续对参数的选择进行了阐述，对于decision-making model而言，参数是固定的，也就是无需训练的，它的参数选择是人为选择的。一般是平衡了激活速度和准确率的中间平衡点作为参数选择。
### The reservoir model
> **Summary**: The reservoir module consists of several **forwardly connected layers**, with each layer having a large number of **recurrently connected neurons**.
- **Function**
  The decision-making model is not enough for discriminating complex spatio-temporal patterns, but the reservoir model could map different spatio-temporal patterns into **spatially separated neural activities**, so that the decision-making module can read out them. 

- **Structure of model**
  - Consists of $L$ forwardly connected layers, and neurons in each layer are connected recurrently.
  - Only Layer 1 receives the external input.
  -  


