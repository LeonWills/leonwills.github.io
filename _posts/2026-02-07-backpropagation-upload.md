---
title: 深度学习基石：从计算图视角理解反向传播算法 (Backpropagation)
date: 2026-02-07 17:43:00 +0800
categories: [Machine Learning, Deep Learning]
tags: [Theory, Gradient Descent, Calculus, Computational Graph]
math: true 
img_path: /assets/img/posts/
---

在深度学习的训练过程中，我们常听到“梯度下降”这个词。如果说梯度下降是优化参数的“战略指导”，那么**反向传播（Backpropagation）**就是执行这一战略的“战术核心”。

它是如何将最终的预测误差（Loss），精准地分摊给网络中成千上万个权重的？本文将从**计算图（Computational Graph）**的角度，结合链式法则，推导反向传播的数学原理。

## 1. 前向传播与计算图

在理解反向传播之前，我们必须先建立**前向传播（Forward Propagation）**的概念。

如下图所示，这是一个典型的全连接神经网络。输入信号经过层层权重相乘（Linear）和激活函数（Non-linear），最终输出预测结果并计算损失。

![image-20260207183601699](../assets/img/posts/backpropagation.png)
*(图 1：全连接神经网络的前向传播结构。数据从 Input Layer 流向 Output Layer，最终通过 Loss Function 计算误差，图中的各个箭头和节点都可以看作是一个计算单元)*

在这个过程中，网络可以被看作一个巨大的**有向无环图（DAG）**，即**计算图**。
* **节点（Node）**：代表具体的数学运算（如加法、乘法、Sigmoid、ReLU 等）。
* **边（Edge）**：代表数据的流动方向，通常承载着权重（$w$）或中间变量。

---

## 2. 链式求导法则：反向传播的引擎

当网络输出结果后，我们需要通过损失函数 $L$ 来衡量预测值与真实值的差距。我们的目标是更新网络中的权重 $w$，使得 $L$ 最小化。为此，我们需要计算梯度：

$$
\nabla w = \frac{\partial L}{\partial w}
$$

对于深层网络，直接求导非常困难。反向传播利用**链式法则（Chain Rule）**，将复杂的全局求导分解为简单的局部求导。

### 2.1 路径这一端的微积分

假设我们想要更新输入层到隐藏层之间的某个权重 $w_{11}$，我们需要求解 $\frac{\partial L}{\partial w_{11}}$。

根据链式法则，梯度的传递路径与前向传播的信号路径刚好相反（从 Loss 回溯）：

$$
\frac{\partial L}{\partial w_{11}} = \underbrace{\frac{\partial L}{\partial x_1^{'}}}_{\text{上游梯度}} \cdot \underbrace{\frac{\partial x_1^{'}}{\partial w_{11}}}_{\text{本地梯度}}
$$

这里我们引入两个关键概念：
1.  **上游梯度（Upstream Gradient）**：从网络深层回传回来的误差信号（$\frac{\partial L}{\partial x_1^{'}}$）。
2.  **本地梯度（Local Gradient）**：当前计算节点产生的梯度（$\frac{\partial x_1^{'}}{\partial w_{11}}$）。

### 2.2 多路聚合：多元复合函数求导

在实际网络中，一个神经元（例如 $x_1^{'}$）通常会连接到下一层的多个神经元（例如 $z_1^{'}$ 和 $z_2^{'}$）。根据**多元复合函数求导法则**，当反向传播经过分叉点时，我们需要将所有分支传回来的梯度**累加**。

$$
\frac{\partial L}{\partial x_1^{'}} = 
\underbrace{\frac{\partial L}{\partial y} \frac{\partial y}{\partial x_1^{''}} \frac{\partial x_1^{''}}{\partial z_1^{'}} \frac{\partial z_1^{'}}{\partial x_1^{'}}}_{\text{路径 1 回传的梯度}}  
+ 
\underbrace{\frac{\partial L}{\partial y} \frac{\partial y}{\partial x_2^{''}} \frac{\partial x_2^{''}}{\partial z_2^{'}} \frac{\partial z_2^{'}}{\partial x_1^{'}}}_{\text{路径 2 回传的梯度}}
$$

将其具体化（假设 $z = w \cdot x$ 形式）：

$$
\frac{\partial L}{\partial x_1^{'}} = w_1^{''}\frac{\partial x_1^{''}}{\partial z_1^{'}}w_{11}^{'} + w_2^{''}\frac{\partial x_2^{''}}{\partial z_2^{'}}w_{12}^{'}
$$

这揭示了反向传播的一个核心逻辑：**误差是加权求和并反向流动的。**

### 2.3 本地梯度的计算

对于链式法则的末端 $\frac{\partial x_1^{'}}{\partial w_{11}}$，由于 $x_1^{'} = \sigma(z_1)$ 且 $z_1 = w_{11} \cdot x_1 + ...$，我们可以得到：

$$
\frac{\partial x_1^{'}}{\partial w_{11}} =
\frac{\partial x_1^{'}}{\partial z_1} \frac{\partial z_1}{\partial x_1}
=\frac{\partial x_1^{'}}{\partial z_1} \cdot x_1
$$

---

## 3. 算法总结：正向与反向的对偶性

通过上述推导，我们可以将计算图中的节点行为总结如下：

> **链式求导法则通俗版：**
> 1.  **局部求导**：每个计算单元算出其输出对输入的偏导数（Jacobian 矩阵）。
> 2.  **链路相乘**：在一条串联链路上，将沿途所有计算单元的偏导数连乘（$z_j = \prod\partial_i$）。
> 3.  **多路相加**：如果一个节点影响了多个下游节点，则将多条链路回传的梯度相加（$y = \sum z_j$）。
>
> *注：对于加法节点（Distributor），梯度直接复制分发（$\frac{\partial(a+b)}{\partial a}=1$）；对于乘法节点，梯度往往涉及交换相乘。*

仔细观察，我们可以知道$$\frac{\partial L}{\partial x_1^{'}}$$是由两条线路上的计算单元的偏导相乘再相加得到的，在此基础上，$$\frac{\partial L}{\partial x_1^{'}}$$乘以$$\frac{\partial x_1^{'}}{\partial z_1}$$，再乘以$$x_1$$就能得到$$\frac{\partial L}{\partial w_{11}}$$的结果，整个求解过程就像是从后往前逆箭头方向计算的，所以称之为反向传播。

![image-20260207191141702](../assets/img/posts/backpropagation2.png)



**正向传播中，各个计算节点都是以$$f(.)$$的形式参与计算的，而反向传播中各个计算节点都是以其偏导数相乘的形式参与计算的。**

## 4. 为什么这很重要？——梯度下降

既然我们已经辛苦算出了 $\frac{\partial L}{\partial w}$，下一步做什么？

这就回到了我们的终极目标——**参数更新**。反向传播算出的梯度，指明了“损失函数上升最快”的方向。为了减小损失，我们需要沿着梯度的**反方向**更新权重：

$$
w_{new} = w_{old} - \eta \cdot \frac{\partial L}{\partial w}
$$

其中 $\eta$ 是学习率（Learning Rate）。

如果没有反向传播，我们将无法高效地计算深度网络中数百万个参数的梯度，深度学习的繁荣也就无从谈起。反向传播算法，本质上就是**链式法则在计算图上的高效实现**。