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
- **Rate of Return ($r_t$)**
	- Model:
$$
\begin{aligned}
r_{t}=\frac{S_{t}-S_{t-1}}{S_{t-1}}
\end{aligned}
$$


