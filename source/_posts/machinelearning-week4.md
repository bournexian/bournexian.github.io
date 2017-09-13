---
title: machinelearning-week4
date: 2017-09-05 22:29:52
tags: machinelearning
mathjax: true
---

## 机器学习 第4周

原来mathjax公式是可以通过右键复制的，囧， 好吧，讲义里面的公式就不用手打了~~~ 新技能get✓

As non-linear hypotheses become complex when more features included, an alternate way is neural networks.

In neural networks, we label the intermediate layer nodes $ a_0^2 ... a_n^2 $ and call them activation units.
 {% raw %}
$$
\begin{align*}& a_i^{(j)} = \text{"activation" of unit $i$ in layer $j$} \newline& \Theta^{(j)} = \text
{matrix of weights controlling function mapping from layer $j$ to layer $j+1$}\end{align*}
$$
 {% endraw %}

For example, a neural network with one intermediate layer looks like:

 {% raw %}
$$
\begin{bmatrix}x_0 \newline x_1 \newline x_2 \newline x_3\end{bmatrix}\rightarrow\begin{bmatrix}a_1^{(2)} \newline a_2^{(2)} \newline a_3^{(2)} \newline \end{bmatrix}\rightarrow h_\theta(x)
 $$
 {% endraw %}

The values of activation nodes:
 {% raw %}
$$
\begin{align*}
a_1^{(2)} = g(\Theta_{10}^{(1)}x_0 + \Theta_{11}^{(1)}x_1 + \Theta_{12}^{(1)}x_2 + \Theta_{13}^{(1)}x_3) \newline
a_2^{(2)} = g(\Theta_{20}^{(1)}x_0 + \Theta_{21}^{(1)}x_1 + \Theta_{22}^{(1)}x_2 + \Theta_{23}^{(1)}x_3) \newline
a_3^{(2)} = g(\Theta_{30}^{(1)}x_0 + \Theta_{31}^{(1)}x_1 + \Theta_{32}^{(1)}x_2 + \Theta_{33}^{(1)}x_3) \newline
h_\Theta(x) = a_1^{(3)} = g(\Theta_{10}^{(2)}a_0^{(2)} + \Theta_{11}^{(2)}a_1^{(2)} + \Theta_{12}^{(2)}a_2^{(2)} + \Theta_{13}^{(2)}a_3^{(2)}) \newline
\end{align*}
$$
 {% endraw %}

The dimensions of the matrices of weights determined as follows:
 {% raw %}
 $$
 \text{If network has $s_j$ units in layer $j$ and $s_{j+1}$ units in layer $j+1$, then $\Theta^{(j)}$ will be of dimension $s_{j+1} \times (s_j + 1)$.}
 $$
 {% endraw %}


Make a new variable z to represent the parameters inside the g function.
 {% raw %}
 $$
 \begin{align*}a_1^{(2)} = g(z_1^{(2)}) \newline a_2^{(2)} = g(z_2^{(2)}) \newline a_3^{(2)} = g(z_3^{(2)}) \newline 
 
 
\newline 
 h_\Theta(x) = a^{(j+1)} = g(z^{(j+1)})
 \end{align*}
 
 $$
 {% endraw %}



For  AND, NOR, and OR operation could be represented by neural network.
And these simple operation could be combined to get XNOR. 

 {% raw %}
 $$
\begin{align*}& a^{(2)} = g(\Theta^{(1)} \cdot x) \newline& a^{(3)} = g(\Theta^{(2)} \cdot a^{(2)}) \newline& h_\Theta(x) = a^{(3)}\end{align*}
 $$
 {% endraw %}