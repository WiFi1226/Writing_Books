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

$$\boxed{\omega_0 = \lambda}\boxed{\omega_0 = \lambda}$$

也就是说, 对应的首项权重 $\omega_0$ 就等于 $\lambda$。

这一结果与 GARCH (1,1)模型的参数形式是一致的, 如果进一步令:

$$\alpha = \lambda, \beta = 1-\lambda, \omega=0$$
则 GARCH (1,1)模型就成为该加权移动平均模型的一种特殊情况。

因此, 对于归一化权重序列 $\omega_\tau = \lambda(1-\lambda)^{\tau-1}$, 其首项权重 $\omega_0$ 为 $\lambda$。


----

非常抱歉, 我的推导中似乎仍然存在一些错误, 让我再次仔细推导:

给定权重序列 $\omega_\tau = \lambda(1-\lambda)^{\tau-1}$, 则有:

$$\sum_{\tau=0}^{\infty}\omega_\tau R_{t-\tau}^2 = \sum_{\tau=0}^{\infty}\lambda(1-\lambda)^{\tau-1}R_{t-\tau}^2$$


---
ARCH模型确实可以直接推导到GARCH模型。事实上,GARCH模型是ARCH模型的一般化。下面我来说明这个推导过程。

我们从ARCH(q)模型开始:

$\sigma_{t}^2=\omega+\sum_{i=1}^{q}\alpha_{i}R_{t-i}^2$

这里,$\sigma_{t}^2$是时间$t$的条件方差,$R_{t}$是时间$t$的收益率,$\omega$和$\alpha_{i}$是待估参数。

现在,我们对$R_{t-q}^2$应用ARCH(q)模型:

$R_{t-q}^2=\sigma_{t-q}^2+v_{t-q}$

其中$v_{t-q}$是一个均值为0的随机变量。将这个表达式代入ARCH(q)模型:

$\sigma_{t}^2=\omega+\sum_{i=1}^{q-1}\alpha_{i}R_{t-i}^2+\alpha_{q}(\sigma_{t-q}^2+v_{t-q})$ $=\omega+\sum_{i=1}^{q-1}\alpha_{i}R_{t-i}^2+\alpha_{q}\sigma_{t-q}^2+\alpha_{q}v_{t-q}$

这就是GARCH(q,1)模型的形式,其中$\beta_{1}=\alpha_{q}$。

我们可以继续这个过程,对$\sigma_{t-q}^2$应用ARCH(q)模型,然后不断迭代。这最终会导致一个一般的GARCH(p,q)模型:

$\sigma_{t}^2=\omega+\sum_{i=1}^{q}\alpha_{i}R_{t-i}^2+\sum_{j=1}^{p}\beta_{j}\sigma_{t-j}^2$

这里,$p$表示GARCH项的数量,$q$表示ARCH项的数量。

所以,GARCH模型可以看作是ARCH模型的自然延伸。通过引入过去的条件方差项,GARCH模型可以更简洁地捕捉波动率的持续性,而不需要像ARCH模型那样引入大量的滞后项。这也解释了为什么在实践中,GARCH(1,1)模型常常优于高阶的ARCH模型。

需要注意的是,虽然GARCH模型可以从ARCH模型推导出来,但反过来不行。GARCH模型是更一般的模型,它包含了ARCH模型作为特例。

---

好的,让我详细解释一下这个步骤。

在ARCH(q)模型中,我们假设条件方差$\sigma_{t}^2$是过去$q$个时期的squared residuals (即$R_{t-1}^2, R_{t-2}^2, \ldots, R_{t-q}^2$)的线性函数。换句话说,我们假设当前的波动率由过去的"意外冲击"(unexpected shocks)决定,这些冲击的大小由squared residuals度量。

然而,在现实中,我们并不能直接观察到这些"意外冲击"。我们观察到的只是实际的收益率$R_{t}$。那么,我们如何将ARCH模型与实际的收益率联系起来呢?这就是我们引入等式$R_{t-q}^2=\sigma_{t-q}^2+v_{t-q}$的原因。

这个等式告诉我们,在时间$t-q$,squared residual $R_{t-q}^2$可以分解为两部分:条件方差$\sigma_{t-q}^2$和一个均值为0的随机变量$v_{t-q}$。这里的关键在于,条件方差$\sigma_{t-q}^2$是在时间$t-q$之前的信息集下的期望,而$v_{t-q}$则表示了在时间$t-q$实现的"意外冲击"。

从数学上讲,如果我们假设收益率服从条件正态分布,那么我们有:

$R_{t-q}|\Omega_{t-q-1} \sim N(0, \sigma_{t-q}^2)$

其中$\Omega_{t-q-1}$表示在时间$t-q-1$之前的所有信息。在这个假设下,$v_{t-q}$实际上就是标准化的残差:

$v_{t-q} = R_{t-q}^2 - \sigma_{t-q}^2 = \sigma_{t-q}^2 (\frac{R_{t-q}^2}{\sigma_{t-q}^2} - 1)$

