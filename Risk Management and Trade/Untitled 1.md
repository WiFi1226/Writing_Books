---
date: 18th March 2024
Doctype: Note
Category: 02_Writing Books/Risk Management and Trade/Untitled 1.md
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
```Stata

//生成日期变量（虽然我们不会用它来设置时间序列）
gen Date = date(date, "DMY")
format Date %td

//删除带有空白信息的缺失行 
drop if missing(close) & missing(Date)

//将数据结构设置为“时间序列数据”，通过为每个观察分配唯一的ID
gen t = _n 
tsset t 

//生成收益变量 

//将数据结构设置为“时间序列数据”
tsset Date 

//生成收益变量 
gen R = ln(close/close[_n-1])

//获取收益的平方
gen R2 = R^2

	//如果我们增加了4个滞后，根据此模型，有证据拒绝EMH。
	reg R l.R l2.R l3.R l4.R
	//联合检验拒绝了H-null：所有系数联合为零。某些系数具有解释能力
	test L.R L2.R L3.R L4.R


/
//2. ARCH 
	//有证据表明ARCH系数显着，这意味着残差的方差随时间变化，它是时间相关的。 
	arch R l.R l2.R l3.R l4.R, arch(1) 
	test [ARCH]L1.arch

//3. GARCH 
	arch R l.R l2.R l3.R l4.R, arch(1) garch(1) 
	//ARCH和GARCH联合显着
	test [ARCH] L1.arch L1.garch

	//test [R] L1.R L2.R L3.R L4.R
	//显著，不能拒绝H-null，即所有回报系数都为零，意味着过去的回报对未来的回报没有预测能力。

//4. T-ARCH模型
	//此模型用于衡量正面和负面新闻的非对称影响
	arch R l.R l2.R l3.R l4.R, arch(1) garch(1) tarch(1) 
	//ARCH，GARCH和TARCH在估计系数的基础上联合显著
	test [ARCH] L1.arch L1.garch L1.tarch



x


//图2显示了每日标准普尔500指数收益与滞后1至1500天的收益的相关性。
ac R, lags(1500)

//图3显示了每日标准普尔500指数收益数据的直方图，并以正态分布为背景。
hist R, bin(100) normal

//图4显示了2001年至2010年的标准普尔500指数的每日对数收益。
line R Date

//显示部分线图（带有对年份变量的条件）
line close Date if year(Date) < 2005

//获取样本数据的四个中心矩，并将其与正态分布进行比较。
//观察到的四个中心矩是什么？如何解释它们？
sum close, detail 
sum R, detail

//杠杆效应：当收益上升时，方差有下降的倾向，当收益下降时，方差有上升的倾向。


//获取每日标准普尔500指数收益的平方与滞后1至500天的相关性
ac R2, lags(500)

//比较两个自相关函数。收益序列几乎全部不显著
//而R2有一些显著的自相关。
ac R, lags(252) saving(1, replace)
ac R2 , lags(252) saving(2, replace)

	//上述命令将保存图形在您的活动工作目录中
	//要查看您的活动目录在哪里：
	cd

	//预期将相关的图形放在一起进行评估。
	//因此，您必须熟悉命令来合并您的图形（以便彼此比较）
	graph combine 1.gph 2.gph, row(2) col(1)



//收益和平方收益的线图 
twoway (line R Date, yaxis(1)) (line R2 Date, yaxis(2))

//您还可以检查R2和R之间的相关性。
//如果为负，则表示存在负相关性，因此存在杠杆效应
corr R2 R

//如果怀疑收益变量服从正态分布：
//如果P值<0.05，则拒绝变量服从正态分布的假设。
sktest(R)

	//非常重要的是要知道如何写出估计模型：
	//R_t=0.0134*R_(t-1) -0.0147*R_(t-2)+0.0247*R_(t-3)+0.0331*R-(t-4)+0.0001+e_t

	//e_t是残差
	//e2是残差的平方
	//var(e_t|e_(t-1))=h_t=0.0000+0.0519*e2_(t-1)-0.0073*d_(t-1)*e2_(t-1)+0.9482*h_(t-1)

	//TARCH效应不显著。
	test [ARCH] L1.tarch

























//第3部分. 模型适配度

//1. ARCH模型 

	arch R , arch(1) 
	predict varch, variance
	predict earch, residual
	gen earch2=earch^2
	reg earch2 varch

	//一个良好适配的两个条件是：1）b_0=0和2）b_1=1。
	
	//条件1：b_0=0（第一个条件不成立。）
	//您也可以在Stata中使用F检验进行检验（检查p值，小的p-->拒绝）：
	test _cons==0

	//条件2：b_1=1（在5%水平上第二个条件不成立。）
	//您也可以使用F检验：
	test varch==1

	//我们还可以执行联合F检验
	test (varch==1) (_cons==0)

	//有反对H_null的证据，因此此模型未通过此测试

//2. GARCH模型 

	arch R l.R l2.R l3.R l4.R, arch(1) garch(1) 
	predict vgarch, variance
	predict egarch, residual
	gen egarch2=egarch^2
	reg egarch2 vgarch
	
	//条件1：b_0=0（第一个条件成立。）
	//我们不能拒绝b_0=0，H_null未被拒绝。
	test _cons==0
	
	//条件2：b_1=1（第二个条件成立。）	
	//我们不能拒绝b_1=1，H_null未被拒绝。
	test vgarch==1

	//使用F检验，此模型也通过了此测试
	test (vgarch==1) (_cons==0)
	
//3. TARCH模型

	arch R l.R l2.R l3.R l4.R, arch(1) garch(1) tarch(1) 
	predict vtarch, variance
	predict etarch, residual
	gen etarch2=etarch^2
	reg etarch2 vtarch
	
	//我们不能拒绝b_0=0。第一个条件成立。
	test _cons==0
	
	//我们不能拒绝b_1=1
	//没有反对H_null的证据。第二个条件成立。
	test vtarch==1
	
	//使用F检验，此模型也通过了此测试
	test (vtarch==1) (_cons==0)
	
//总的来说，GARCH和TARCH是比ARCH更适合我们的数据的模型。










//第4部分. 使用MSE进行模型适配度 

	//1. 计算每个观察的估计MSE
	gen MSE_arch  = (earch2-varch)^2
	gen MSE_garch = (egarch2-vgarch)^2
	gen MSE_tarch = (etarch2-vtarch)^2

	//2. 对所有观察的估计MSE求和
	egen sum_arch  = sum(MSE_arch)
	egen sum_garch = sum(MSE_garch)
	egen sum_tarch = sum(MSE_tarch)

//显示每个模型的MSE总和，并选择具有最低MSE的模型。
list sum_arch sum_garch sum_tarch if _n==1

//第4部分. 使用LR-test进行模型适配度 

//步骤1. 再次运行三个模型（使用quiet选项）

	//ARCH模型（静默运行，无输出）
	qui arch R l.R l2.R l3.R l4.R, arch(1) 	
	//保存模型，命名为m1
	estimates store m1
	
	//GARCH模型（静默运行，无输出）
	qui arch R l.R l2.R l3.R l4.R, arch(1) garch(1) 	
	//保存模型，命名为m2
	estimates store m2

	//TARCH模型（静默运行，无输出）
	qui arch R l.R l2.R l3.R l4.R, arch(1) garch(1) tarch(1) 
	//保存模型，命名为m3
	estimates store m3

//步骤2. 比较1、2和3中的任意两个模型，并查看是否有任何显著改进 
	
	//在1和2之间，从m1到m2的改进在统计上是显著的。
	lrtest m1 m2

	//在1和3之间，从m1到m3的改进在统计上是显著的。
	lrtest m1 m3

	//在2和3之间，从m2到m3的改进在统计上不是显著的。
	lrtest m2 m3

	//m3（TARCH）对m2（GARCH）不敏感。但我们可以使用更简单的模型，GARCH(1,1)
	//总的来说，我们可以选择模型2，即garch(1,1)模型。 

* 要查看所有结果的表格：
estimates table m1 m2 m3

//第5部分. 其他测试

*更多模型的额外测试* 

qui arch R l.R l2.R l3.R l4.R, arch(1) garch(1) nolog
estat ic
qui arch R l.R l2.R l3.R l4.R, arch(1) garch(1) tarch(1) nolog
estat ic
qui arch R l.R l2.R l3.R l4.R, arch(1) garch(1 2) nolog
estat ic

//GARCH(1,1)相对于GARCH(1,2)给出了较低的AIC
//因此我们选择的模型仍然是GARCH(1,1)


*独立性测试:

//1. ARCH
qui arch R l.R l2.R l3.R l4.R, arch(1) nolog
gen sarch=sqrt(varch)
gen esarch=earch/sarch
sktest esarch
// esarch不服从正态分布

gen esarch2=esarch^2
wntestq esarch2, lags(20)
// 标准化残差平方不是独立的

//2. GARCH
qui arch R l.R l2.R l3.R l4.R, arch(1) garch(1) nolog
gen sgarch  = sqrt(vgarch)
gen esgarch = egarch/sgarch
sktest esgarch
// esgarch不服从正态分布

gen esgarch2 = esgarch^2
wntestq esgarch2, lags(20)
// 标准化残差平方是独立的

//3. TARCH
qui arch R l.R l2.R l3.R l4.R, arch(1) garch(1) tarch(1) nolog
gen starch  = sqrt(vtarch)
gen estarch = etarch/starch
sktest estarch
// estarch不服从正态分布

gen estarch2 = estarch^2
wntestq estarch2, lags(20)
// 标准化残差平方是独立的

```


