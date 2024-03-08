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

好的, 让我重新开始推导, 并注意给出最终结果中每个参数的计算方式。

我们从更一般的形式 $\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2$ 出发:

1. 非归一化权重推导:

设非归一化权重为 $\tilde{w}_\tau$, 则有:

$$\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2 = \sum_{\tau=0}^{T_{total}-1}\frac{\tilde{w}_\tau}{T_{total}}R_{t-\tau}^2 \tag{1}$$

对 (1)进行推导:

$$\begin{aligned}
\sum_{\tau=0}^{T_{total}-1}\frac{\tilde{w}_\tau}{T_{total}}R_{t-\tau}^2 &= \frac{\tilde{w}_0}{T_{total}}R_t^2 + \sum_{\tau=1}^{T_{total}-1}\frac{\tilde{w}_\tau}{T_{total}}R_{t-\tau}^2\\
&= \frac{\tilde{w}_0}{T_{total}}R_t^2 + \left(1-\frac{\tilde{w}_0}{T_{total}}\right)\left(\sum_{\tau=1}^{T_{total}-1}\frac{\tilde{w}_\tau}{\sum_{s=1}^{T_{total}-1}\tilde{w}_s}R_{t-\tau}^2\right)\\
&= \frac{\tilde{w}_0}{T_{total}}R_t^2 + \left(1-\frac{\tilde{w}_0}{T_{total}}\right)\sigma_t^2
\end{aligned}\tag{2}$$

其中 $\sigma_t^2 = \sum_{\tau=1}^{T_{total}-1}\frac{\tilde{w}_\tau}{\sum_{s=1}^{T_{total}-1}\tilde{w}_s}R_{t-\tau}^2$ 为与非归一化权重 $\tilde{w}_\tau$ 相关的条件方差。

最终得到非归一化形式:

$$\boxed{\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2 = \frac{\tilde{w}_0}{T_{total}}R_t^2 + \left(1-\frac{\tilde{w}_0}{T_{total}}\right)\sigma_t^2}$$

其中 $\tilde{w}_0,\tilde{w}_\tau$ 为给定的非归一化权重序列。

2. 归一化权重推导:  

设归一化权重为 $w_\tau$, 满足 $\sum_{\tau=0}^{T_{total}-1}w_\tau = 1$, 则有:

$$\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2 = \sum_{\tau=0}^{T_{total}-1}w_\tau R_{t-\tau}^2 \tag{3}$$

对 (3)进行推导:

$$\begin{aligned}
\sum_{\tau=0}^{T_{total}-1}w_\tau R_{t-\tau}^2 &= w_0 R_t^2 + \sum_{\tau=1}^{T_{total}-1}w_\tau R_{t-\tau}^2\\
&= w_0 R_t^2 + (1-w_0)\left(\sum_{\tau=1}^{T_{total}-1}\frac{w_\tau}{1-w_0}R_{t-\tau}^2\right)\\
&= w_0 R_t^2 + (1-w_0)\sigma_t^2
\end{aligned}\tag{4}$$

其中 $\sigma_t^2 = \sum_{\tau=1}^{T_{total}-1}\frac{w_\tau}{1-w_0}R_{t-\tau}^2$ 为与归一化权重 $w_\tau$ 相关的条件方差。

最终得到归一化形式:  

$$\boxed{\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^2 = w_0 R_t^2 + (1-w_0)\sigma_t^2}$$

其中 $w_0$ 为给定的归一化权重序列的首项权重, $\sigma_t^2$ 由其余权重 $w_\tau$ 计算得到。

以上两种形式都可以用于逼近或等价于不同的 GARCH 类模型, 只需对权重序列 $\tilde{w}_\tau$ 或 $w_\tau$ 进行适当的参数化设置即可。请指出任何不清楚的地方。

---

非常抱歉, 在之前的推导中, 我没有清晰地解释 $\omega_0$ 是如何计算得出的。让我重新说明一下:

1. 对于非归一化形式 (2):

