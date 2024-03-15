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
- 

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
&\xlongequal{\text{ARCH(q)}}Var(\epsilon_{t+1} | \epsilon_{t}, \cdots, \epsilon_{t-q+1}) =  \alpha_0 \epsilon^2_{t} + \cdots + \alpha_q \epsilon^2_{t-q+1}= \sum_{i=0}^{q-1} \alpha_i \epsilon_{t-i}^2\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=\sum_{\tau=0}^{q-1}\alpha_{\tau}R_{t-\tau}^{2}=\sum_{\tau=0}^{q-1}F(\lambda,\tau)R_{t-\tau}^{2},\space(s.t.\space \mu_{R_{t+1}}=0,\space\text{Includes current period})\\
&\xlongequal{\text{GARCH(1,q)}}Var(\epsilon_{t+1} | \epsilon_{t}, \cdots, \epsilon_{t-q+1}) =  \alpha_0 \epsilon^2_{t} + \cdots + \alpha_q \epsilon^2_{t-q+1}= \sum_{i=0}^{q-1} \alpha_i \epsilon_{t-i}^2\\
=\sum_{\tau=0}^{T-1}F(\lambda,\tau)R_{t-\tau}^{2}=ARCH(q)+ARCH_{G}(p=1)\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=\sum_{\tau=0}^{q-1}F(\lambda,\tau)R_{t-\tau}^{2}+\sum_{\tau=0}^{T-q-1}F(\lambda,\tau+q)\sigma_{t-q+1}^{2}\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\xlongequal{\text{GARCH(p,q)}}\sum_{\tau=0}^{q-1}F(\lambda,\tau)R_{t-\tau}^{2}+\sum_{q=1}^{p}\sum_{\tau=0}^{T-q-1}F(\lambda,\tau+q)\sigma_{t-q+1}^{2}\space(space\s.t.\space p+q\leq T)\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=\sum_{\tau=0}^{q-1}\alpha_{\tau}R_{t-\tau}^{2}+\sum_{q=1}^{p}\beta_{r,q}\sigma_{t-q+1}^{2}\\
&\xlongequal{\text{TARCH(p,q)}}\sum_{\tau=0}^{T-1}F(\lambda,\tau)R_{t-\tau}^{2}=ARCH(q^+,q^-)+ARCH_{G}(p)\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\xlongequal{\text{Embodying leverage}}\sum_{\tau=0}^{q-1}[F(\lambda,\tau)+\gamma d_{\tau}]R_{t-\tau}^{2}+\sum_{q=1}^{p}\sum_{\tau=0}^{T-q-1}F(\lambda,\tau+q)\sigma_{t-q+1}^{2}\space(s.t.\space p+q\leq T)\\
&\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space\space=\sum_{\tau=0}^{q-1}(\alpha_{\tau}+\gamma d_{\tau})R_{t-\tau}^{2}+\sum_{q=1}^{p}\beta_{r,q}\sigma_{t-q+1}^{2}\\
\end{aligned}
$$
- ? 对于跳期的分析必要？
-  $GARCH(1,1)\xlongequal{F(\lambda,\tau)=\lambda^{\tau}(1-\lambda)}\text{RiskMetrics model}(\text{normalisable},\space\lambda\space\text{constant as}\space0.94)$
	- Dominance： 
		1.  $\sigma^2_{t-\tau} \propto \frac{1}{\lambda}$ ($\lambda < 1,\space\lambda^m <1$)
		2. Contains only one unknown argument: $\lambda$
		3. Requires very little data storage. It is only necessary to remember the current estimate of the variance rate and the most recent observation of the market variable value. Old estimates of the variance rate and old market variable values can be discarded.
		4. High λ values (close to 1.0) produce daily volatility estimates that are relatively slow ( $1-\lambda$ ) to respond to new information ( $R_{t}^2$ ) provided by daily variation.
		5. When $\lambda = 0.94$, daily variance forecasts can be made without estimation. (The weight of today's squared returns isn $1 - \lambda = 0.06$ and the $\omega_{\sigma^2_{t-100}}$ index decays to $(1 - λ)λ99 = 0.000131$. The $\sum_{0}^{99} \omega$ is 0.998, i.e. it contains 99.8% of the weights. Thus only about 100 daily return lags need to be stored .
		6. ![[variance_estimate_evolution_correct_time_step.svg]]
- For CONSTANT values (note the iteration): although the conditional variance responds to new information in the short term, it always tends to return to a constant level in the long term. And because the process is covariance-smooth, the conditional variance is expected to be equal to its unconditional mean, in the long run.
$$\begin{aligned}
s.t.\space &E(\sigma_{t+1}^2) = E(\sigma_t^2) = \sigma^2_{\text{long term stability}}\\
\\
\because\space E(\sigma_{t+1}^2)=E(\sigma_{t}^2)&=\omega + \alpha E(R_t^2) + \beta E(\sigma_t^2)\\
\sigma^2&= \omega + \alpha \sigma^2 + \beta \sigma^2\\
\sigma^2&= \omega + (\alpha + \beta) \sigma^2\\
\therefore\space\omega&=(1-\alpha-\beta)\sigma^2
\end{aligned}
$$
























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



# EMH


```tikz
\begin{document} 
\begin{tikzpicture} 
\draw (2,2) rectangle (6,3); 
\node[font=\footnotesize,fill=white] at (4,2.5) {New Public Information};
\draw (2,3.5) rectangle (6,4.5); 
\node[font=\footnotesize,fill=white] at (4,3.5) {Weak Form EMH};
\node[font=\footnotesize,fill=white] at (4,4) {Past public information};
\draw (7,2) rectangle (11,3); 
\node[font=\footnotesize,fill=white] at (9,2.5) {New Private Information};
\draw (7,3.5) rectangle (11,4.5); 
\node[font=\footnotesize,fill=white] at (9,4) {Past Private Information};
\draw (1.5,1.5) rectangle (6.5,5); 
\node[font=\footnotesize,fill=white] at (4,1.5) {Semi-Strong Form EMH};
\draw (1,1) rectangle (11.5,5.5); 
\node[font=\footnotesize,fill=white] at (6.25,1) {Strong Form EMH};
\end{tikzpicture} 
\end{document}
```

- Importance of time lag：
	- Markets already fully reflect information 
		- => prices reflect no time lag
			- => no future information 
				- => no way to predict future prices

# 