---
layout: post
title: Statistics Basic Theory
date: 2023-01-26
Author: Yancy
tags: [Stat]
comments: true
toc: true
---

统计学分为描述性统计和推断统计两大部分。描述性统计可以继续细分为图表法和数值法。而推断统计则包含概率论、抽样理论、估计理论、假设检验这四大组成部分。

[some basic theory](https://zhuanlan.zhihu.com/p/77312635)

### Central limit theorem (CLT)

The Central Limit Theorem (CLT) states that when plotting a sample distribution of mean the **mean of the sample will be equal to the population mean** and **the sample distribution will approach normal distribution with variance equal to standard error.** 

样本均值等于总体均值，样本分布接近方差等于样本方差的正态分布

Assumptions behind the CLT:

- The sample data must be sampled and **selected randomly** from the population. 
- There should **not be any multicollinearity** in the sampled data; one sample should not influence the other samples.
- The **sample size** should be no more than 10% of the population. Generally, a sample size greater than 30 (n>30) is considered good.

statistical inference: 

**Confidence Interval**: the sample providing information about the precision and reliability of the estimate concerning the larger population.----**Uncertainty of the sample**

### Law of Large number

As the number of (**identically distributed**), **randomly generated** variables increases, their **sample mean (average) approaches their theoretical mean**. 

随着同分布随机生成变量增加，样本均值接近理论均值

the law of large numbers relates to the peak (the **mean**) of a curve, while the central limit theorem relates to the **distribution** of a curve.

### Simpson's Paradox

https://www.sisense.com/blog/understanding-simpsons-paradox-to-avoid-faulty-conclusions/

If **unequal distribution** of data into groups and **undetected confounding variables** are combined in a study, Simpson’s paradox will occur.

suitable experimental design and  dispersed between the sample group:

**Simple randomization**: strewing data into sample groups

**Randomized block design**: the study data are grouped into subgroups according to their similar characteristics. Reduce the consequences of confounding variables.

**Minimization**: randomly distributes subjects to equivalent groups and the likely confounding variables are equally distributed