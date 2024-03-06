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

1. 首先给出了 RiskMetrics 波动率模型的通用公式表示:
$$\sigma_{t+1}^2 = (1 - \lambda) \sum_{\tau=1}^\infty \lambda^{\tau-1} R_{t+1-\tau}^2$$
其中 $0 < \lambda < 1$, $\sigma_{t+1}^2$ 是未来时间点 $t+1$ 的方差预测值, $R_{t+1-\tau}$ 是 $t+1-\tau$ 时间点的回报率。

2. 将无穷求和部分提取出来, 用递推关系式表示:
$$\sigma_{t+1}^2 = \lambda \sigma_t^2 + (1-\lambda)R_t^2$$
其中 $\sigma_t^2$ 是当前时间点 $t$ 的方差, $R_t^2$ 是当前时间点的回报率平方。

3. 解释了这一递推式的意义: $\sigma_{t+1}^2$ 是明天的波动性预测值, 是当天波动性 $\sigma_t^2$ 和当天回报率平方 $R_t^2$ 的加权平均。

4. 通过这一递推关系, 只要观测到当前时间点的波动率 $\sigma_t^2$ 和回报率平方 $R_t^2$, 就可以预测未来任意时间点的波动率 $\sigma_{t+1}^2$。

5. 权重参数 $\lambda$ 控制了过去观测值的贡献大小。$\lambda$ 越大, 过去观测值的影响越持久; $\lambda$ 越小, 新观测值的影响越大。

综上, 该简化过程利用了加权平均的递推思想, 将 RiskMetrics 波动率模型从无穷求和形式化简为易于计算和预测的递归形式, 并阐明了参数 $\lambda$ 的作用机制。这一简化使得实际运算和参数估计更加直观和方便。




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
