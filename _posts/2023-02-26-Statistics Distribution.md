---
layout: post
title: Statistics Distribution
date: 2023-02-26
author: Yancy
tags: [Stat]
comments: true
toc: true
---


## Basic Concept

expectation and mean: 均值是针对实验观察到的特征样本而言的，期望是针对于随机变量而言的一个量，针对于他的样本空间而言的。Expectation and mean: **期望是均值随样本趋于无穷的极限**，可以看出均值和期望的联系也是大数定理联系起来的。(As the number of **randomly generated** variables increases, their **sample mean (average) approaches their expectation**. )

### Expectation

Proposition1: if c is constant, then $E(c)=c$ 

Proposition2: Expected value is linear; that is, for random variables X and Y and some constant c,

$$
E(X + Y ) = E(X) + E(Y )\\E(cX) = cE(X)
$$

Proposition3: for two function g(X) f(x), $E(g(X)\pm f(X))=E(g(X))\pm E(f(X))$

### Variance

$$
Var(X)= E(X - E(X))^2
$$

Proposition1: $Var(X)= E(X^2)-  [E(X)]^2$

$$
Var(X)= E(X - E(X))^2=E(X^2-2XE(X)+[E(X)]^2)=E(X^2-[E(X)]^2)=E(X^2)-  [E(X)]^2
$$

Proposition2: if c is constant, then $Var(c)=0$ 

Proposition3: if a,b are constants, then $Var(aX+b)=a^2 Var(X)$ 

### PMF(概率质量函数)

The **probability mass function** (PMF) of a **discrete** random variable (a random variable with enumerable values) is a function that gives the probability that the random variable takes some value. 

That is, given a discrete random variable X, its PMF is $f_X(x) = P(X = x)$

### PDF(概率密度函数)

Let X be a random variable. Then X has a **probability density function (PDF)** $f_X(x)$ if

$$
P(a\leq X \leq b) = \int^b_a f_X(x)\mathrm{d}x
$$

a valid PDF must satisfy:

$$
\forall x, f_X(x)\geq 0\\ \int_{-\infty} ^\infty f_X(x)\mathrm{d}x =1
$$

### CDF(累积分布函数)

The **cumulative distribution function (CDF)** of a random variable X is

$$
F_X (x) = P (X \leq x)
$$

Properties of CDFs: F_X is a valid CDF iff the following hold about F_X:

1. monotonically nondecreasing
2. right-continuous
3. limx→−∞ FX (x) = 0 and limx→∞ FX (x) = 1.

### Law of the unconscious statistician (LOTUS)

Let X be a continuous random variable, g : R → R continuous. Then$E(g(X))=\int_{-\infty} ^\infty g(x)f_X(x)\mathrm{d}x$

where $f_X$ is the PDF of X. And for discrete X with PMF $f_X$, $E(g(X))=\sum_xg(X)f_X(x)$

This allows us to determine the expected value of g(X) without knowing its distribution.用分布算出的期望等于直接在概率空间上积分得到的期望

