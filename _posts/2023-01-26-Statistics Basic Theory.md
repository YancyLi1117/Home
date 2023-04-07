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

## Basic Theory
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

## Probability

### Bayes’ Theorem

$$
p(A|B)=\frac{P(B|A)P(A)}{P(B)}
$$

### Law of Total Probability

Let S be a sample space and A1, . . . , An a partition of S. Then

$$
P(B)=P(B \cap A_1)+...+P(B\cap A_n)=P(B|A_1)P(A_1)+...+P(B|A_n)P(A_n)
$$

Two events A and B are conditionally independent of an event
$$
P((A \cap B) | C) = P(A|C)P(B)
$$

### Joint Probability

Let X,Y be random variables. Their joint CDF is given by $F(x,y)=P(X≤x,Y ≤y)$

In the discrete case, X and Y have a joint PMF given by $P(X=x,Y =y)$

and in the continuous case, X and Y have a joint PDF given by $f(x,y)=\frac{\partial}{\partial x \partial y}F(x,y)$

and we can compute $P((X,Y)\in B)=\int\int_B f(x,y)\mathrm{d}x\mathrm{d}y $

Their separate CDFs and PMFs (e.g., P(X ≤ x)) are referred to as marginal CDFs, PMFs, or PDFs. X and Y are independent precisely when the the joint CDF is equal to the product of the marginal CDFs: $F(x,y) = F_X(x)F_Y (y)$

### Covariance

判断正相关还是负相关

The covariance of random variables X and Y is $Cov(X,Y)=E((X−EX)(Y −EY))$, [-∞,∞]

We can use covariance to compute the variance of sums:

$$
Var(X + Y ) = Cov(X, X) + Cov(X, Y ) +Cov(Y,X)+Cov(Y,Y) \\= Var(X) + 2 Cov(X, Y ) + Var(Y )
$$

**Theorem**: If X,Y are independent, then Cov(X, Y ) = 0 

But! **The converse of the above is false.** Let Z ∼ N(0,1), X = Z, Y = Z2, and let us compute the covariance=0. But X and Y are very dependent since Y is a function of X.

### Correlation

协方差除了XY各自的标准差，刻画XY之间联动性的强弱

The correlation of two random variables X and Y is $Cor(X,Y) r=\frac{Cov(X,Y)}{S_XS_Y}$, [-1,1]

**协方差和相关系数都是描述线性关系**