如果模型设定正确,那么标准化残差应该是一个均值为0,方差为1的独立同分布(i.i.d.)随机变量序列。

通过引入$R_{t-q}^2=\sigma_{t-q}^2+v_{t-q}$,我们建立了ARCH模型中的"意外冲击"与实际的squared residuals之间的联系。这允许我们将ARCH模型写成GARCH模型的形式,从而得到一个更一般,更灵活的波动率模型。

需要注意的是,虽然我们在推导过程中假设了条件正态分布,但GARCH模型并不严格依赖于这个假设。GARCH模型可以捕捉波动率聚集的特征,即使收益率的条件分布不是正态的。这是GARCH模型的一个重要优点。


---

这是一个非常好的问题。在GARCH(1,2)模型中,确实存在一定的重复计算,因为$\sigma_{t-1}^2$和$\sigma_{t-2}^2$都包含了$R_{t-2}^2$及之前的信息。让我们详细看看这个问题。

GARCH(1,2)模型可以写成:

$\sigma_{t}^2 = \omega + \alpha_{1}R_{t-1}^2 + \beta_{1}\sigma_{t-1}^2 + \beta_{2}\sigma_{t-2}^2$

展开$\sigma_{t-1}^2$,我们得到:

$\sigma_{t}^2 = \omega + \alpha_{1}R_{t-1}^2 + \beta_{1}(\omega + \alpha_{1}R_{t-2}^2 + \beta_{1}\sigma_{t-2}^2 + \beta_{2}\sigma_{t-3}^2) + \beta_{2}\sigma_{t-2}^2$
$= \omega(1 + \beta_{1}) + \alpha_{1}R_{t-1}^2 + (\alpha_{1}\beta_{1} + \beta_{2})\sigma_{t-2}^2 + \beta_{1}^2\sigma_{t-2}^2 + \beta_{1}\beta_{2}\sigma_{t-3}^2$

我们可以看到,$\sigma_{t-2}^2$在这个表达式中出现了两次,一次是独立的,另一次是$\sigma_{t-1}^2$的一部分。这意味着$R_{t-2}^2$及之前的信息被重复使用了。

这种重复使用过去信息的特性是GARCH模型的一个固有特征。它源于GARCH模型的递归结构:当前的条件方差依赖于过去的条件方差,而过去的条件方差又依赖于更早的条件方差,以此类推。

尽管存在这种重复计算,GARCH模型在实践中还是非常有用的。这是因为GARCH模型的目的不是提供一个精确的波动率分解,而是提供一个简洁的方式来捕捉波动率的关键特征,如波动率聚集和持续性。

此外,重复使用过去信息的影响会随着时间的推移而衰减。在上面的例子中,$\sigma_{t-3}^2$的系数是$\beta_{1}\beta_{2}$,这通常小于$\sigma_{t-2}^2$的系数$(\alpha_{1}\beta_{1} + \beta_{2})$。随着我们往前推移,过去信息的权重会以几何速度衰减。

所以,虽然GARCH(1,2)模型确实存在一定的重复计算,但这并不影响它在实践中的有效性。GARCH模型提供了一个简洁且直观的方式来捕捉波动率的关键特征,这使得它成为金融计量经济学中最广泛使用的工具之一。

$$
\begin{aligned}
\b
\end{aligned}

$$


---

这是一个非常有洞察力的观察。确实,在GARCH模型中,距离越远的条件方差似乎会被重复计算得越多。但实际上,GARCH模型有一个特性可以避免这个问题,那就是"协方差平稳性"(covariance stationarity)。

在GARCH(1,2)模型中,如果$\alpha_1 + \beta_1 + \beta_2 < 1$,那么这个模型就是协方差平稳的。这个条件确保了过去的冲击对当前的条件方差的影响会随时间衰减,而不是无限累积。

让我们看看这是如何发生的。我们可以将GARCH(1,2)模型迭代地展开:

$\sigma_{t}^2 = \omega + \alpha_{1}R_{t-1}^2 + \beta_{1}\sigma_{t-1}^2 + \beta_{2}\sigma_{t-2}^2$
$= \omega + \alpha_{1}R_{t-1}^2 + \beta_{1}(\omega + \alpha_{1}R_{t-2}^2 + \beta_{1}\sigma_{t-2}^2 + \beta_{2}\sigma_{t-3}^2) + \beta_{2}(\omega + \alpha_{1}R_{t-3}^2 + \beta_{1}\sigma_{t-3}^2 + \beta_{2}\sigma_{t-4}^2)$
$= \omega(1 + \beta_1 + \beta_2) + \alpha_1 R_{t-1}^2 + (\alpha_1\beta_1 + \alpha_1\beta_2)R_{t-2}^2 + (\alpha_1\beta_1^2 + \alpha_1\beta_1\beta_2)R_{t-3}^2 + \cdots$

