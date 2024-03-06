---
Date: 19th February 2024
Doctype: Note
Category: 02_Writing Books/Risk Management and Trade/Risk Management and Trade.md
Status: Draft
Abstract:
Keywords:
tags:
Version: 1.0.0
License: MIT
Copyright: © 2024 FAN WANG. All rights reserved.
---
# Preliminaries
## Anscombe's quartet
**Problems when analysing using only basic statistical properties - the importance of visualisation
![[anscombes_quartet_transparent.svg]]
The $\bar{x},\bar{y},var(x),var(y),\rho_{x,y},\beta_{i}$ for each set of data are almost the same,but their distributions are significantly different.

# Return($R_{t+1}$) 

## Statistical characterisation of $R_{t,\text{past}}$   tips: $\tau\text{ is step length}$
-  **Autocorrelation: $Corr(R_{t},R_{t-\tau})\approx 0, \tau \in \mathbb{Q}$
- **Moment statistic
	- **Mean**: Small compared to standard deviation so not a concern 
	- **Standard deviation (volatility)**: 
		- Squared Returns: Balancing positive and negative effects ==> Emphasis on extreme movements 
			- $Corr(R_{t}^2,R_{t-\tau}^2)\approx 0, \tau \in \mathbb{Q}$
			- larger price movements (both positive and negative) contribute more to total volatility than smaller movements.
	- **Skewness**: Negative skewness
	- **Kurtosis**: Extreme kurtosis ==> fat tail phenomenon
![[normal_high_kurtosis_neg_skew.svg]]
## Mathematical definition ($R_{t+1}$)
$$
\begin{align}
\because\space&S_{t}=P_{t} \xrightarrow{\text{first-order difference form}} R_{t}=\Delta S_{t} \xrightarrow{\text{logarithmisation by means of a linear}} R_{t}=\Delta ln(S_{t}) \xrightarrow{\text{ODF => OR(Ratio)F}} R_{t}=ln(\frac{S_{t}}{S_{t-1}}) \\
\therefore\space&S_{t+1}=S_{t}·(1+r_{t+1})\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=ln(\frac{S_{t}-S_{t-1}}{S_{t-1}}+1) \\
&S_{t+1}=S_{t}·\exp(R_{t+1})\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\thinspace\space\space\space\space\space\space\space=ln(r+1) \\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\thinspace\sim r(\text{Short-term or low yields}) \\
\end{align}
$$
## Common definition ($R_{t+1}$)   tips: $N=\frac{T_{total}}{\tau}, \tau\text{ is interval}$
- ? 这个定义咋回事, R^2 不关注了吗, 是否等于 0 是在 R^2 的基础上要求的
$$
\begin{aligned}
R_{t+1}\xlongequal{\text{Expected returns } (\mu) \text{ and stochastic volatility returns }(\sigma)}&\mu_{t+1}+\sigma_{t+1}z_{t+1},\space with \space z_{t+1} \sim i.i.d.\space \mathcal{N}(0,1)\\
\\
% 一阶中心矩（期望值）的推导 
 \mu_{R_{t+1}} &= E(R_{t+1})= \frac{1}{T_{\text{total}}} \sum_{i=1}^{{T_{\text{total}}}}R_i\\
 &= E(\mu_{t+1} + \sigma_{t+1}z_{t+1})\\
 &= \mu_{t+1} + \sigma_{t+1}E(z_{t+1})\\
 &= \mu_{t+1} + \sigma_{t+1} \times 0\\
 &= \mu_{t+1}\\
\\
% 二阶中心矩（方差）的推导
\sigma_{R_{t+1}}&= E[R_{t+1} - E(R_{t+1})]^2= \frac{1}{T_{\text{total}}} \sum_{i=1}^{{T_{\text{total}}}} (R_i - \mu_{R_{t+1}})^2\\
 &= E(\mu_{t+1} + \sigma_{t+1}z_{t+1} - \mu_{t+1})^2\\
 &= E(\sigma_{t+1}z_{t+1})^2\\
 &= \sigma_{t+1}^2 E(z_{t+1})^2\\
 &= \sigma_{t+1}^2 \times 1\\
 &= \sigma_{t+1}^2\\
 \\
\sigma_{R_{t+1}^2} &= E(\mu_{t+1} + \sigma_{t+1}z_{t+1})^2 \\
&= E(\mu_{t+1}^2 + 2\mu_{t+1}\sigma_{t+1}z_{t+1} + \sigma_{t+1}^2z_{t+1}^2) \\
&= \mu_{t+1}^2 + 2\mu_{t+1}\sigma_{t+1}E(z_{t+1}) + \sigma_{t+1}^2E(z_{t+1})^2\\
&= \mu_{t+1}^2 + 2\mu_{t+1}\sigma_{t+1} \times 0 + \sigma_{t+1}^2 \times 1 \\
&= \mu_{t+1}^2 + \sigma_{t+1}^2
\end{aligned}
$$
Since $\sigma_{R_{t+1}}$ (i.e. the volatility of returns) is the main objective of the modelling, we shift the objective to $\sigma_{t+1}^2$.

