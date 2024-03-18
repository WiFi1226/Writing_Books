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