如果我们继续这个过程,我们会得到一个无穷级数:

$\sigma_{t}^2 = \frac{\omega}{1-\beta_1-\beta_2} + \alpha_1 \sum_{i=1}^{\infty} (\beta_1 + \beta_2)^{i-1} R_{t-i}^2$

这个表达式告诉我们,当前的条件方差是过去所有squared residuals的加权和,权重以几何速度衰减。如果$\alpha_1 + \beta_1 + \beta_2 < 1$,那么这个无穷级数是收敛的,过去的冲击对当前条件方差的影响会随时间衰减到0。

从直观上讲,协方差平稳性条件确保了GARCH过程有一个"长期均值"。虽然条件方差在短期内会对新的信息做出反应,但从长远来看,它会回归到这个长期均值。这防止了条件方差随时间无限增长。

所以,虽然在GARCH模型的递归结构中,距离越远的条件方差似乎被重复计算得越多,但由于协方差平稳性的要求,这种影响实际上会随时间衰减。这确保了GARCH模型可以生成一个均值回复的波动率过程,这与我们在金融市场中观察到的情况一致。

----

当然,我们可以用类似的方法将GARCH(1,q)模型展开为一个无穷级数。

GARCH(1,q)模型的一般形式为:

$\sigma_{t}^2 = \omega + \alpha_{1}R_{t-1}^2 + \beta_{1}\sigma_{t-1}^2 + \beta_{2}\sigma_{t-2}^2 + \cdots + \beta_{q}\sigma_{t-q}^2$

我们可以迭代地展开这个模型:

$\sigma_{t}^2 = \omega + \alpha_{1}R_{t-1}^2 + \beta_{1}(\omega + \alpha_{1}R_{t-2}^2 + \beta_{1}\sigma_{t-2}^2 + \cdots + \beta_{q}\sigma_{t-q-1}^2) + \cdots + \beta_{q}(\omega + \alpha_{1}R_{t-q-1}^2 + \beta_{1}\sigma_{t-q-1}^2 + \cdots + \beta_{q}\sigma_{t-2q}^2)$

通过不断地替换右边的$\sigma_{t-i}^2$项,我们最终会得到一个只包含$R_{t-i}^2$项和常数项的无穷级数:

$\sigma_{t}^2 = \omega(1 + \sum_{i=1}^{\infty}\sum_{j=1}^{q}\beta_{j}^{i}) + \alpha_{1}\sum_{i=1}^{\infty}(\sum_{j=1}^{q}\beta_{j})^{i-1}R_{t-i}^2$

这个结果可以进一步简化。注意到几何级数的和公式:

$\sum_{i=1}^{\infty}ar^{i-1} = \frac{a}{1-r}, |r|<1$

我们可以将无穷级数简化为:

$\sigma_{t}^2 = \frac{\omega}{1-\sum_{j=1}^{q}\beta_{j}} + \alpha_{1}\sum_{i=1}^{\infty}(\sum_{j=1}^{q}\beta_{j})^{i-1}R_{t-i}^2$

这个结果与GARCH(1,2)的情况非常相似。当前的条件方差可以表示为过去所有squared residuals的加权和,权重以几何速度衰减。唯一的区别是,衰减速度现在由$\sum_{j=1}^{q}\beta_{j}$决定,而不仅仅是$\beta_1 + \beta_2$。

为了确保这个无穷级数收敛,我们需要一个类似的协方差平稳性条件:

$\alpha_1 + \sum_{j=1}^{q}\beta_{j} < 1$

这个条件确保了过去的冲击对当前条件方差的影响会随时间衰减,使得条件方差的长期均值为$\frac{\omega}{1-\sum_{j=1}^{q}\beta_{j}}$。

这个结果再次展示了GARCH模型的优雅之处。通过引入有限个参数,GARCH模型可以生成一个复杂的波动率过程,其中当前的条件方差依赖于无穷多个过去的squared residuals。这使得GARCH模型成为捕捉金融市场波动率动态的强大工具。

---

好的,让我们来推导GARCH(p,q)模型的无穷级数表示。

GARCH(p,q)模型的一般形式为:

$\sigma_{t}^2 = \omega + \sum_{i=1}^{p}\alpha_{i}R_{t-i}^2 + \sum_{j=1}^{q}\beta_{j}\sigma_{t-j}^2$

我们可以通过不断地替换右边的$\sigma_{t-j}^2$项来展开这个模型:

$\sigma_{t}^2 = \omega + \sum_{i=1}^{p}\alpha_{i}R_{t-i}^2 + \sum_{j=1}^{q}\beta_{j}(\omega + \sum_{i=1}^{p}\alpha_{i}R_{t-i-j}^2 + \sum_{k=1}^{q}\beta_{k}\sigma_{t-j-k}^2)$

通过不断地迭代这个过程,我们最终会得到一个只包含$R_{t-i}^2$项和常数项的无穷级数:

$\sigma_{t}^2 = \omega(1 + \sum_{i=1}^{\infty}(\sum_{j=1}^{q}\beta_{j})^i) + \sum_{i=1}^{\infty}(\sum_{j=1}^{q}\beta_{j})^{i-1}(\sum_{k=1}^{p}\alpha_{k}R_{t-k-i+1}^2)$

这个结果可以进一步简化。使用几何级数的和公式:

$\sum_{i=1}^{\infty}ar^{i-1} = \frac{a}{1-r}, |r|<1$

我们可以将无穷级数简化为:

$\sigma_{t}^2 = \frac{\omega}{1-\sum_{j=1}^{q}\beta_{j}} + \sum_{k=1}^{p}\alpha_{k}\sum_{i=1}^{\infty}(\sum_{j=1}^{q}\beta_{j})^{i-1}R_{t-k-i+1}^2$

这个结果表明,在GARCH(p,q)模型中,当前的条件方差可以表示为过去所有squared residuals的加权和,权重以几何速度衰减。衰减速度由$\sum_{j=1}^{q}\beta_{j}$决定,而$\alpha_{1}, \alpha_{2}, \ldots, \alpha_{p}$决定了每个滞后的squared residual的初始权重。

为了确保这个无穷级数收敛,我们需要一个协方差平稳性条件:

$\sum_{i=1}^{p}\alpha_i + \sum_{j=1}^{q}\beta_{j} < 1$

这个条件确保了过去的冲击对当前条件方差的影响会随时间衰减,使得条件方差的长期均值为$\frac{\omega}{1-\sum_{j=1}^{q}\beta_{j}}$。

这个结果展示了GARCH(p,q)模型的一般性。通过引入p个ARCH项和q个GARCH项,GARCH(p,q)模型可以生成一个非常灵活的波动率过程,其中当前的条件方差依赖于无穷多个过去的squared residuals,且这种依赖关系可以有多个不同的初始权重和衰减速度。这种灵活性使得GARCH(p,q)模型成为捕捉各种不同型态的波动率动态的强大工具。

---

好的,让我们来迭代展开GARCH(1,1)模型的$\sigma_{t+1}^2$。

GARCH(1,1)模型为:

$\sigma_{t+1}^2 = \omega + \alpha_{1}R_{t}^2 + \beta_{1}\sigma_{t}^2$

我们首先替换$\sigma_{t}^2$:

$\sigma_{t+1}^2 = \omega + \alpha_{1}R_{t}^2 + \beta_{1}(\omega + \alpha_{1}R_{t-1}^2 + \beta_{1}\sigma_{t-1}^2)$
$= \omega(1 + \beta_1) + \alpha_{1}R_{t}^2 + \alpha_1\beta_1R_{t-1}^2 + \beta_1^2\sigma_{t-1}^2$

然后替换$\sigma_{t-1}^2$:

$\sigma_{t+1}^2 = \omega(1 + \beta_1) + \alpha_{1}R_{t}^2 + \alpha_1\beta_1R_{t-1}^2 + \beta_1^2(\omega + \alpha_{1}R_{t-2}^2 + \beta_{1}\sigma_{t-2}^2)$
$= \omega(1 + \beta_1 + \beta_1^2) + \alpha_{1}R_{t}^2 + \alpha_1\beta_1R_{t-1}^2 + \alpha_1\beta_1^2R_{t-2}^2 + \beta_1^3\sigma_{t-2}^2$

继续这个过程,我们得到:

$\sigma_{t+1}^2 = \omega(1 + \beta_1 + \beta_1^2 + \beta_1^3 + \cdots) + \alpha_{1}R_{t}^2 + \alpha_1\beta_1R_{t-1}^2 + \alpha_1\beta_1^2R_{t-2}^2 + \alpha_1\beta_1^3R_{t-3}^2 + \cdots$

使用几何级数的和公式:

$\sum_{i=0}^{\infty}ar^{i} = \frac{a}{1-r}, |r|<1$

我们可以将无穷级数简化为:

$\sigma_{t+1}^2 = \frac{\omega}{1-\beta_1} + \alpha_1\sum_{i=0}^{\infty}\beta_1^iR_{t-i}^2$

这就是GARCH(1,1)模型$\sigma_{t+1}^2$的无穷级数表示。它表明,在GARCH(1,1)模型中,下一期的条件方差是过去所有squared residuals的加权和,权重以几何速度衰减,衰减速度由$\beta_1$决定。常数项$\frac{\omega}{1-\beta_1}$表示条件方差的长期均值。

为了确保这个无穷级数收敛,我们需要协方差平稳性条件:

$\alpha_1 + \beta_1 < 1$

这个条件确保了过去的冲击对未来条件方差的影响会随时间衰减。这也是GARCH(1,1)模型能够生成均值回复的波动率过程的原因。



根据图片,GARCH(1,1)模型的条件方差公式为:

$\sigma_{t+1}^2 = \omega + \alpha R_t^2 + \beta \sigma_t^2, with \alpha + \beta < 1$

其中 $\omega = \sigma^2(1 - \alpha - \beta)$,而 $\sigma^2$ 是长期平均方差。

图片还提到,如果我们令 $\alpha = 1 - \lambda, \beta = \lambda$,使得 $\alpha + \beta = 1$,并且 $\omega = 0$,那么RiskMetrics模型可以被视为简单GARCH模型的一个特例。

这表明,RiskMetrics模型是GARCH(1,1)模型的一个特殊情况,其中参数满足特定的限制条件。在这些条件下,RiskMetrics模型可以用更简单的形式表示GARCH(1,1)过程。

---

在GARCH(1,1)模型中,$\omega$表示条件方差的长期均值。让我们来推导为什么$\omega = \sigma^2(1 - \alpha - \beta)$。

我们从GARCH(1,1)模型的条件方差公式开始:

$\sigma_{t+1}^2 = \omega + \alpha R_t^2 + \beta \sigma_t^2$

如果这个过程是协方差平稳的,那么从长远来看,我们期望条件方差等于它的无条件均值,也就是$E(\sigma_{t+1}^2) = E(\sigma_t^2) = \sigma^2$。

将这个结果代入条件方差公式:

$\sigma^2 = \omega + \alpha E(R_t^2) + \beta \sigma^2$

注意到$E(R_t^2) = \sigma^2$,因为$R_t$是一个均值为0,方差为$\sigma^2$的过程。

代入$E(R_t^2) = \sigma^2$:

$\sigma^2 = \omega + \alpha \sigma^2 + \beta \sigma^2$

整理这个等式:

$\sigma^2 = \omega + (\alpha + \beta) \sigma^2$
$\sigma^2 - (\alpha + \beta) \sigma^2 = \omega$
$(1 - \alpha - \beta) \sigma^2 = \omega$

最后,我们得到:

$\omega = \sigma^2(1 - \alpha - \beta)$

这个结果表明,$\omega$不是一个独立的参数,而是由其他参数($\alpha$,$\beta$)和长期均值$\sigma^2$决定的。这也保证了当$\alpha + \beta < 1$时,条件方差的长期均值存在且为正。

从直观上讲,$\omega$可以被理解为条件方差的"基准水平"。当没有新的信息进入市场时($R_t = 0$),条件方差会收敛到这个水平。$\alpha$和$\beta$则决定了条件方差围绕这个基准水平波动的幅度和持续性。

这个结果也强调了GARCH模型的一个重要特性:尽管条件方差在短期内会对新信息做出反应,但从长远来看,它总是倾向于回归到一个常数水平。这种均值回复的特性使得GARCH模型能够生成与我们在金融市场中观察到的波动率动态相一致的过程。

---

在这个推导中，我们可以看到 $\sigma_{t+1}^2$ 被分解成了两部分：$(1-\lambda)R_t^2$ 和 $\sum_{r=1}^{\infty} (1-\lambda)\lambda^{r}R_{t-r}^2$。

第二部分 $\sum_{r=1}^{\infty} (1-\lambda)\lambda^{r}R_{t-r}^2$ 可以进一步化简：

$\sum_{r=1}^{\infty} (1-\lambda)\lambda^{r}R_{t-r}^2$ $= (1-\lambda)\lambda \sum_{r=0}^{\infty} \lambda^{r}R_{t-1-r}^2$ $= (1-\lambda)\lambda \sigma_{t}^2$

所以，我们可以将 $\sigma_{t+1}^2$ 写成：

$\sigma_{t+1}^2 = (1-\lambda)R_t^2 + (1-\lambda)\lambda \sigma_{t}^2$

比较这个式子和原始的式子：

$\sigma_{t+1}^2 = (1-\lambda)R_t^2 + \beta \sigma_{t}^2$

我们可以得出：

$\beta = (1-\lambda)\lambda$

或者，用 $\lambda$ 表示 $\beta$：

$\lambda = 1 - \frac{\beta}{1-\beta}$

这就是 $\beta$ 用 $\lambda$ 表示的方式。

---

好的，让我们重新推导一次，用 $F(\lambda,r)$ 替换 $(1-\lambda)\lambda^r$。

$\sigma_{t+1}^2 = \sum_{r=0}^{\infty} F(\lambda,r)R_{t-r}^2$

$F(\lambda,0)R_t^2 +F(\lambda,1)R_{t-1}^2 + \sum_{r=1}^{\infty} F(\lambda,r)R_{t-r}^2$

$= F(\lambda,0)R_t^2 + \sum_{r=0}^{\infty} F(\lambda,r+1)R_{t-1-r}^2$

现在，让我们定义一个新的函数 $G(\lambda)$，使得：

$G(\lambda) = \sum_{r=0}^{\infty} F(\lambda,r+1)$

则：