# Volatility ($\sigma_{R_t}$)
## Common definition
- **Standard deviation of asset returns**
$$
\begin{aligned} 
\sigma_{R_{t+\tau}} &\xlongequal{\text{common definition of  return}}  E[R_{t+\tau} - E(R_{t+\tau})]^2= \frac{1}{\frac{T_{\text{total}}}{\tau}} \sum_{i=1}^{\frac{T_{\text{total}}}{\tau}} (R_i - \mu_{R_{t+\tau}})^2
\\ 
&\xlongequal{\text{Covariance rate - usually denotes the mathematical definition of a single day's return}} \sigma_{t+\tau}^2 \\ 
&\xlongequal{\text{conversion of length of time}}\sigma_{\text{daily}} \times \sqrt{\tau}
\end{aligned}
$$


Therefore, we have (Simple models treated as equivalent weights) 
$$
\begin{aligned}
\sigma_{R_{t+1}} =\sigma^2_{t+1}&= \frac{1}{T_{total}}\sum_{i=1}^{T_{total}}(R_i - \mu_{R_{t+1}})^2\\
&\xlongequal{\text{simple weighted average model}}\frac{1}{T_{total}}\sum_{j=0}^{T_{total}-1}R_{t-j}^{2,},\space (s.t.\space \mu_{R_{t+1}}=0,\space\text{Includes current period})
\end{aligned}
$$


这个简化过程可以分解为以下几个步骤:

好的, 我来详细解释一下 RiskMetrics 模型公式的推导过程:

RiskMetrics 模型最初的波动率公式是:

$$\sigma_{t+1}^2 = (1-\lambda)\sum_{\tau=1}^\infty \lambda^{\tau-1} R_{t+1-\tau}^2 \tag{1}$$

其中:
- $\sigma_{t+1}^2$ 是时间 t+1 的条件方差 (波动率)预测值
- $R_{t+1-\tau}$ 是落后 $\tau$ 期的回报率平方
- $\lambda$ 是衰减因子, 满足 $0<\lambda<1$

我们想将其化简为关于当期波动率和回报率平方的递归形式。

第一步: 将求和部分单独提取出来, 设为:
$$\nu_t = \sum_{\tau=1}^\infty \lambda^{\tau-1} R_{t-\tau+1}^2$$

这实际上将当期及之前所有回报率的加权平均定义为 $\nu_t$。

第二步: 将 $\nu_t$ 代入原方程:
$$\sigma_{t+1}^2 = (1-\lambda)\nu_t$$

第三步: 滞后一期, 得到:
$$\sigma_t^2 = (1-\lambda)\nu_{t-1}$$

第四步: 将 $\nu_{t-1}$ 展开表示为:
$$\nu_{t-1}=\sum_{\tau=1}^\infty \lambda^{\tau-1} R_{t-\tau}^2 = \lambda \sum_{\tau=0}^\infty \lambda^{\tau} R_{t-\tau}^2 = \lambda \nu_t + \lambda R_t^2$$


好的, 我来详细解释第四步的推导过程:

第三步我们得到:
$$\sigma_t^2 = (1-\lambda)\nu_{t-1}$$

其中 $\nu_{t-1}$ 表示:
$$\nu_{t-1} = \sum_{\tau=1}^\infty \lambda^{\tau-1} R_{t-\tau+1}^2$$

我们希望将 $\nu_{t-1}$ 表示为当期 $\nu_t$ 和当期回报率平方 $R_t^2$ 的函数。

首先注意到 $\nu_{t-1}$ 的求和下标从 $\tau=1$ 开始, 我们可以将它分成两部分:

$$\nu_{t-1} = R_t^2 + \sum_{\tau=2}^\infty \lambda^{\tau-1}R_{t-\tau+1}^2$$

对第二部分, 我们令 $\tau' = \tau - 1$, 则有:

$$\sum_{\tau=2}^\infty \lambda^{\tau-1}R_{t-\tau+1}^2 = \sum_{\tau'=1}^\infty \lambda^{\tau'}R_{t-\tau'}^2$$

合并上面两个式子, 得到:

$$\nu_{t-1} = R_t^2 + \sum_{\tau'=1}^\infty \lambda^{\tau'} R_{t-\tau'}^2$$

现在让我们定义 $\nu_t$ 为:

$$\nu_t = \sum_{\tau'=1}^\infty \lambda^{\tau'} R_{t-\tau'+1}^2$$

你会发现, 通过令 $\tau' \rightarrow \tau'-1$, 我们有:

$$\nu_{t-1} = R_t^2 + \lambda \nu_t$$

将这一结果代回第三步的方程, 我们得到:

$$\sigma_t^2 = (1-\lambda)(R_t^2 + \lambda \nu_t)$$

进一步展开并整理, 就能得到最终的递推式:

$$\boxed{\sigma_t^2 = \lambda\sigma_{t+1}^2 + (1-\lambda)R_t^2}$$

关键步骤是通过改变求和变量和下标, 将 $\nu_{t-1}$ 分解表示为当期回报率平方和滞后一期的 $\nu_t$ 的线性组合, 进而推导出两个波动率之间的递推关系。这利用了级数循环节性的性质。






因为级数 $\sum \lambda^\tau$ 是等比级数, 和为 $\frac{1}{1-\lambda}$, 所以前项可化为 $\lambda \nu_t$。

将这一表达式代回第三步, 得:

$$\sigma_t^2 = (1-\lambda)(\lambda\nu_t + \lambda R_t^2)$$

整理可得:
$$\sigma_t^2 = \lambda(1-\lambda)\nu_t + \lambda(1-\lambda)R_t^2$$

进一步整理为:
$$\boxed{\sigma_t^2 = \lambda \sigma_{t+1}^2 + (1-\lambda)R_t^2}$$

# Models
## Unit Root
### Unit Root Fundamental - Sequence instability
Test ways:
- **Augmented Dickey-Fuller (ADF) Test & Phillips-Perron (PP) Test**
	- Model:$$
\begin{aligned}
\Delta Y_t = \alpha Y_{t-1} + \beta \Delta Y_{t-1} + \gamma_1 \Delta Y_{t-2} + \ldots + \gamma_p \Delta Y_{t-p} + \epsilon_t
\end{aligned}
$$
	- Target: The goal of the PP test is similar to that of the ADF test, but the sequence correlation of the sequence is considered, including the correction term to deal with the sequence correlation problem.
- **Kwiatkowski-Phillips-Schmidt-Shin (KPSS) Test**
	- Model:$$
\begin{aligned}
Level check&:Y_t = \mu + \epsilon_t 
\\ Trend check&:Y_t = \mu + \beta t + \epsilon_t
\end{aligned}
$$
	- Target: The goal of the KPSS test is to test stationarity, and if the level test rejects the unit root hypothesis, the sequence may be stationary. If the trend test rejects the unit root trend, the sequence may be trending.
- 
- 
- 


# GARCH (1,1) Model

The Generalized Autoregressive Conditional Heteroskedasticity (GARCH) model, specifically GARCH (1,1), is a popular model for estimating the volatility of financial time series. It incorporates both autoregressive and moving average components for modeling the conditional variance of the time series. The GARCH (1,1) model can be represented as follows:

## Model Definition

Given a time series ($y_t$), the GARCH (1,1) model for the conditional variance ($\sigma_t^2$) is defined by two equations:

1. **Mean Equation**:
   $$\begin{aligned}
   y_t = \mu + \epsilon_t
   \end{aligned}$$
   Where:
   - $\mu$  is the mean of the series,
   - $\epsilon_t$ is the error term at time \(t\), which is normally distributed with mean 0 and variance \(\sigma_t^2\).

2. **Variance Equation**:
   $$\begin{aligned}
   \sigma_t^2 = \alpha_0 + \alpha_1 \epsilon_{t-1}^2 + \beta_1 \sigma_{t-1}^2
   \end{aligned}$$
   Where:
   - ($\sigma_t^2$) is the conditional variance at time ($t$),
   - ($\alpha_0 > 0$) (constant term),
   - ($\alpha_1, \beta_1 \geq 0$) are parameters of the model,
   - ($\epsilon_{t-1}^2$) is the squared error term from the previous time period,
   - ($\sigma_{t-1}^2$) is the conditional variance from the previous time period.

## Parameter Constraints

For the GARCH (1,1) model to be stationary and ensure that the conditional variance is positive, the parameters must satisfy the following constraints:

- \(\alpha_0 > 0\),
- \(\alpha_1 + \beta_1 < 1\).

These constraints ensure that the impact of past squared errors and past variances on current variance diminishes over time.

## Application

The GARCH (1,1) model is widely used in financial applications to model and forecast the volatility of asset returns. Its ability to capture the "volatility clustering" phenomenon observed in financial time series makes it particularly useful for risk management, financial derivatives pricing, and strategic asset allocation.
