---
date: 7th March 2024
Doctype: Note
Category: 02_Writing Books/Risk Management and Trade/Untitled.md
Status: 
abstract: 
Keywords: 
tags:
  - "1"
  - "2"
  - "3"
Version: 1.0.0
License: MIT
Copyright: © 2024 FAN WANG. All rights reserved.
---
好的, 我将把上两个回答结合, 并注意逻辑顺序的清晰性:

ARCH (自回归条件异方差)模型是最早用于捕捉金融时间序列数据中的条件异方差效应的模型, 由 Robert Engle 于 1982 年提出。ARCH (q)模型可表示为:

$$\sigma_t^2 = \omega + \alpha_1\epsilon_{t-1}^2 + ... + \alpha_q\epsilon_{t-q}^2 \tag{1}$$

它可看作是对过去 q 期残差平方的加权和, 权重分别为 $\alpha_1,...,\alpha_q$, 常数项 $\omega$ 确保方差为正。

我们可以从加权移动平均模型的角度来推导 ARCH 模型。加权移动平均模型为:

$$\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2 = \lambda \sum_{\tau=1}^{\infty}(1-\lambda)^{\tau-1}R_{t-\tau}^2 \tag{2}$$

令 $w_\tau = \alpha_\tau, \tau=1,2,...q, 其他\tau权重为0$,则 (2)可推广为:

$$\sum_{\tau=0}^{T_{total}-1}w_\tau R_{t-\tau}^2 = \alpha_1 R_{t-1}^2 + \alpha_2 R_{t-2}^2 + ... + \alpha_q R_{t-q}^2 \tag{3}$$

加上常数项 $\omega$, 我们就得到了 ARCH (q)模型形式 (1)。

GARCH (广义自回归条件异方差)模型是在 ARCH 模型基础上提出的, 不仅考虑了残差项, 还包括了过去条件方差对当前条件方差的影响, 从而更好地捕捉了条件异方差的持续性和聚集性。GARCH (1,1)模型为:

$$\sigma_t^2 = \omega + \alpha R_{t-1}^2 + \beta \sigma_{t-1}^2, \quad \text{with} \alpha + \beta < 1 \tag{4}$$

其中 $\alpha$ 捕捉残差的影响 (ARCH 项), $\beta$ 捕捉过去条件方差的影响 (GARCH 项), $\omega$ 为常数项, 约束 $\alpha+\beta<1$ 确保平稳性。根据(4)可推导:

$$\omega = \sigma^2(1 - \alpha - \beta) \tag{5}$$

其中 $\sigma^2$ 为 GARCH 过程的无条件长期均值方差。

我们也可以从加权移动平均模型 (2)的角度来推导 GARCH (1,1)模型, 令 $\alpha=\lambda, \beta=(1-\lambda)\lambda, \omega=(1-\lambda)\sigma^2$,则 (2)就变为 GARCH (1,1)模型 (4)。

另外, RiskMetrics 模型是 GARCH (1,1)模型的一种特殊情况:

$$\text{If} \ \alpha = 1 - \lambda, \ \beta = \lambda, \ \omega = 0, \ \text{then GARCH(1,1) reduces to RiskMetrics model}$$

当 $\alpha+\beta=1$ 时,GARCH (1,1)模型成为积分 GARCH (IGARCH)模型, 无条件方差不存在。

TARCH (绝对值 ARCH)模型在 ARCH 模型基础上, 对正负残差赋予不同权重, 捕捉了杠杆效应:

$$\sigma_t^2 = \omega + \alpha_1\epsilon_{t-1}^2 + \gamma_1\epsilon_{t-1}^2I_{t-1} + ... + \alpha_q\epsilon_{t-q}^2 + \gamma_q\epsilon_{t-q}^2I_{t-q}$$

其中 $I_t=1$ 如果 $\epsilon_t<0$, 否则为 0。当 $\gamma_1=0$ 时,TARCH (1,1)就等价于 GARCH (1,1)模型。