$\sigma_{t+1}^2 = F(\lambda,0)R_t^2 + G(\lambda) \sum_{r=0}^{\infty} \frac{F(\lambda,r+1)}{G(\lambda)} R_{t-1-r}^2$

$= F(\lambda,0)R_t^2 + G(\lambda) \sigma_t^2$

因此，我们可以得出：

$\beta = G(\lambda) = \sum_{r=0}^{\infty} F(\lambda,r+1)$

这就是用一般函数 $F(\lambda,r)$ 替换 $(1-\lambda)\lambda^r$ 后的推导结果。$\beta$ 可以表示为 $F(\lambda,r)$ 的无限和。

---


$\sigma_{t+1}^2 = F(\lambda,0)R_t^2 + F(\lambda,1)R_{t-1}^2 + \sum_{r=1}^{\infty} F(\lambda,r)R_{t-r}^2$

$= F(\lambda,0)R_t^2 + F(\lambda,1)R_{t-1}^2 + \sum_{r=0}^{\infty} F(\lambda,r+2)R_{t-2-r}^2$

现在，让我们定义一个新的函数 $H(\lambda)$，使得：

$H(\lambda) = \sum_{r=0}^{\infty} F(\lambda,r+2)$

则：

$\sigma_{t+1}^2 = F(\lambda,0)R_t^2 + F(\lambda,1)R_{t-1}^2 + H(\lambda) \sum_{r=0}^{\infty} \frac{F(\lambda,r+2)}{H(\lambda)} R_{t-2-r}^2$

$= F(\lambda,0)R_t^2 + F(\lambda,1)R_{t-1}^2 + H(\lambda) \sigma_{t-1}^2$

在这个式子中，我们没有一个单独的项可以表示为 $\beta \sigma_t^2$ 的形式。因此，我们需要对 $\sigma_t^2$ 进行展开：

$\sigma_t^2 = F(\lambda,0)R_{t-1}^2 + F(\lambda,1)R_{t-2}^2 + H(\lambda) \sigma_{t-2}^2$

将这个展开式代入原式：

$\sigma_{t+1}^2 = F(\lambda,0)R_t^2 + F(\lambda,1)R_{t-1}^2 + H(\lambda) (F(\lambda,0)R_{t-1}^2 + F(\lambda,1)R_{t-2}^2 + H(\lambda) \sigma_{t-2}^2)$

$= F(\lambda,0)R_t^2 + (F(\lambda,1) + H(\lambda)F(\lambda,0))R_{t-1}^2 + H(\lambda)F(\lambda,1)R_{t-2}^2 + H(\lambda)^2 \sigma_{t-2}^2$

在这个式子中，我们仍然没有一个单独的项可以表示为 $\beta \sigma_t^2$ 的形式。事实上，由于我们改变了原始的式子，引入了 $R_{t-1}^2$ 项，使得我们无法得到一个简单的 $\beta$ 表达式。

这个例子说明，改变原始的式子结构可能会导致无法得到一个简单的 $\beta$ 表达式。原始的式子之所以能够得到简单的 $\beta$ 表达式，是因为它的结构允许我们将 $\sigma_{t+1}^2$ 表示为 $R_t^2$ 和 $\sigma_t^2$ 的线性组合。
$$ \begin{aligned} 
\boxed{ \begin{aligned} c &\in D \ A &+ B \end{aligned} } \

\boxed{ \begin{aligned} c &\in D \ A &+ B \end{aligned} } 

\end{aligned}
$$





