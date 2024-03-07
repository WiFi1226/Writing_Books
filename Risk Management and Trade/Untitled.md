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
$对于 ARCH 模型和 TARCH 模型,它们与加权移动平均 (WMA)模型存在一定联系, 具体如下:

1. ARCH (q)模型:
σ_t^2 = ω + α_1ε_(t-1)^2 + ... + α_qε_(t-q)^2 

可看作是对过去 q 期残差平方的加权和, 权重分别为α_1,...,α_q, 常数项ω确保方差为正。

2. TARCH (绝对值 ARCH)模型:
σ_t^2 = ω + α_1ε_(t-1)^2 + γ_1ε_(t-1)^2 I_(t-1) + ... + α_qε_(t-q)^2 + γ_qε_(t-q)^2 I_(t-q)  
I_t = 1 если ε_t < 0, 否则为 0

TARCH 在 ARCH 基础上, 对正负残差平方赋予不同权重, 从而捕捉杠杆效应。

3. 加权移动平均模型:
\\frac{1}{T\_{total}}\\sum\_{\\tau=0}^{T\_{total}-1}R\_{t-\\tau}^{2} = \\lambda \\sum\_{\\tau=1}^{\\infty}(1-\\lambda)^{\\tau-1}R\_{t-\\tau}^{2}

其中λ为平滑参数, T_total 为权重加和, 可理解为对过去所有残差平方按指数权重进行加权求和。当 T_total->∞时, 等价于 IGARCH (1,1)模型, 此时ω=0,α+β=1。

我们可以通过设定不同参数, 使 ARCH 类模型与加权移动平均模型联系起来:

(1) 当 q=1 时,ARCH (1)模型就等价于加权移动平均模型, 令α_1 = λ,ω = (1-λ)σ^2, 则有:
σ_t^2 = (1-λ)σ^2 + λR_(t-1)^2

(2) 当 q->∞,α_i = (1-λ)λ^(i-1),ω = 0 时,ARCH (∞)等价于 IGARCH (1,1)模型, 也等价于加权移动平均模型。

(3) 对于 TARCH (1,1)模型, 当γ_1=0 时, 它就等价于 GARCH (1,1)模型; 如果进一步令ω=0,α_1=1-λ,β_1=λ, 那么它就等价于 RiskMetrics 模型, 也等价于加权移动平均模型。

综上所述, ARCH 类模型通过设定合理的滞后阶数和参数, 可以很好地逼近和等价于加权移动平均模型, 从而捕捉金融时间序列数据中的 volatility clustering 效应$