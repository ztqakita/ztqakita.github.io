---
title: "Optimization for Deep Learning"
date: 2020-07-25T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: Optimization for Deep Learning
    identifier: optimizer-for-DL
    parent: deep-learning
    weight: 10
math: true
---
> Some Notation: <br>
> $\theta_t$: model parameter at time step t <br>
> $\nabla$$L(\theta_t)$ or $g_t$: gradient at $\theta_t$, used to compute $\theta_{t+1}$ <br>
> $m_{t+1}$: momentum accumlated from time step 0 to time step t, which is used to compute $\theta_{t+1}$

## I. Adaptive Learning Rates
In gradient descent, we need to set the learning rate to converge properly and find the local minima. But sometimes it's difficult to find a proper value of the learning rate. Here are some methods to introduce which can automatically produce the adaptive learning rate based on the certain situation.

- **Popular & Simple Idea**: Reduce the learning rate by some factor every few epochs (eg. $\eta^t=\eta/\sqrt{t+1}$)
- **Adagard**: Divide the learning rate of each parameter by the ***root mean square of its previous derivatives***
![](/images/posts/DL/grad.JPG)
![](/images/posts/DL/adg.JPG)
When $\eta = \frac{\eta^t}{\sqrt{t+1}}$, we could derive that:
![](/images/posts/DL/adg2.JPG)

## II. Stochastic Gradient Descent
![](/images/posts/DL/sgd.JPG)
SGD update for each example, so it would converge more quickly. 

## III. Limitation of Gradient Descent
- Stuck at local minima ($\frac{\partial L}{\partial \omega}$ = 0)
- Stuck at saddle point ($\frac{\partial L}{\partial \omega}$= 0)
- **Very slow at plateau**: Actually this is the most serious limitation of Gradient Descent.

So there are other methods perform better than gradient descent.

## IV. New Optimizers
> notice: <br>
> on-line: one pair of ($x_t$, $\hat{y_t}$) at a time step <br>
> off-line: pour all ($x_t$, $\hat{y_t}$) into the model at every time step

### 1. SGD with Momentum (SGDM)
![](/images/posts/DL/sgdm.JPG)
Previous gradient will affect the next movement. $v^i$ is actually the weighted sum of all the previous gradient:
$\nabla L(\theta^0), \nabla L(\theta^1), \dots \nabla L(\theta^{i-1})$ <br>
Use SGDM could erase the limitation of $\nabla L(\theta) = 0$. 
For momentum, you could imagine it as physical inertia.
![](/images/posts/DL/sgdm2.JPG)

### 2. Root Mean Square Propagation (RMSProp)
This optimizer is based on **Adagrad**, but the main difference is the calculation of the denominator. The reason for this new calculation is to avoid the situation that ***the learning rate is too small from the first step***. In other words, exponential moving average(EMA) of squared gradients is not monotonically increasing.
![](/images/posts/DL/sgdm3.JPG)

### 3. Adam
Adam is an optimization algorithm that can be used instead of the classical stochastic gradient descent procedure to update network weights iterative based in training data.
Adam realizes the benefits of both AdaGrad and RMSProp.
![](/images/posts/DL/adam.JPG)

### 4. Adam vs SDGM
- Adam: fast training, large generalization gap, unstable
- SGDM: stable, little generalization gap, better convergence

### 5. Supplement
- ASMGrad (ICLR'18)
![](/images/posts/DL/asm.JPG)
- AdaBound (ICLR'19)
![](/images/posts/DL/adab.JPG)
    > note: Clip(x, lowerbound, upperbound)

- SWATS (arXiv'17)
Begin with Adam(fast), end with SGDM
![](/images/posts/DL/swats.JPG)
- RAdam (Adam with warmup, ICLR'20)
![](/images/posts/DL/radam.JPG)
- RAdam vs SWATS
![](/images/posts/DL/cmp.JPG)

To be continued ...