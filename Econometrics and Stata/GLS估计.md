---
Date: 19th February 2024
Doctype: Note
Category: 02_Writing Books/Econometrics and Stata/GLS估计.md
Status: Draft
Abstract: 
Keywords: 
tags: 
Version: 1.0.0
License: MIT
Copyright: © 2024 FAN WANG. All rights reserved.
---
---
附加权重的OLS估计
$$
\begin{aligned}
Y_i &= \sum_{k=1}^{n} \beta_kX_{ki} +u_i\space\space and\space\space X_{1i}=1 \\
&\downarrow var(\frac{u_i}{\sigma_i}) = E(\frac{u_i}{\sigma_i})^2 = \frac{1}{\sigma_i^2}E(u_i^2)=\frac{\sigma_i^2}{\sigma_i^2}=1=c \\
\frac{Y_i}{\sigma_i} &= \frac{\sum_{k=1}^{n} \beta_kX_{ki}}{\sigma_i} + \frac{u_i}{\sigma_i}\space\space and\space\space X_{1i}=1\\
\therefore \space GLS&：min\space \sum u_i^2\frac{1}{\sigma_i^2} = \sum(Y_i-\sum_{k=1}^{n} \beta_kX_{ki})^2\frac{1}{\sigma_i^2}
\end{aligned}
$$残差的预估和回归估参是不同的！

克鲁斯卡尔定理