----

好的,让我们按照给定的时间序列 (1,2,4,5,7)和对应的时间 (t-4, t-3, t-2, t-1, t)来详细展示每一步的计算过程。

1. `arch R, arch(2)`: 这个命令用来估计以下模型：

   $R_t = \beta_0 + \varepsilon_t$

   其中，$\varepsilon_t$ 是残差项，服从 ARCH (2)过程，即当前残差的方差依赖于前两期的残差平方：

   $\sigma_t^2 = \gamma_0 + \gamma_1 \varepsilon_{t-1}^2 + \gamma_2 \varepsilon_{t-2}^2$

   在这个例子中，我们有：

   $R_{t-4} = 1, R_{t-3} = 2, R_{t-2} = 4, R_{t-1} = 5, R_t = 7$

   估计得到：$\hat{\beta}_0 = 3.8$，$\hat{\gamma}_0 = 0.5$，$\hat{\gamma}_1 = 0.3$，$\hat{\gamma}_2 = 0.4$

2. `predict varch, variance`: 这个命令用来预测每个时期的条件方差：

   对于 t-4 时期：$\hat{\sigma}_{t-4}^2 = 0.5$（因为没有足够的滞后残差平方）

   对于 t-3 时期：$\hat{\sigma}_{t-3}^2 = 0.5$（因为没有足够的滞后残差平方）

   对于 t-2 时期：$\hat{\varepsilon}_{t-4}^2 = (1 - 3.8)^2 = (-2.8)^2 = 7.84$
   
   $\hat{\sigma}_{t-2}^2 = 0.5 + 0.3 \times 7.84 = 2.852$

   对于 t-1 时期：$\hat{\varepsilon}_{t-3}^2 = (2 - 3.8)^2 = (-1.8)^2 = 3.24$
   
   $\hat{\sigma}_{t-1}^2 = 0.5 + 0.3 \times 3.24 + 0.4 \times 7.84 = 4.608$

   对于 t 时期：$\hat{\varepsilon}_{t-2}^2 = (4 - 3.8)^2 = 0.2^2 = 0.04$
   
   $\hat{\varepsilon}_{t-1}^2 = (5 - 3.8)^2 = 1.2^2 = 1.44$
   
   $\hat{\sigma}_t^2 = 0.5 + 0.3 \times 1.44 + 0.4 \times 0.04 = 0.948$