[更好的解释加例题](https://bookdown.org/kevin_davisross/probsim-book/law-of-the-unconscious-statistician-lotus.html )

**Attention: E(g(X))≠g(E(X))**

### Moment-generating function (MGF) 矩母函数

A random variable X has moment-generating function (MGF)

$$
M(t)=E(e^{tX})=E(\sum^\infty_{n=0}\frac{X^nt^n}{n!})=\sum^\infty_{n=0}\frac{E(X^n)t^n}{n!}
$$

if M(t) is bounded on some interval (−ε,ε) about zero.

This observation also shows us that $E(X^n)=M^{(n)}(0)$

**Claim**: If X and Y have the same MGF, then they have the same CDF.

**Observation**: If X has MGF $M_X$ and Y has MGF $M_Y$ , then $M_{X+Y}=M_XM_Y$

[关于矩的解释](https://zhuanlan.zhihu.com/p/148408669)， 推导了一系列分布的MGF公式...


## Discrete Distribution

### Bernoulli

A random variable X is said to have the Bernoulli distribution if X has only two possible values, 0 and 1, and there is some p such that

$$
P(X = 1) = p\\ P(X = 0) = 1 − p
$$

We say that **X ∼ Bern(p)**

Let X ∼ Bern(p). Then $E(X) = p$, $var(X)=p(1-p)$

### Binomial

The distribution of successes in n independent Bern(p) trials is called the binomial distribution and is given by

$$
P(X = k) = \binom{n}{k}p^k(1 − p)^{n−k}
$$

where 0 ≤ k ≤ n. We write **X ∼ Bin(n, p)**

In addition to our definition of the binomial distribution by its **PMF**, we can also express a random variable X ∼ Bin(n, p) as a sum of indicator random variables,

$$
X = X_1 + · · · + X_n
$$

the $X_i$ are i.i.d. (independent, identically distributed) Bern(p).

If X, Y are independent random variables and X ∼ Bin(n, p), Y ∼ Bin(m, p), then

$$
X + Y ∼ Bin(n + m, p)
$$

Let X ∼ Bin(n, p). Then $E(X) =np$, $Var(X)=np(1-p)$

### Poisson

The Poisson distribution, Pois(λ), is given by the PMF

$$
P(X=k)=\frac{e^{-\lambda}\lambda^k}{k!}\quad for\ k\in \N,X \sim Pois(\lambda)
$$

We call λ the rate parameter.

$E(X)= \lambda$, $Var(X)= \lambda$

Let X ∼ Bin(n, p). Then as n → ∞, p → 0, and where λ = np is held constant, we have X ∼ Pois(λ) **泊松分布是二项分布的特例**

### Geometric

The geometric distribution, Geom(p), is the time until **the first success** of independent Bern(p) trials, counting the success. X 为事件 A 首次出现时的试验次数. Its PMF is given by (for X ∼ Geom(p))

$$
P(X = k) = （1-p)^{k-1}p \quad for\ k=1,2,...
$$

$E(X)=\frac{1}{p}$, $Var(X)=\frac{1-p}{p^2}$

### Negative Binomial

The negative binomial distribution, NB(r,p), is given by the number of failures of indepen- dent Bern(p) trials before the rth success. The PMF for X∼NB(r,p) is given by

$$
P(X=n)=\binom{k-1}{r-1} p^r(1-p)^{k-r}\quad for\ k=r,r+1,...
$$

$E(X)=\frac{r}{p}$, $Var(X)=\frac{r(1-p)}{p^2}$

### Hypergeometric

Suppose we have w white and b black marbles, out of which we choose a simple random sam- ple of n. The distribution of # of white marbles in the sample, which we will call X, is given by

$$
P(X=k)=\frac{\binom{w}{k}\binom{b}{n-k}}{\binom{w+b}{n}}\
where\ 0≤k≤w\ and\ 0≤n−k≤b
$$

This is called the hypergeometric distribution, denoted HGeom(w, b, n).

$E(X)=\frac{nw}{w+b}$

## Continuous Distribution

### Uniform

The uniform distribution, Unif (a, b), is given by a completely random point chosen in the interval [a,b]. Note that the probability of picking a given point x0 is exactly 0; the uniform distribution is continuous. The PDF for U ∼ Unif(a, b) is given by

$$
f_U(x)=
\begin{cases}
\frac{1}{b-a}& {a \leq x \leq b}\\
0& {otherwise}
\end{cases}
$$

Then the CDF:

$$
F_U(x)=\begin{cases}
0&x<a\\
\frac{x-a}{b-a}& {a \leq x \leq b}\\
1& {x>b}
\end{cases}
$$

$E(U) =\frac{a+b}{2}$, $Var(U)=\frac{(b-a)^2}{12}$

The standard uniform distribution is symmetric; that is, if U ∼ Unif (0, 1), then also 1 − U ∼ Unif (0, 1).

### Normal

The normal distribution, given by N(0,1), is defined by PDF

$$
f_Z(x)=ce^{-z^2/2}, c=\frac{1}{\sqrt{2\pi}}
$$

We use $\Phi$ to denote the standard normal CDF; so

$$
\Phi(z)=\frac{1}{\sqrt{2\pi}}\int_{-\infty} ^z e^{-t^2/2}\mathrm{d}t
$$

By symmetry, we have $\Phi(-z)=1-\Phi(z)$

$E(Z)=0$, $Var(Z)=1$

Let $X = μ+σZ$, with μ ∈ R (the mean or center), σ > 0 (the SD or scale). Then we say X ∼ N (μ, σ2). This is the general normal distribution.

If $X∼N(μ,σ^2)$,we have $E(X)=μ$ and $Var(μ+ σZ) = σ^2$ $Var(Z) = σ^2$. We call $Z = X−μ$ the standardization of X. X has CDF:

$$
P(X\leq x)=P(\frac{X-\mu}{\sigma}\leq \frac{x-\mu}{\sigma})=\Phi(\frac{x-\mu}{\sigma})
$$

which yields a PDF of

$$
f_N(x)=\frac{1}{\sigma\sqrt{2\pi}}e^{-(\frac{x-\mu}{\sigma})^2/2}
$$

We also have $−X = −μ + σ(−Z) ∼ N (−μ, σ^2)$.

**Observation1**: if $X_i ∼ N(μ_i,σ_i^2)$ are independent, then 

$$
X_i + X_j ∼ N ( μ_i + μ_j , σ_i^2 + σ_j^2 )\\X_i - X_j ∼ N ( μ_i - μ_j , σ_i^2 + σ_j^2 )
$$

**Observation2**: If$ X\sim N(\mu,\sigma^2)$, we have

$$
P(|X-\mu|\leq\sigma)\approx68\%\\
P(|X-\mu|\leq2\sigma)\approx95\%\\
P(|X-\mu|\leq3\sigma)\approx99.7\%
$$

### Exponential

The exponential distribution, Expo(λ), is defined by PDF, We call λ the rate parameter.

$$
f_E(x)=\lambda e^{-\lambda x},\ for \ x>0 \ and \ 0\ elsewise
$$

Integrating clearly yields 1, which demonstrates validity. Our CDF is given by

$$
F_E(x)=\int_{0} ^x \lambda e^{-\lambda t}\mathrm{d}t=\begin{cases}
1-e^{-\lambda t}& {x>0}\\
0& {otherwise}
\end{cases}
$$

$E(X) = \frac{1}{\lambda}$ and $Var(X) = \frac{1}{\lambda^2}$ .

### Gamma

The gamma function is given by 

$$
\Gamma(\alpha)=\int_{0} ^\infty x^{\alpha-1}e^{-x}\mathrm{d}x
$$

for any a > 0. The gamma function is a continuous extension of the factorial operator on natural numbers. For n a positive integer, $\Gamma(n+1)=n\Gamma(n)=n!$

The standard gamma distribution, Gamma(a, 1), is defined by PDF

$$
\frac{1}{\Gamma(\alpha)}x^{\alpha-1}e^{-x}
$$

for x > 0, which is simply the integrand of the normalized Gamma function. 

$E(X)=\frac{\alpha}{\lambda}$, $Var(X)=\frac{\alpha}{\lambda^2}$

Two specific situation:

$Gamma(1,\lambda)=Expo(\lambda)$, $Gamma(n/2,1/2)=\chi^2(n)$

### Chi-squared 

 Let $V = Z_1^2 +···+Z_n^2$ where the Zj ∼ N (0, 1) i.i.d. Then V has the chi-squared distribution with n degrees of freedom, $V ∼ χ^2_n$.

E(X) = n; Var(X) = 2n

### Student-t

Let Z∼N(0,1) and $V∼\chi^2$ be independent. Let

$$
T=\frac{Z}{\sqrt{V/n}}
$$

Then T has the Student-t distribution with n degrees of freedom, $T ∼ t_n$ .

The Student-t distribution looks much like the normal distribution but is heavier-tailed, especially if n is small. As n → ∞, we claim that the Student-t converges to the standard normal.

### Beta

The standard beta distribution, Beta(a, b), is defined by PDF

$$
\frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}x^{a-1}(1-x)^{b-1}
$$