综上所述, ARCH 模型家族通过权重修改和参数设定, 能很好地捕捉金融时间序列数据中的波动聚集性, 展现出了优秀的建模能力。

----
好的, 我们来推导一下相关公式:

(1) 首先从 $\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2$ 推导出 $(1-\lambda)R_{t}^2+\lambda\sigma_{t}^2$:

$$\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2 = \lambda \sum_{\tau=1}^{\infty}(1-\lambda)^{\tau-1}R_{t-\tau}^2$$

令 $\tau=0$ 项为 $(1-\lambda)R_{t}^2$, 其余项为 $\lambda\sigma_{t}^2$,则:

$$(1-\lambda)R_{t}^2 + \lambda\sigma_{t}^2 = \lambda \sum_{\tau=0}^{\infty}(1-\lambda)^{\tau}R_{t-\tau}^2$$

(2) 现在我们来看 $\lambda \sum_{\tau=1}^{\infty}(1-\lambda)^{\tau-1}R_{t-\tau}^2$ 推导出什么:

$$\lambda \sum_{\tau=1}^{\infty}(1-\lambda)^{\tau-1}R_{t-\tau}^2 = \lambda \sum_{\tau=0}^{\infty}(1-\lambda)^{\tau}R_{t-\tau-1}^2$$

令 $s=\tau+1$, 则:

$$\begin{aligned}
\lambda \sum_{\tau=0}^{\infty}(1-\lambda)^{\tau}R_{t-\tau-1}^2 &= \lambda \sum_{s=1}^{\infty}(1-\lambda)^{s-1}R_{t-s}^2\\
&= \lambda \left(\sigma_{t}^2 - (1-\lambda)R_{t}^2\right)
\end{aligned}$$

其中 $\sigma_{t}^2$ 为条件方差, 等于 $\sum_{s=1}^{\infty}(1-\lambda)^{s-1}R_{t-s}^2$。

因此, 我们得到:

$$\boxed{\lambda \sum_{\tau=1}^{\infty}(1-\lambda)^{\tau-1}R_{t-\tau}^2 = \lambda\sigma_{t}^2 - \lambda(1-\lambda)R_{t}^2}$$

这一结果与 GARCH (1,1)模型 $\sigma_{t}^2 = \omega + \alpha R_{t-1}^2 + \beta\sigma_{t-1}^2$ 的形式相吻合, 也说明了加权移动平均模型与 GARCH 模型之间的密切关系。

----

好的, 我们对 $\sum_{\tau=0}^{T_{total}-1}w_\tau R_{t-\tau}^2$ 进行类似的推导:

首先, 我们有加权移动平均模型:

$$\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2 = \lambda \sum_{\tau=1}^{\infty}(1-\lambda)^{\tau-1}R_{t-\tau}^2 \tag{1}$$

令 $w_\tau = \frac{1}{T_{total}}$, 则左边就变为 $\sum_{\tau=0}^{T_{total}-1}w_\tau R_{t-\tau}^2$。

现在,我们将 (1)中的 $\lambda \sum_{\tau=1}^{\infty}(1-\lambda)^{\tau-1}R_{t-\tau}^2$ 展开:

$$\begin{aligned}
\lambda \sum_{\tau=1}^{\infty}(1-\lambda)^{\tau-1}R_{t-\tau}^2 &= \lambda R_{t-1}^2 + \lambda(1-\lambda)R_{t-2}^2 + \lambda(1-\lambda)^2R_{t-3}^2 + \cdots \\
&= \lambda R_{t-1}^2 + (1-\lambda)\lambda\sum_{\tau=2}^{\infty}(1-\lambda)^{\tau-2}R_{t-\tau}^2 \\
&= \lambda R_{t-1}^2 + (1-\lambda)\left(\sum_{\tau=1}^{\infty}(1-\lambda)^{\tau-1}R_{t-\tau}^2 - R_{t-1}^2\right) \\
&= \lambda R_{t-1}^2 + (1-\lambda)\left(\sigma_t^2 - R_{t-1}^2\right)
\end{aligned}\tag{2}$$