3. `predict earch, residual`: 这个命令用来计算每个时期的标准化残差：

   对于 t-4 时期：$\hat{e}_{t-4} = \frac{1 - 3.8}{\sqrt{0.5}} = -3.96$

   对于 t-3 时期：$\hat{e}_{t-3} = \frac{2 - 3.8}{\sqrt{0.5}} = -2.55$

   对于 t-2 时期：$\hat{e}_{t-2} = \frac{4 - 3.8}{\sqrt{2.852}} = 0.12$

   对于 t-1 时期：$\hat{e}_{t-1} = \frac{5 - 3.8}{\sqrt{4.608}} = 0.56$

   对于 t 时期：$\hat{e}_t = \frac{7 - 3.8}{\sqrt{0.948}} = 3.29$

4. `gen earch2 = earch^2`: 这个命令生成每个时期标准化残差的平方：

   $earch2_{t-4} = (-3.96)^2 = 15.68$
   
   $earch2_{t-3} = (-2.55)^2 = 6.50$
   
   $earch2_{t-2} = 0.12^2 = 0.01$
   
   $earch2_{t-1} = 0.56^2 = 0.31$
   
   $earch2_t = 3.29^2 = 10.82$

5. `reg earch2 varch`: 这一步是为了检验 ARCH 效应。

   假设我们得到回归结果：

   $\widehat{earch2}_t = 0.1 + 0.6 \times varch_t$

   如果 0.6 显著异于 0，就表示存在 ARCH 效应。