$$\sum_{\tau=0}^{T_{total}-1}\frac{\tilde{w}_\tau}{T_{total}}R_{t-\tau}^2 = \frac{\tilde{w}_0}{T_{total}}R_t^2 + \left(1-\frac{\tilde{w}_0}{T_{total}}\right)\sigma_t^2$$

其中,$\omega_0 = \frac{\tilde{w}_0}{T_{total}}$,即$\omega_0$是将非归一化权重$\tilde{w}_0$除以$T_{total}$得到的。

2. 对于归一化形式 (4):  

$$\sum_{\tau=0}^{T_{total}-1}w_\tau R_{t-\tau}^2 = w_0 R_t^2 + (1-w_0)\sigma_t^2$$

其中, 由于 $w_\tau$ 已经是归一化的权重序列, 满足 $\sum_{\tau=0}^{T_{total}-1}w_\tau = 1$, 所以 $\omega_0$ 就直接等于 $w_0$, 即归一化权重序列的首项权重。

所以, 在这两种情况下, $\omega_0$ 的计算方式是:

- 非归一化形式: $\omega_0 = \frac{\tilde{w}_0}{T_{total}}$
- 归一化形式: $\omega_0 = w_0$

我应该在最初的推导时就清晰地解释 $\omega_0$ 是如何得到的, 这样不会显得太突兀。总之, 对于非归一化形式, $\omega_0$ 是通过将首项权重 $\tilde{w}_0$ 除以 $T_{total}$ 得到的; 对于归一化形式, $\omega_0$ 就直接等于归一化权重序列的首项 $w_0$。请让我知道如果还有任何不清楚的地方。


----

对于归一化权重序列 $\omega_\tau = \lambda(1-\lambda)^{\tau-1}$ 的情况, 我们来计算 $\omega_0$:

首先确认该权重序列已经满足归一化条件:

$$\sum_{\tau=0}^{\infty}\omega_\tau = \sum_{\tau=0}^{\infty}\lambda(1-\lambda)^{\tau-1} = 1$$

因为这是一个等比数列求和, 根据公式:

$$\sum_{\tau=0}^{\infty}ar^{\tau} = \frac{a}{1-r},\\quad |r|<1$$

当 $a=\lambda, r=1-\lambda$ 时, 上式成立。

所以对于给定的 $\omega_\tau = \lambda(1-\lambda)^{\tau-1}$, 它已经是一个归一化的权重序列。

根据前面推导的归一化形式结果:

$$\sum_{\tau=0}^{T_{total}-1}w_\tau R_{t-\tau}^2 = w_0 R_t^2 + (1-w_0)\sigma_t^2$$

对于 $\omega_\tau = \lambda(1-\lambda)^{\tau-1}$ 这一特殊情况, 我们有:

$$\boxed{\omega_0 = \lambda}$$

也就是说, 对应的首项权重 $\omega_0$ 就等于 $\lambda$。

这一结果与 GARCH (1,1)模型的参数形式是一致的, 如果进一步令:

$$\alpha = \lambda, \beta = 1-\lambda, \omega=0$$
则 GARCH (1,1)模型就成为该加权移动平均模型的一种特殊情况。

因此, 对于归一化权重序列 $\omega_\tau = \lambda(1-\lambda)^{\tau-1}$, 其首项权重 $\omega_0$ 为 $\lambda$。


----

非常抱歉, 我的推导中似乎仍然存在一些错误, 让我再次仔细推导:

给定权重序列 $\omega_\tau = \lambda(1-\lambda)^{\tau-1}$, 则有:

$$\sum_{\tau=0}^{\infty}\omega_\tau R_{t-\tau}^2 = \sum_{\tau=0}^{\infty}\lambda(1-\lambda)^{\tau-1}R_{t-\tau}^2$$

将求和展开:

$$\begin{aligned}
\sum_{\tau=0}^{\infty}\lambda(1-\lambda)^{\tau-1}R_{t-\tau}^2 &= \lambda R_t^2 + \lambda(1-\lambda)R_{t-1}^2 + \lambda(1-\lambda)^2R_{t-2}^2 + ... \\
&= \lambda R_t^2 + (1-\lambda)\left(\lambda R_{t-1}^2 + \lambda(1-\lambda)R_{t-2}^2 + ...\right) \\
&= \lambda R_t^2 + (1-\lambda)\left(\sum_{\tau=1}^{\infty}\lambda(1-\lambda)^{\tau-1}R_{t-\tau}^2\right) \\
&= \lambda R_t^2 + (1-\lambda)\sigma_t^2
\end{aligned}$$

其中 $\sigma_t^2 = \sum_{\tau=1}^{\infty}\lambda(1-\lambda)^{\tau-1}R_{t-\tau}^2$。

因此, 正确的结果应该是:

$$\boxed{\sum_{\tau=0}^{\infty}\lambda(1-\lambda)^{\tau-1}R_{t-\tau}^2 = \lambda R_t^2 + (1-\lambda)\sigma_t^2}$$

这与我之前的推导都有出入, 非常感谢您的反复指正, 让我重新审视推导过程。请继续指出任何错误, 我会虚心学习。数学推导的严谨性非常重要, 我会努力改正。再次感谢您的耐心指导!

---

好的,我们将从归一化权重序列出发,推导到GARCH(1,1)模型:

给定归一化权重序列$\omega_\tau$,满足$\sum_{\tau=0}^{\infty}\omega_\tau = 1$,我们有:

$$\sum_{\tau=0}^{\infty}\omega_\tau R_{t-\tau}^2 = \omega_0 R_t^2 + \sum_{\tau=1}^{\infty}\omega_\tau R_{t-\tau}^2$$

进一步推导:

$$\begin{aligned} \sum_{\tau=0}^{\infty}\omega_\tau R_{t-\tau}^2 &= \omega_0 R_t^2 + \left(\sum_{\tau=1}^{\infty}\omega_\tau\right)\left(\sum_{\tau=1}^{\infty}\frac{\omega_\tau}{\sum_{s=1}^{\infty}\omega_s}R_{t-\tau}^2\right)\ &= \omega_0 R_t^2 + (1-\omega_0)\sigma_t^2 \end{aligned}$$

其中$\sigma_t^2 = \sum_{\tau=1}^{\infty}\frac{\omega_\tau}{1-\omega_0}R_{t-\tau}^2$为与归一化权重$\omega_\tau$相关的条件方差。

这与GARCH(1,1)模型的形式完全一致:

$$\sigma_t^2 = \omega + \alpha R_{t-1}^2 + \beta \sigma_{t-1}^2$$

将上式两端同乘$(1-\beta)$,并将$t$时间往前推移一期,我们有:

$$\begin{aligned} (1-\beta)\sigma_{t-1}^2 &= \omega + \alpha R_{t-2}^2\ \sigma_{t-1}^2 &= \frac{\omega}{1-\beta} + \frac{\alpha}{1-\beta}R_{t-2}^2 \end{aligned}$$

将上式代入GARCH(1,1)模型中,得到:

$$\begin{aligned} \sigma_t^2 &= \omega + \alpha R_{t-1}^2 + \beta\left(\frac{\omega}{1-\beta} + \frac{\alpha}{1-\beta}R_{t-2}^2\right)\ &= \frac{\omega}{1-\beta} + \alpha R_{t-1}^2 + \frac{\alpha\beta}{1-\beta}R_{t-2}^2 \end{aligned}$$

令$\omega_0 = \frac{\omega}{1-\beta}, \alpha_1 = \alpha, \alpha_2 = \frac{\alpha\beta}{1-\beta}$,我们得到:

$$\boxed{\sigma_t^2 = \omega_0 R_t^2 + \alpha_1 R_{t-1}^2 + \alpha_2 R_{t-2}^2 + ...}$$

这就是GARCH(1,1)模型从归一化权重序列的推导过程。当$\alpha_2=\alpha_3=...=0$时,它就化为标准的GARCH(1,1)形式。通过这种推导,我们可以看出GARCH模型是从加权移动平均模型的思路出发,对权重进行了特殊的参数化,从而捕捉了方差的持续性和聚集性。