```tikz
\usetikzlibrary{decorations.pathreplacing}

\begin{document}
\begin{tikzpicture}
\draw[thick]  (3.75,9.5) rectangle (6.25,8.25);
\fill[green,opacity=0.25] (3.75,9.5) rectangle (6.25,8.25);

\draw[thick]  (3.75,8.25) rectangle (8.75,7);
\fill[blue,opacity=0.25] (3.75,8.25) rectangle (8.75,7);

\draw[thick]  (3.75,7) rectangle (11.25,5.75);
\fill[yellow,opacity=0.25]  (3.75,7) rectangle (11.25,5.75);

\draw[thick]  (3.75,5.75) rectangle (13.75,4.5);
\fill[red,opacity=0.25]  (3.75,5.75) rectangle (13.75,4.5);

% 红色矩形 (底部)
\draw[thick] (3.75,3.875-0.75) rectangle (13.75,2.625-0.75);  
\fill[red,opacity=0.75] (3.75,3.875-0.75) rectangle (13.75,2.625-0.75);
\node [font=\LARGE] at (8.75,2.5) {$\sigma^2_{t+1}$};

% 黄色矩形 (第三层)
\draw[thick] (3.75,3.875-0.5) rectangle (11.25,2.625-0.5);
\fill[yellow,opacity=0.75] (3.75,3.875-0.5) rectangle (11.25,2.625-0.5);
\node [font=\LARGE] at (7.5,2.75) {$\beta_{0}\sigma^2_{t}$};

% 蓝色矩形 (第二层)
\draw[thick] (3.75,3.875-0.25) rectangle (8.75,2.625-0.25);
\fill[blue,opacity=0.75] (3.75,3.875-0.25) rectangle (8.75,2.625-0.25);
\node [font=\LARGE] at (6.25,3) {$\beta_{1}\sigma^2_{t-1}$};

% 绿色矩形 (最上面)
\draw[thick] (3.75,3.875) rectangle (6.25,2.625);
\fill[green,opacity=0.75] (3.75,3.875) rectangle (6.25,2.625);
\node [font=\LARGE] at (5,3.25) {$\beta_{2}\sigma^2_{t-2}$};

\node [font=\LARGE] at (5,10.125) {$\alpha_{3}R^2_{t-3}$};
\node [font=\LARGE] at (7.5,10.125) {$\alpha_{2}R^2_{t-2}$};
\node [font=\LARGE] at (10,10.125) {$\alpha_{1}R^2_{t-1}$};
\node [font=\LARGE] at (12.5,10.125) {$\alpha_{0}R^2_{t}$};
\node [font=\LARGE] at (16.25,10.125) {$=ARCH(0,4)$};

\node [font=\LARGE] at (5,8.875) {$\beta_{2}\sigma^2_{t-2}$};
\node [font=\LARGE] at (7.5,8.875) {$\alpha_{2}R^2_{t-2}$};
\node [font=\LARGE] at (10,8.875) {$\alpha_{1}R^2_{t-1}$};
\node [font=\LARGE] at (12.5,8.875) {$\alpha_{0}R^2_{t}$};

\node [font=\LARGE] at (6.25,7.625) {$\beta_{1}\sigma^2_{t-1}$};
\node [font=\LARGE] at (10,7.625) {$\alpha_{1}R^2_{t-1}$};
\node [font=\LARGE] at (12.5,7.625) {$\alpha_{0}R^2_{t}$};

\node [font=\LARGE] at (7.5,6.375) {$\beta_{0}\sigma^2_{t}$};
\node [font=\LARGE] at (12.5,6.375) {$\alpha_{0}R^2_{t}$};

\node [font=\LARGE] at (8.75,5.125) {$\sigma^2_{t+1}$};


\node [font=\LARGE] at (16.25,5.125) {$=GARCH(1,0)$};
\node [font=\LARGE] at (16.25,6.375) {$=GARCH(1,1)$};
\node [font=\LARGE] at (16.25,7.625) {$=GARCH(1,2)$};
\node [font=\LARGE] at (16.25,8.875) {$=GARCH(1,3)$};
\node [font=\LARGE] at (16.25,2.5) {$=GARCH(4,0)$};

\end{tikzpicture}
\end{document}
```