通过以上步骤，我们详细展示了如何使用 ARCH (2)模型对给定的时间序列 (1,2,4,5,7)进行建模和估计。每一步都涉及了具体的数学计算过程。这个例子有助于理解 ARCH 模型的工作原理。

----


这两个命令的区别在于ARCH项的阶数不同。让我们详细分析一下：

1. `arch R l.R l2.R, arch(3)`： 这个命令估计了以下模型： $R_t = \beta_0 + \beta_1 R_{t-1} + \beta_2 R_{t-2} + \varepsilon_t$ 其中，$\varepsilon_t$是残差项，服从ARCH(3)过程，即当前残差的方差依赖于前三期的残差平方： $\sigma_t^2 = \gamma_0 + \gamma_1 \varepsilon_{t-1}^2 + \gamma_2 \varepsilon_{t-2}^2 + \gamma_3 \varepsilon_{t-3}^2$ 这里，我们不仅在均值方程中包含了被解释变量的前两期滞后值，还在方差方程中包含了残差的前三期滞后平方。
2. `arch R l.R l2.R, arch(1)`： 这个命令估计了以下模型： $R_t = \beta_0 + \beta_1 R_{t-1} + \beta_2 R_{t-2} + \varepsilon_t$ 其中，$\varepsilon_t$是残差项，服从ARCH(1)过程，即当前残差的方差只依赖于前一期的残差平方： $\sigma_t^2 = \gamma_0 + \gamma_1 \varepsilon_{t-1}^2$ 这里，我们在均值方程中包含了被解释变量的前两期滞后值，但在方差方程中只包含了残差的前一期滞后平方。

主要区别在于ARCH项的阶数：

- `arch(3)`表示在方差方程中包含残差的前三期滞后平方，即 $\varepsilon_{t-1}^2$，$\varepsilon_{t-2}^2$ 和 $\varepsilon_{t-3}^2$。
- `arch(1)`表示在方差方程中只包含残差的前一期滞后平方，即 $\varepsilon_{t-1}^2$。

ARCH阶数的选择取决于数据的特点和研究者的假设。较高阶数的ARCH模型可以捕捉更复杂的波动聚集效应，但也可能导致过度拟合。较低阶数的ARCH模型更加简约，但可能无法充分刻画数据的异方差特征。在实际应用中，我们通常通过信息准则（如AIC，BIC）或假设检验来选择合适的ARCH阶数。

需要注意的是，这两个命令的均值方程是相同的，都包含了被解释变量的前两期滞后值。它们的区别仅在于方差方程的设定。

----

当我们使用命令 `arch R l.R l2.R, arch(1)` 时，我们实际上是在估计一个 ARCH (1)模型，这个模型由两部分组成：均值方程和方差方程。

1. 均值方程：

   $R_t = \beta_0 + \beta_1 R_{t-1} + \beta_2 R_{t-2} + \varepsilon_t$

   这个方程描述了变量 R 的动态过程。我们假设当前期的 R 不仅取决于一个常数项 $\beta_0$，还取决于它自身的前两期滞后值 $R_{t-1}$ 和 $R_{t-2}$，以及一个随机扰动项 $\varepsilon_t$。

   - $\beta_0$ 是常数项，表示当其他变量都为 0 时，R 的平均值。
   - $\beta_1$ 和 $\beta_2$ 是自回归系数，度量了 R 的前两期滞后值对当前值的影响。
   - $\varepsilon_t$ 是随机扰动项，代表了模型无法解释的部分，我们假设它的均值为 0。