for 1> x > 0, which is simply the integrand of the normalized Beta function. 

## Memorylessness

It usually refers to the cases when the distribution of a "waiting time" until a certain event does not depend on how much time has elapsed already. 

For discrete: **geomatric distribution** 

Suppose *X* is a discrete random variable whose values lie in the set {0, 1, 2, ...}. The probability distribution of *X* is **memoryless** precisely if for any *m* and *n* in {0, 1, 2, ...}, we have$P(X>m+n|X>m)=P(x>n)$

For continuous: **exponential distribution**

Suppose *X* is a continuous random variable whose values lie in the non-negative real numbers [0, ∞). The probability distribution of *X* is memoryless precisely if for any non-negative real numbers *t* and *s*, we have$P(X>t+s|X>t)=P(x>s)$

## St. Petersburg Paradox

Suppose you are given the offer to play a game where a coin is flipped until a heads is landed. Then, for the number of flips i made up to and including the heads, you receive $2i. How much should you be willing to pay to play this game? That is, what price would make the game fair, or the expected value zero?

[更好的解释](https://zhuanlan.zhihu.com/p/579732711)

Although the expection is infinite, the **rate of convergence** is very slow. The average benifit S approaches log2n according to the computer simulation. 

如果玩 n 次，总收益依概率为 nlog2⁡n，平均一局赚 log2⁡n 元，只要价格低于 log2⁡n 元就可以考虑去玩。

## Birthday Paradox

[完整的解释](https://m.ituring.com.cn/book/tupubarticle/20967?bookID=2068&type=tubook&subject=%E6%83%8A%E4%BA%BA%E7%9A%84%E2%80%9C%E5%90%8C%E6%9C%88%E5%90%8C%E6%97%A5%E7%94%9F%E2%80%9D) 

当我们看到“有人生日相同”时，下意识地会用“与我生日相同”去推测，而实际上“与我生日相同”的概率确实非常小。于是直觉告诉我们，“有人生日相同”的概率也很小。但是，“生日悖论”中真正的问题其实是23 人中至少有2 人以上生日相同的概率，而不论究竟是谁的生日。这与我们的直觉中预设的前提条件有着根本的不同。

An example: Suppose we have n people and we want to know the **approximate** probability that at least three individuals have the same birthday. 

Let X = # triple matches. Then we know that $E(X)=\frac{C_n^3365}{365^3}=\binom{n}{3}/365^2$

To approximate P (X ≥ 1), we approximate X ∼ Pois(λ) with λ = E(X). Then we have

$P(X\geq1)=1-P(X=0)=1-e^{-\lambda}\frac{\lambda^0}{0!}=1-e^{-\lambda}$