$$
\begin{aligned} 
\sigma_{R_{t+1}} &\xlongequal{\text{conversion of length of time}}\sigma_{\text{daily}} \times \sqrt{\tau}\xlongequal{\text{Covariance rate - usually denotes the mathematical definition of a single day's return}} \sigma_{t+1}^2 \\ 
 &\xlongequal{\text{common definition of  return}}E[R_{t+1} - E(R_{t+1})]^2= \frac{1}{T_{\text{total}}} \sum_{\tau=0}^{T_{\text{total}}-1} (R_{t-\tau} - \mu_{R_{t+1}})^2\\
&\xlongequal{\text{simple weighted average model}}\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^{2},\space (s.t.\space \mu_{R_{t+1}}=0,\space\text{Includes current period})\\
&\xlongequal{\text{ARCH(q)}}\frac{1}{q}\sum_{\tau=0}^{q-1}R_{t-\tau}^{2}=\sum_{\tau=0}^{q-1}\frac{1}{q}R_{t-\tau}^{2}\xlongequal{\text{wide sense}}\sum_{\tau=0}^{q-1}\alpha_{\tau}R_{t-\tau}^{2}=\sum_{\tau=0}^{q-1}F(\lambda,\tau)R_{t-\tau}^{2},\space(\text{Not includes constant})\\
&\xlongequal{\text{GARCH(p,q)}}\sum_{\tau=0}^{T-1}F(\lambda,\tau)R_{t-\tau}^{2}=ARCH(q)+ARCH_{G}(p=1)\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=\sum_{\tau=0}^{q-1}F(\lambda,\tau)R_{t-\tau}^{2}+\sum_{\tau=1}^{T-q}F(\lambda,\tau+q+1)\sigma_{t-q+1}^{2}\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\xlongequal{\text{Expand to item p}}\sum_{\tau=0}^{q-1}F(\lambda,\tau)R_{t-\tau}^{2}+\sum_{p=q}^{\max:T-1}\sum_{\tau=1}^{T-p}F(\lambda,\tau+p+1)\sigma_{t-p+1}^{2}\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=\sum_{\tau=0}^{q-1}\alpha_{\tau}R_{t-\tau}^{2}+\sum_{p=q}^{\max:T-1}\beta_{r,p}\sigma_{t-p+1}^{2}\\
&\xlongequal{\text{TARCH(p,q)}}\sum_{\tau=0}^{T-1}F(\lambda,\tau)R_{t-\tau}^{2}=ARCH(q^+,q^-)++ARCH_{G}(p)\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\xlongequal{\text{Embodying leverage}}\sum_{\tau=0}^{q-1}[F(\lambda,\tau)+\gamma d_{\tau}]R_{t-\tau}^{2}+\sum_{p=q}^{\max:T-1}\sum_{\tau=1}^{T-p}F(\lambda,\tau+p+1)\sigma_{t-p+1}^{2}\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=\sum_{\tau=0}^{q-1}(\alpha_{\tau}+\gamma d_{\tau})R_{t-\tau}^{2}+\sum_{p=q}^{\max:T-1}\beta_{r,p}\sigma_{t-p+1}^{2}\\
\end{aligned}
$$

---

介绍了对S&P 500日收盘价从2001年1月1日至2010年12月31日的风险分析模型。定义了收益率序列 $R_t = \ln(\frac{Price_t}{Price_{t-1}})$ 和收益率平方序列 $R^2_t = R_t^2$。
通过观察 $R^2_t$ 的自相关图,发现存在明显的正自相关,表明未来的收益率方差可能是可预测的。同时观察到收益率平方图呈现出波动聚集(平静期后是波动期)的特点。
使用GARCH(1,1)模型来拟合数据: $\sigma^2_{t+1} = 0.000 + 0.078R^2_t + 0.913\sigma^2_t$
介绍了有效市场假说(EMH),认为不可能利用历史信息预测未来收益。通过回归 $R_t = \alpha + \beta R_{t-1} + \epsilon_t$ 来检验EMH。OLS估计得到 $R_{t-1}$ 显著为负,拒绝EMH。
考虑误差项 $\epsilon_t$ 存在ARCH效应和GARCH效应的情形,模型变为: $R_t = \alpha + \beta R_{t-1} + \epsilon_t$ $Var(\epsilon_t | \epsilon_{t-1}) = \omega + \alpha_1 \epsilon^2_{t-1}$ (ARCH(1)) $Var(\epsilon_t | \epsilon_{t-1}) = \omega + \alpha_1 \epsilon^2_{t-1} + \beta_1 \sigma^2_{t-1}$ (GARCH(1,1))
使用 Stata 估计上述模型, 结果显示 $\epsilon^2_{t-1}$ 和 $\sigma^2_{t-1}$ 的系数显著为正, 同时 $R_{t-1}$ 依然显著为负, 再次拒绝 EMH。其中 $\epsilon^2_{t-1}$ 的系数称为 ARCH 参数, $\sigma^2_{t-1}$ 的系数称为 GARCH 参数

---

在金融时间序列数据中,我们经常观察到收益率的波动率(方差)呈现出随时间变化的特点,即异方差性(heteroskedasticity)。为了刻画这种现象,Engle(1982)提出了自回归条件异方差(ARCH)模型。

假设误差项 $\epsilon_t$ 服从ARCH(1)过程,那么给定过去一期的信息 $\epsilon_{t-1}$,当期误差项的条件方差可以表示为:

$$Var(\epsilon_t | \epsilon_{t-1}) = \omega + \alpha_1 \epsilon^2_{t-1}$$

其中, $\omega > 0$, $\alpha_1 \geq 0$ 以保证条件方差非负。

这个公式可以这样理解:

1. $\omega$ 表示一个常数项,即误差项的基础波动水平。
2. $\alpha_1 \epsilon^2_{t-1}$ 表示 $t-1$ 期的squared shock(平方误差项)对当期波动率的影响。 $\alpha_1$ 衡量了这种影响的大小。
3. $\epsilon^2_{t-1}$ 越大,表明上一期的意外冲击越大,根据ARCH模型的设定,这会导致当期的波动率上升。这与我们在金融市场上观察到的波动聚集现象相一致。
4. ARCH(1)模型假设当期的条件方差只依赖于上一期的squared shock,更一般地,ARCH(q)模型允许条件方差依赖于过去q期的squared shock:

$$Var(\epsilon_{t+1} | \epsilon_{t}, \cdots, \epsilon_{t-q}) =  \alpha_0 \epsilon^2_{t} + \cdots + \alpha_q \epsilon^2_{t-q}= \sum_{i=0}^{q} \alpha_i \epsilon_{t-i}^2$$

5. 为了使条件方差在所有时点都是正的,需要对参数施加限制: $\omega > 0$, $\alpha_i \geq 0$, $i=1,\cdots,q$, 且 $\sum_{i=1}^q \alpha_i < 1$。

因此,ARCH  模型通过引入误差项的条件方差方程,提供了一种描述金融时间序列异方差特征的系统方法。在估计  ARCH  族模型时,通常采用极大似然估计方法。





