---
title: "Neuroscience Basic Knowledge"
date: 2021-08-09T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: Neuroscience Basic Knowledge
    identifier: NBK
    parent: NC
    weight: 10
math: true
---
## The LIF model
> LIF: Leaky Integrated-and -Fire
#### membrane equation
$$
\begin{align*}
\\
&\tau_m\\,\frac{d}{dt}\\,V(t) = E_{L} - V(t) + R\\,I(t) &\text{if }\quad V(t) \leq V_{th} \\\\
\\\\
&V(t) = V_{reset} &\text{otherwise}\\
\\
\end{align*}
$$

where $V(t)$ is the membrane potential(膜电势), $\tau_m$ is the membrane time constant, $E_{L}$ is the leak potential, $R$ is the membrane resistance, $I(t)$ is the synaptic input current, $V_{th}$ is the firing threshold, and $V_{reset}$ is the reset voltage. We can also write $V_m$ for membrane potential, which is more convenient for plot labels.

The membrane equation describes the time evolution of membrane potential $V(t)$ in response to synaptic input and leaking of charge across the cell membrane. This is an *ordinary differential equation (ODE)*.

## The Mammalian Visual System
### 1. Structure
![](/images/posts/NC/2.JPG)

- Estimating receptive fields: spike-triggered averaging(STA)

