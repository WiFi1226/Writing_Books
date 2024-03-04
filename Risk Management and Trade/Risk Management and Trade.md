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
The $\bar{x},\bar{y},var(x),var(y),\rho_{x,y},\beta{i}$ for each set of data are almost the same,but their distributions are significantly different.

# Return and Price
## Definition 
$$
\begin{align}
\because S_{t}=P_{t} \xrightarrow{\text{first-order difference form}} R_{t}=\Delta S_{t} \xrightarrow{\text{logarithmisation by means of a linear}} R_{t}=\Delta ln(S_{t}) \xrightarrow{\text{ODF => OR(Ratio)F}} R_{t}&=ln(\frac{S_{t}}{S_{t-1}}) \\
=ln(\frac{S_{t}-S_{t-1}}{S_{t-1}}+1) \\
=ln(r+1) \\
&\sim r(\text{Short-term or low yields}) \\
\
\end{align}
$$

$$
\begin{align}
&x + y &&= 10 \\
&2x - y &&= 5 \\
&&&= 3x + 2
\end{align}

$$$


为什么差分形式可以移除线性趋势
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
- 
- **Rate of Return ($r_t$)**
	- Model:
$$
\begin{aligned}
r_{t}=\frac{S_{t}-S_{t-1}}{S_{t-1}}
\end{aligned}
$$














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