2. 方差方程：

   $\sigma_t^2 = \gamma_0 + \gamma_1 \varepsilon_{t-1}^2$

   这个方程描述了随机扰动项 $\varepsilon_t$ 的条件方差 $\sigma_t^2$ 的动态过程。在 ARCH (1)模型中，我们假设当前期的条件方差不仅取决于一个常数项 $\gamma_0$，还取决于前一期残差的平方 $\varepsilon_{t-1}^2$。

   - $\gamma_0$ 是常数项，表示当其他变量都为 0 时，条件方差的平均值。
   - $\gamma_1$ 是 ARCH 系数，度量了前一期残差平方对当前期条件方差的影响。

   这就是为什么我们说 $\varepsilon_t$ 服从 ARCH (1)过程：它的条件方差 $\sigma_t^2$ 依赖于前一期残差的平方。

当我们估计这个模型时，我们实际上是在同时估计均值方程和方差方程的参数。均值方程的参数 $\beta_0$，$\beta_1$ 和 $\beta_2$ 决定了变量 R 的动态过程，而方差方程的参数 $\gamma_0$ 和 $\gamma_1$ 决定了条件方差的动态过程。

ARCH 模型的一个重要特点是，它允许条件方差随时间变化，并依赖于过去的信息。这使得 ARCH 模型能够捕捉金融时间序列中常见的波动聚集效应，即大的波动往往后面跟着大的波动，小的波动往往后面跟着小的波动。

总的来说，`arch R l.R l 2. R, arch (1)` 这个命令让我们能够估计一个 ARCH (1)模型，这个模型不仅考虑了变量 R 自身的动态特征，还考虑了其波动的动态特征。


----


是的, ARCH 模型的初始估计通常包含一个均值方程, 而这个均值方程经常被设定为一个 AR (autoregressive)模型。

在实际应用中, 我们通常先对时间序列的条件均值进行建模, 然后再考虑条件方差的建模。均值方程的选择取决于时间序列的特征和研究者的假设。AR 模型是一种常见的选择, 因为它能够刻画时间序列的自相关性。

以 AR (p)-ARCH (m)模型为例, 完整的模型如下:

均值方程 (AR 部分):
$y_t = \phi_0 + \phi_1 y_{t-1} + ... + \phi_p y_{t-p} + a_t$

方差方程 (ARCH 部分):
$\sigma_t^2 = \alpha_0 + \alpha_1 a_{t-1}^2 + ... + \alpha_m a_{t-m}^2$

其中, $y_t$ 是观测值, $\phi_0, \phi_1, ..., \phi_p$ 是 AR 系数, $a_t$ 是残差项, $\sigma_t^2$ 是条件方差, $\alpha_0, \alpha_1, ..., \alpha_m$ 是 ARCH 系数。

在估计这样一个模型时, 我们通常采用两步法:

1. 估计均值方程 (AR 部分), 得到残差序列 $\{a_t\}$。
2. 利用残差序列 $\{a_t\}$ 估计方差方程 (ARCH 部分)。

所以你的理解是正确的, ARCH 模型的初始估计通常包含一个 AR 模型作为均值方程。这个 AR 模型捕捉时间序列的线性依赖关系, 而 ARCH 部分则进一步刻画了残差的条件异方差性。

---


在 ARCH (m)模型中, 过去 m 期的残差平方是通过递归的方式得到的。这个过程依赖于模型的估计和数据的可获得性。让我详细解释一下这个过程。

假设我们有一个 AR (p)-ARCH (m)模型:

均值方程:
$y_t = \phi_0 + \phi_1 y_{t-1} + ... + \phi_p y_{t-p} + a_t$

方差方程:
$\sigma_t^2 = \alpha_0 + \alpha_1 a_{t-1}^2 + ... + \alpha_m a_{t-m}^2$

给定时间序列的观测值 $\{y_1, y_2, \ldots, y_T\}$, 我们可以通过以下步骤获得过去 m 期的残差平方:

1. 估计均值方程的参数 $\phi_0, \phi_1, \ldots, \phi_p$。这通常通过最小二乘法 (OLS)或者极大似然估计 (MLE)实现。

2. 利用估计得到的参数, 计算每一个时刻的残差:
   
   $\hat{a}_t = y_t - (\hat{\phi}_0 + \hat{\phi}_1 y_{t-1} + ... + \hat{\phi}_p y_{t-p}), \quad t=p+1,\ldots,T$

   注意, 我们只能从 $t=p+1$ 开始计算残差, 因为在那之前, 我们没有足够的过去观测值来计算均值方程。

