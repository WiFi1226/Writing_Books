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

## Statistical characterisation of $R_{t}$   tips: $\tau\text{ is measuring past steps}$
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
## Common definition ($R_{t+1}$) 
- ? 这个定义咋回事, R^2 不关注了吗, 是否等于 0 是在 R^2 的基础上要求的
$$
\begin{aligned}
R_{t+1}\xlongequal{\text{Expected returns } (\mu) \text{ and stochastic volatility returns }(\sigma)}&\mu_{t+1}+\sigma_{t+1}z_{t+1},\space with \space z_{t+1} \sim i.i.d.\space \mathcal{N}(0,1)\\
\\
% 一阶中心矩（期望值）的推导 
 \mu_{R_{t+1}} &= E(R_{t+1})= \frac{1}{T_{\text{total}}} \sum_{\tau=0}^{{T_{\text{total}}-1}}R_{t-\tau}\\
 &= E(\mu_{t+1} + \sigma_{t+1}z_{t+1})\\
 &= \mu_{t+1} + \sigma_{t+1}E(z_{t+1})\\
 &= \mu_{t+1} + \sigma_{t+1} \times 0\\
 &= \mu_{t+1}\\
\\
% 二阶中心矩（方差）的推导
\sigma_{R_{t+1}}&= E[R_{t+1} - E(R_{t+1})]^2= \frac{1}{T_{\text{total}}}\sum_{\tau=0}^{{T_{\text{total}-1}}} (R_{t-\tau} - \mu_{R_{t+1}})^2\\
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

# Volatility ($\sigma_{R_{t+1}}$)
## Common definition
- **Standard deviation of asset returns
- ? 对于跳期的分析必要？
$$
\begin{aligned} 
\sigma_{R_{t+1}} &\xlongequal{\text{common definition of  return}}  E[R_{t+1} - E(R_{t+1})]^2= \frac{1}{T_{\text{total}}} \sum_{\tau=0}^{T_{\text{total}}-1} (R_{t-\tau} - \mu_{R_{t+1}})^2\\
&\xlongequal{\text{simple weighted average model}}\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^{2},\space (s.t.\space \mu_{R_{t+1}}=0,\space\text{Includes current period})\\
\\ 
&\xlongequal{\text{Covariance rate - usually denotes the mathematical definition of a single day's return}} \sigma_{t+1}^2 \\ 
&\xlongequal{\text{conversion of length of time}}\sigma_{\text{daily}} \times \sqrt{\tau}
\end{aligned}
$$
### RiskMetrics model
$$
\begin{aligned}
\sigma_{R_{t+1}} =\sigma^2_{t+1}&= \frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}(R_{t-\tau} - \mu_{R_{t+1}})^2=\frac{1}{T_{total}}\sum_{\tau=0}^{T_{total}-1}R_{t-\tau}^{2},\space (s.t.\space \mu_{R_{t+1}}=0,\space\text{Includes current period})\\
&\xlongequal{\text{RiskMetrics model}}(1-\lambda)\sum_{\tau=0}^\infty\lambda^{\tau} R_{t-\tau}^2,\space(s.t.\space(1-\lambda)\times\sum_{\tau=0}^{\infty}\lambda^{\tau}=(1-\lambda)\times \frac{1}{1-\lambda}=1)\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=(1-\lambda)\sum_{\tau-1=0}^\infty\lambda^{\tau-1} R_{t-(\tau-1)}^2\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=(1-\lambda)[R_{t}^2+\sum_{\tau=2}^{\infty} \lambda^{\tau-1} R_{t-(\tau-1)}^2]\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=(1-\lambda)[R_{t}^2+\sum_{\tau-2=0}^{\infty} \lambda^{(\tau-2)+1} R_{(t-1)-(\tau-2)}^2]\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=(1-\lambda)[R_{t}^2+\sum_{\tau'=0}^{\infty} \lambda^{\tau'+1} R_{(t-1)-\tau'}^2]\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=(1-\lambda)[R_{t}^2+\lambda\sum_{\tau'=0}^{\infty} \lambda^{\tau'} R_{(t-1)-\tau'}^2]\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=(1-\lambda)R_{t}^2+\lambda(1-\lambda)\sum_{\tau'=0}^{\infty} \lambda^{\tau'} R_{(t-1)-\tau'}^2\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=\boxed{(1-\lambda)R_{t}^2+\lambda\sigma_{t}^2}\\
\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space= \lambda[\lambda(\sigma^2_{t-1}) + (1 - \lambda)R^2_{t-1}] + (1 - \lambda)R^2_{t} \\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space= (1 - \lambda)(R^2_{t} + \lambda R^2_{t-1}) + \lambda^2 \sigma^2_{t-1}\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space= (1 - \lambda)(R^2_{t} + \lambda R^2_{t-1} + \lambda^2 R^2_{t-2}) + \lambda^3 \sigma^2_{t-2}\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space= (1 - \lambda)(R^2_{t} + \lambda R^2_{t-1} + \lambda^2 R^2_{t-2} + \dots + \lambda^{m-1} R^2_{t-m}) + \lambda^m \sigma^2_{t-m}
\end{aligned}
$$
#### Dominance
1. $\sigma^2_{t-\tau} \propto \frac{1}{\lambda}$ ($\lambda < 1,\space\lambda^m <1$)
2. Contains only one unknown argument: $\lambda$
3. Requires very little data storage. It is only necessary to remember the current estimate of the variance rate and the most recent observation of the market variable value. Old estimates of the variance rate and old market variable values can be discarded.
4. High λ values (close to 1.0) produce daily volatility estimates that are relatively slow ( $1-\lambda$ ) to respond to new information ( $R_{t}^2$ ) provided by daily variation.
5. When $\lambda = 0.94$, daily variance forecasts can be made without estimation. (The weight of today's squared returns isn $1 - \lambda = 0.06$ and the $\omega_{\sigma^2_{t-100}}$ index decays to $(1 - λ)λ99 = 0.000131$. The $\sum_{0}^{99} \omega$ is 0.998, i.e. it contains 99.8% of the weights. Thus only about 100 daily return lags need to be stored .
![[variance_estimate_evolution_correct_time_step.svg]]
- ! 结合指令和结果附SVG图




























---
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