其中 $\sigma_t^2 = \sum_{\tau=1}^{\infty}(1-\lambda)^{\tau-1}R_{t-\tau}^2$ 为条件方差。

将 (2)代入 (1)得:

$$\boxed{\sum_{\tau=0}^{T_{total}-1}w_\tau R_{t-\tau}^2 = \lambda R_{t-1}^2 + (1-\lambda)\sigma_t^2}$$

这一结果也与 GARCH (1,1)模型 $\sigma_t^2 = \omega + \alpha R_{t-1}^2 + \beta\sigma_{t-1}^2$ 相吻合, 进一步说明了加权移动平均模型与 GARCH 模型之间的联系。如果令 $\alpha=\lambda,\beta=1-\lambda,\omega=0$, 那么 GARCH (1,1)就成为加权移动平均模型的特殊情况。

----

非常抱歉, 我之前的推导存在一些问题。现在我们从更一般的形式 $\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2$ 出发, 使用不同的参数代表权重进行推导:

设权重为 $w_\tau$, 则有:

$$\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2 = \sum_{\tau=0}^{T_{total}-1}w_\tau R_{t-\tau}^2 \tag{1}$$

其中 $\sum_{\tau=0}^{T_{total}-1}w_\tau = 1$

我们对 (1)进行如下推导:

$$\begin{aligned}
\sum_{\tau=0}^{T_{total}-1}w_\tau R_{t-\tau}^2 &= w_0 R_t^2 + \sum_{\tau=1}^{T_{total}-1}w_\tau R_{t-\tau}^2\\
&= w_0 R_t^2 + (1-w_0)\left(\sum_{\tau=1}^{T_{total}-1}\frac{w_\tau}{1-w_0}R_{t-\tau}^2\right)\\
&= w_0 R_t^2 + (1-w_0)\sigma_t^2
\end{aligned}\tag{2}$$

其中 $\sigma_t^2 = \sum_{\tau=1}^{T_{total}-1}\frac{w_\tau}{1-w_0}R_{t-\tau}^2$ 为与权重 $w_\tau$ 相关的条件方差。

将 (2)代入 (1)得:

$$\boxed{\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2 = w_0 R_t^2 + (1-w_0)\sigma_t^2}$$

这个结果更加一般化, 不再局限于某种特殊的级数形式, 只要权重之和为 1 即可。当 $w_0=\lambda,w_\tau=(1-\lambda)\lambda^{\tau-1}$ 时, 它就化为之前的 GARCH (1,1)形式。

我们还可以进一步推导 $\sigma_t^2$ 的表达式:

$$\begin{aligned}
\sigma_t^2 &= \sum_{\tau=1}^{T_{total}-1}\frac{w_\tau}{1-w_0}R_{t-\tau}^2\\
&= \sum_{\tau=1}^{T_{total}-1}\frac{w_\tau}{w_0}\left(\frac{1}{T_{total}}\sum_{s=0}^{T_{total}-1}R_{t-s}^2 - w_0 R_t^2\right)\\
&= \alpha_t\left(\frac{1}{T_{total}}\sum_{s=0}^{T_{total}-1}R_{t-s}^2 - w_0 R_t^2\right)
\end{aligned}$$

其中 $\alpha_t = \sum_{\tau=1}^{T_{total}-1}\frac{w_\tau}{w_0}$。

所以最终我们得到:

$$\boxed{\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2 = w_0 R_t^2 + (1-w_0)\alpha_t\left(\frac{1}{T_{total}}\sum_{s=0}^{T_{total}-1}R_{t-s}^2 - w_0 R_t^2\right)}$$

这是一个比较普遍的形式, 根据不同的权重设定, 可以逼近或等价于 GARCH 等模型。请指出如有任何问题, 我将非常感谢。

----