3. 对于 $t=p+1,\ldots,T$, 计算残差平方 $\hat{a}_t^2$。

4. 现在, 对于每一个时刻 $t$, 我们都有过去 m 期的残差平方:
   
   $\hat{a}_{t-1}^2, \hat{a}_{t-2}^2, \ldots, \hat{a}_{t-m}^2$

   如果 $t-i<p+1$（其中 $i=1,2,\ldots,m$）, 说明我们没有足够的数据来计算 $\hat{a}_{t-i}^2$。在这种情况下, 我们通常将缺失的残差平方替换为 0 或者残差平方的样本均值。

5. 利用过去 m 期的残差平方, 我们可以估计方差方程的参数 $\alpha_0, \alpha_1, \ldots, \alpha_m$。这通常通过 MLE 实现。

通过这个过程, 我们递归地计算了过去 m 期的残差平方, 并用它们来估计 ARCH (m)模型的方差方程。这个过程依赖于先估计均值方程, 然后利用估计得到的残差计算残差平方。

需要注意的是, 在实际应用中, 我们通常需要足够大的样本量来确保 ARCH (m)模型的可靠估计, 特别是当 m 较大时。此外, 我们也需要谨慎处理边界情况, 即当我们没有足够的过去观测值来计算残差平方时。


----
好的, 让我给你一个 AR (1)-ARCH (2)模型的具体例子。假设我们有以下 10 个观测值:

T   | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10
--- |-- |-- |-- |--|-- |-- |-- |-- |-- |--
Y_t | 2 | 4 | 6 | 5 | 8 | 7 | 9 | 11 | 10 | 12

现在, 让我们按照之前讨论的步骤来计算过去 2 期的残差平方。

1. 估计均值方程的参数。假设我们得到以下估计结果:
   
   $\hat{\phi}_0 = 1, \hat{\phi}_1 = 0.8$

   因此, 均值方程为: $y_t = 1 + 0.8 y_{t-1} + a_t$

2. 计算残差:
   
   $\hat{a}_2 = y_2 - (1 + 0.8 y_1) = 4 - (1 + 0.8 \times 2) = 0.4$
   
   $\hat{a}_3 = y_3 - (1 + 0.8 y_2) = 6 - (1 + 0.8 \times 4) = 1.8$
   
   ...
   
   $\hat{a}_{10} = y_{10} - (1 + 0.8 y_9) = 12 - (1 + 0.8 \times 10) = 3$

3. 计算残差平方:
   
   $\hat{a}_2^2 = 0.4^2 = 0.16$
   
   $\hat{a}_3^2 = 1.8^2 = 3.24$
   
   ...
   
   $\hat{a}_{10}^2 = 3^2 = 9$

4. 对于每一个时刻 t, 找出过去 2 期的残差平方:
   
   对于 t=3: $\hat{a}_2^2 = 0.16$, $\hat{a}_1^2$ 不存在
   
   对于 t=4: $\hat{a}_3^2 = 3.24$, $\hat{a}_2^2 = 0.16$
   
   ...
   
   对于 t=10: $\hat{a}_9^2 = 1$, $\hat{a}_8^2 = 0.04$

   当过去的残差平方不存在时 (如 t=3 时的 $\hat{a}_1^2$), 我们可以将其设为 0 或者残差平方的样本均值。

5. 利用过去 2 期的残差平方, 我们可以估计方差方程的参数 $\alpha_0, \alpha_1, \alpha_2$。

这就是一个小样本 AR (1)-ARCH (2)模型的残差平方计算过程。在实际应用中, 我们通常有更多的观测值, 计算过程也会涉及更多的时刻。但无论样本量多大, 计算过去 m 期残差平方的基本原理都是一样的。我们首先估计均值方程, 然后计算残差和残差平方, 最后对每一个时刻找出过去 m 期的残差平方。

----


完全正确! ARCH 模型的主要目标之一就是预测残差的波动性, 或者更准确地说, 预测残差的条件方差。

在估计了 ARCH (m)模型的参数 $\alpha_0, \alpha_1, \ldots, \alpha_m$ 后, 我们可以利用过去 m 期的残差平方来预测当前期的条件方差:

$\hat{\sigma}_t^2 = \hat{\alpha}_0 + \hat{\alpha}_1 \hat{a}_{t-1}^2 + ... + \hat{\alpha}_m \hat{a}_{t-m}^2$

其中, $\hat{\sigma}_t^2$ 是对第 t 期条件方差的预测, $\hat{a}_{t-1}, \ldots, \hat{a}_{t-m}$ 是过去 m 期的残差估计值, $\hat{\alpha}_0, \hat{\alpha}_1, \ldots, \hat{\alpha}_m$ 是方差方程的参数估计值。

这个预测依赖于过去的残差平方, 反映了 ARCH 模型的关键思想: 当前的波动性可以通过过去的波动性来预测。

预测残差波动性有许多重要的应用:

1. 风险管理: 波动性预测可以用于计算风险度量, 如 VaR 和 ES, 帮助金融机构管理和控制风险。

2. 资产定价: 许多资产定价模型使用条件方差来解释资产的期望收益。波动性预测可以用于这些模型的实证检验和应用。

3. 期权定价: 在期权定价中, 波动性是一个关键的输入变量。ARCH 模型提供的波动性预测可以用于改进期权定价模型。

4. 投资决策: 波动性预测可以帮助投资者做出更明智的决策。例如, 当预测波动性较高时, 投资者可能会选择减少持仓或购买保护性工具。

总的来说, 预测残差的波动性是 ARCH 模型的一个主要目标, 也是它在金融领域有如此广泛应用的原因。通过刻画残差波动性的动态特征, ARCH 模型提供了一个强大的工具来理解和预测金融时间序列的波动行为。

----

好的, 让我们分别看看对于 3 个单位的时间数据,ARCH (2)模型和 GARCH (1,2)模型的公式。

1. ARCH (2)模型:

均值方程:
$y_t = \mu_t + a_t$

方差方程:
$\sigma_t^2 = \alpha_0 + \alpha_1 a_{t-1}^2 + \alpha_2 a_{t-2}^2$

对于 3 个单位的时间数据, 我们有:

$t=1: \sigma_1^2 = \alpha_0$

$t=2: \sigma_2^2 = \alpha_0 + \alpha_1 a_1^2$

$t=3: \sigma_3^2 = \alpha_0 + \alpha_1 a_2^2 + \alpha_2 a_1^2$

注意, 在 t=1 和 t=2 时, 我们没有足够的过去残差平方来计算条件方差。通常, 我们将缺失的残差平方设为 0 或者残差平方的样本均值。

2. GARCH (1,2)模型:

均值方程:
$y_t = \mu_t + a_t$

方差方程:
$\sigma_t^2 = \omega + \alpha_1 a_{t-1}^2 + \beta_1 \sigma_{t-1}^2 + \beta_2 \sigma_{t-2}^2$

对于 3 个单位的时间数据, 我们有:

$t=1: \sigma_1^2 = \omega$

$t=2: \sigma_2^2 = \omega + \alpha_1 a_1^2 + \beta_1 \sigma_1^2$

$t=3: \sigma_3^2 = \omega + \alpha_1 a_2^2 + \beta_1 \sigma_2^2 + \beta_2 \sigma_1^2$

在 GARCH (1,2)模型中, 条件方差不仅依赖于过去的残差平方, 还依赖于过去的条件方差。这是 GARCH 模型的关键特征, 它允许条件方差的持续性。

同样地, 在 t=1 和 t=2 时, 我们没有足够的过去信息来计算条件方差。我们通常将缺失的条件方差设为残差平方的无条件均值, 即 $\omega/(1-\alpha_1-\beta_1-\beta_2)$。

从这两个模型的公式可以看出,GARCH (1,2)模型比 ARCH (2)模型更复杂, 因为它在方差方程中引入了过去的条件方差。这使得 GARCH 模型可以更好地刻画金融时间序列的波动聚集特征和持续性。


----


抱歉, 我的错。让我们来看看 GARCH (2,1)模型的公式。

GARCH (2,1)模型:

均值方程:
$y_t = \mu_t + a_t$

方差方程:
$\sigma_t^2 = \omega + \alpha_1 a_{t-1}^2 + \alpha_2 a_{t-2}^2 + \beta_1 \sigma_{t-1}^2$