如有任何错误,请继续指正,我会谨记教训,重新学习。再次感谢您的耐心指导!

----

好的, 我们继续推导 GARCH (1,2)和 GARCH (2,1)模型:

1. GARCH (1,2)推导

我们将 GARCH (1,1)模型的推导结果扩展到 2 个滞后阶数:

$$\sigma_t^2 = \omega_0 R_t^2 + \alpha_1 R_{t-1}^2 + \alpha_2 R_{t-2}^2 + ...$$

考虑截止到 2 个滞后阶数的情况, 并令 $\alpha_3 = \alpha_4 = ... = 0$, 则有:

$$\begin{aligned}
\sigma_t^2 &= \omega_0 R_t^2 + \alpha_1 R_{t-1}^2 + \alpha_2 R_{t-2}^2\\
         &= \omega_0 R_t^2 + (\alpha_1 + \beta_1) R_{t-1}^2 + \beta_1 \sum_{\tau=2}^{\infty}\frac{\alpha_2}{1-\beta_1}R_{t-\tau}^2\\
         &= \omega_0 R_t^2 + (\alpha_1 + \beta_1) R_{t-1}^2 + \beta_1 \frac{\alpha_2}{1-\beta_1}R_{t-2}^2
\end{aligned}$$

其中 $\alpha_1 + \beta_1 < 1, \frac{\alpha_2}{1-\beta_1} < 1$ 确保平稳性。

将 $\omega_0, \alpha_1, \beta_1, \alpha_2$ 重新参数化, 我们得到 GARCH (1,2)模型:

$$\boxed{\sigma_t^2 = \omega + \alpha_1 R_{t-1}^2 + \alpha_2 R_{t-2}^2 + \beta \sigma_{t-1}^2}$$

2. GARCH (2,1)推导  

类似地, 我们可以推导 GARCH (2,1)模型:

$$\begin{aligned}
\sigma_t^2 &= \omega_0 R_t^2 + \alpha_1 R_{t-1}^2 + \alpha_2 R_{t-2}^2 + \beta_1 \sigma_{t-1}^2 + \beta_2 \sigma_{t-2}^2\\
         &= \omega_0 R_t^2 + (\alpha_1 + \gamma_1)R_{t-1}^2 + \gamma_1 \sum_{\tau=2}^{\infty}\frac{\alpha_2}{1-\gamma_1}R_{t-\tau}^2 \\
         &\quad + \beta_1 \sigma_{t-1}^2 + \beta_2 \frac{\beta_1}{1-\beta_1}\sigma_{t-1}^2\\
         &= \omega_0 R_t^2 + (\alpha_1 + \gamma_1)R_{t-1}^2 + \gamma_1 \frac{\alpha_2}{1-\gamma_1}R_{t-2}^2 + \beta_1 \sigma_{t-1}^2 + \beta_2 \sigma_{t-2}^2
\end{aligned}$$

其中 $\alpha_1 + \gamma_1 < 1, \frac{\alpha_2}{1-\gamma_1} < 1, \beta_1 + \beta_2 < 1$ 确保平稳性。

将参数重新标记, 我们得到 GARCH (2,1)模型:

$$\boxed{\sigma_t^2 = \omega + \alpha_1 R_{t-1}^2 + \alpha_2 R_{t-2}^2 + \beta_1 \sigma_{t-1}^2 + \beta_2 \sigma_{t-2}^2}$$

通过上述推导, 我们可以看到 GARCH (p, q)模型是在加权移动平均模型的基础上, 通过对权重序列进行特殊的参数化, 从而引入了自回归和移动平均两部分, 捕捉了方差的持续性和聚集性。

这种从基础模型出发, 通过修改权重得到更一般模型的思路, 可以应用于推导更多的 GARCH family 模型。如有任何错误, 请继续指正。再次感谢您的悉心指导!