好的, 我们给出非归一化权重的推导:

设非归一化权重为 $\tilde{w}_\tau$, 则有:

$$\sum_{\tau=0}^{T_{total}-1}\tilde{w}_\tau R_{t-\tau}^2 \tag{1}$$

我们对 (1)进行如下推导:

$$\begin{aligned}
\sum_{\tau=0}^{T_{total}-1}\tilde{w}_\tau R_{t-\tau}^2 &= \tilde{w}_0 R_t^2 + \sum_{\tau=1}^{T_{total}-1}\tilde{w}_\tau R_{t-\tau}^2\\
&= \tilde{w}_0 R_t^2 + \left(\sum_{\tau=1}^{T_{total}-1}\tilde{w}_\tau\right)\left(\sum_{\tau=1}^{T_{total}-1}\frac{\tilde{w}_\tau}{\sum_{s=1}^{T_{total}-1}\tilde{w}_s}R_{t-\tau}^2\right)\\
&= \tilde{w}_0 R_t^2 + \left(\sum_{\tau=0}^{T_{total}-1}\tilde{w}_\tau - \tilde{w}_0\right)\sigma_t^2
\end{aligned}\tag{2}$$

其中 $\sigma_t^2 = \sum_{\tau=1}^{T_{total}-1}\frac{\tilde{w}_\tau}{\sum_{s=1}^{T_{total}-1}\tilde{w}_s}R_{t-\tau}^2$ 为与非归一化权重 $\tilde{w}_\tau$ 相关的条件方差。

将 (2)代入 (1)得:

$$\boxed{\sum_{\tau=0}^{T_{total}-1}\tilde{w}_\tau R_{t-\tau}^2 = \tilde{w}_0 R_t^2 + \left(\sum_{\tau=0}^{T_{total}-1}\tilde{w}_\tau - \tilde{w}_0\right)\sigma_t^2}$$

这个结果使用了非归一化的权重 $\tilde{w}_\tau$, 更加通用。当权重满足 $\sum_{\tau=0}^{T_{total}-1}\tilde{w}_\tau = 1$ 时, 就化为之前的归一化形式。

我们还可以进一步推导 $\sigma_t^2$ 的表达式:

$$\begin{aligned}
\sigma_t^2 &= \sum_{\tau=1}^{T_{total}-1}\frac{\tilde{w}_\tau}{\sum_{s=1}^{T_{total}-1}\tilde{w}_s}R_{t-\tau}^2\\
&= \sum_{\tau=1}^{T_{total}-1}\frac{\tilde{w}_\tau}{\sum_{\tau=0}^{T_{total}-1}\tilde{w}_\tau - \tilde{w}_0}\left(\sum_{s=0}^{T_{total}-1}\tilde{w}_sR_{t-s}^2 - \tilde{w}_0 R_t^2\right)\\
&= \alpha_t\left(\sum_{s=0}^{T_{total}-1}\tilde{w}_sR_{t-s}^2 - \tilde{w}_0 R_t^2\right)
\end{aligned}$$

其中 $\alpha_t = \sum_{\tau=1}^{T_{total}-1}\frac{\tilde{w}_\tau}{\sum_{\tau=0}^{T_{total}-1}\tilde{w}_\tau - \tilde{w}_0}$。

所以最终我们得到:

$$\boxed{\sum_{\tau=0}^{T_{total}-1}\tilde{w}_\tau R_{t-\tau}^2 = \tilde{w}_0 R_t^2 + \left(\sum_{\tau=0}^{T_{total}-1}\tilde{w}_\tau - \tilde{w}_0\right)\alpha_t\left(\sum_{s=0}^{T_{total}-1}\tilde{w}_sR_{t-s}^2 - \tilde{w}_0 R_t^2\right)}$$

这是一个使用非归一化权重 $\tilde{w}_\tau$ 的更普遍形式。根据不同的权重设定, 可逼近或等价于 GARCH 等模型。请指出任何问题。