对于 3 个单位的时间数据, 我们有:

$t=1: \sigma_1^2 = \omega$

$t=2: \sigma_2^2 = \omega + \alpha_1 a_1^2 + \beta_1 \sigma_1^2$

$t=3: \sigma_3^2 = \omega + \alpha_1 a_2^2 + \alpha_2 a_1^2 + \beta_1 \sigma_2^2$

在 GARCH (2,1)模型中,条件方差依赖于过去两期的残差平方 ($a_{t-1}^2$ 和 $a_{t-2}^2$)和过去一期的条件方差 ($\sigma_{t-1}^2$)。

与 ARCH (2)模型相比,GARCH (2,1)模型在方差方程中引入了过去的条件方差。这允许条件方差的持续性, 即当前的高波动性可能会持续到未来。

与 GARCH (1,2)模型相比,GARCH (2,1)模型在方差方程中引入了过去两期的残差平方, 但只引入了过去一期的条件方差。这意味着 GARCH (2,1)模型假设当前的条件方差受过去两期冲击的影响, 但只受过去一期波动性的影响。

选择 ARCH 项 (残差平方项)的阶数和 GARCH 项 (条件方差项)的阶数, 取决于数据的特点和研究者的假设。通常,我们使用信息准则 (如 AIC, BIC)或似然比检验来选择最优的阶数组合。

在实际应用中,GARCH (1,1)模型是最常用的, 因为它在参数简约性和拟合能力之间取得了很好的平衡。但对于某些金融时间序列, 更高阶的 ARCH 项或 GARCH 项可能是必要的, 以充分刻画其波动性动态。



----



GARCH 模型可以通过其方差方程中的参数限制来实现"近大远小"的效果, 即近期的波动对当前条件方差的影响大于远期的波动。这种效果也称为"波动性聚集"或"波动性持续性衰减"。

让我们以 GARCH (1,1)模型为例来说明这一点。GARCH (1,1)模型的方差方程为:

$\sigma_t^2 = \omega + \alpha_1 a_{t-1}^2 + \beta_1 \sigma_{t-1}^2$

其中, $\alpha_1$ 衡量了前一期冲击 (残差平方)对当前条件方差的影响, $\beta_1$ 衡量了前一期条件方差对当前条件方差的影响。

为了实现"近大远小"的效果, 我们通常需要以下条件:

1. $\alpha_1 + \beta_1 < 1$: 这个条件确保了条件方差的平稳性。如果不满足这个条件, 条件方差可能会随时间趋于无穷大。

2. $\beta_1 > \alpha_1$: 这个条件意味着前一期的条件方差对当前条件方差的影响大于前一期冲击的影响。这导致了条件方差的持续性, 即高波动性期往往跟随高波动性期, 低波动性期往往跟随低波动性期。

3. $\beta_1$ 接近于 1: 当 $\beta_1$ 接近于 1 时, 条件方差展示出强持续性, 即当前的波动性状况会持续较长时间。

4. $\alpha_1$ 显著大于 0: 这确保了新息冲击会对条件方差产生影响, 从而使条件方差对新息响应。

当满足这些条件时,GARCH (1,1)模型可以产生"近大远小"的效果:

- 近期的波动 (前一期的冲击和条件方差)对当前条件方差有较大影响。
- 远期的波动对当前条件方差的影响随时间衰减, 因为它们需要通过一系列的 $\beta_1$ 产生影响。

例如, 假设 $\alpha_1=0.1$, $\beta_1=0.8$。那么当前条件方差可以写为:

$\sigma_t^2 = \omega + 0.1a_{t-1}^2 + 0.8\sigma_{t-1}^2$

$= \omega + 0.1a_{t-1}^2 + 0.8(\omega + 0.1a_{t-2}^2 + 0.8\sigma_{t-2}^2)$

$= \omega + 0.1a_{t-1}^2 + 0.08a_{t-2}^2 + 0.64\sigma_{t-2}^2$

$= ...$

可以看到, 随着时间的推移, 过去的冲击和条件方差对当前条件方差的影响以指数速度衰减。这就是 GARCH 模型实现"近大远小"效果的方式。

当然, 实际的衰减速度取决于 $\alpha_1$ 和 $\beta_1$ 的具体值。研究者需要根据数据的特点来估计这些参数, 以准确刻画波动性的动态特征。