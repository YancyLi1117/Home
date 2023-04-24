---
layout: post
title: Linear Model
date: 2023-04-23
author: Yancy
tags: [ML]
comments: true
toc: true
---

## Assumptions

- **Linear and additive relationship** between dependent and independent variables---Detect: visualization 自变量与因变量, 预测值与残差; Solution: 进行非线性变换

- Independent variables: **No correlation** ---or: multicollinearity   Detect:  **Variance Inflation Factor (VIF)** 

  Solution: 剔除、合并相关变量（PCA); 运用岭回归

- Residual error terms: **No correlation** ---or: autocorrelation; **have constant variance**---or: heteroskedasticity; **normally distributed**

Loss function: Mean Square Error / Quadratic Loss / L2 Loss

For $w=(w_1,w_2,w_3...w_n)^T, y=(y_1,y_2,y_3...y_N)^T,x_i=(x_i^1,x_i^2,x_i^3...x_i^n)$

The tips: $y: N\*1,X: N\*n,w: n\*1,x_i: 1\*n,y_i: 1\*1$

Interaction variables: When they have large main effects; the effect of one changes for various subgroups of the other (**opposite effect** /  **the same effect with different intensity** )

## Ordinary Least Square (OLS)

---minimize the loss, Y=Xw:

$$
w=\arg\min_wLoss=\arg\min_w\sum_{i=1}^N(y_i-f(x_i;w))^2\\L=\sum_{i=1}^{N}(y_i-x_iw)^2=||y-Xw||^2_2=(y-Xw)^T(y-Xw)\\\nabla_wL=2X^TXw-2X^Ty=0\Rightarrow w_{OLS}=(X^TX)^{-1}X^Ty
$$

If $(X^TX)^{-1}$is not full rank, and doesn't have an Analytic expression. Use **gradient descent**.

$w^{(\tau+1)}=w^{(\tau)}-\eta \nabla_w L$, $\tau$ is the step, $\eta$ is the learning rate.

### Result of Regression Analysis

| Type(SST=SSR+SSE)                                           |                                          | freedom degree          |                 | F value |
| ----------------------------------------------------------- | ---------------------------------------- | ----------------------- | --------------- | ------- |
| Variance between groups <br />sum of squares for regression | $\sum_{i=1}^N(\hat{y_i}-\overline{y})^2$ | n (#parameter)          | SSR/n----1      | 1/2     |
| Variance within groups<br />sum of squares for error        | $\sum_{i=1}^N(y_i-\hat{y_i})^2$          | N-n-1                   | SSE/(N-n-1)---2 |         |
| Total variance<br />sum of squares for total                | $\sum_{i=1}^N(y_i-\overline{y})^2$       | N-1 (unbias estimation) |                 |         |

R square: 1-SSE/SST

### Test the Model

$H_0:w_1=w_2=...w_n=0$; $H_1: for\ w_1...w_n$, there is at least one parameter$\ne 0$

calculate the p-value--- the min significance value to reject H0. The statistic F:

$$
F=\frac{SSR/n}{SSE/(N-n-1)}
$$

compare with the significance value when the model follows F(n, N-n-1).

### Test the variable

consider the residual error: $\epsilon \sim N(0, \sigma^2I),y=Xw+\epsilon$, 

the estimated w is $\hat{w}=(X^TX)^{-1}X^Ty=w+(X^TX)^{-1}X^T\epsilon$, so  $\hat{w}\sim N(w,\sigma^2(X^TX)^{-1})$	(w, X are const)

for one parameter $w_j$, $\hat{w_j}\sim N(w_j,\sigma^2(X^TX)^{-1}_{jj})$, after some complex deduction---w follows Students' t distribution

$H_0:w_j=0$, $H_1:w_j \ne0$

calculate the p-value--- the min significance value to reject H0. The statistic t: 

$$
t_j=\frac{\hat{w_j}}{\sqrt{\hat{\sigma}^2(X^TX)^{-1}_{jj}}} ,\hat{\sigma}^2=\frac{SSE}{N-n-1}
$$

compare with the significance value when the model follows t (N-n-1).

## Maximum Likelihood (MLE)

Then consider from probability, the basic assumption: the error follow the Normal Distribution

$$
y=Xw+\epsilon\quad \epsilon\sim N(0,\sigma^2I)\quad y\sim N(Xw,\sigma^2 I)\\p(y_i|x_i;w)=\frac{1}{\sqrt{2\pi}\sigma^2}\exp\{-\frac{1}{2\sigma^2}(y_i-x_iw)^2\}
$$

（x和w同时发生，y的概率）Then the maximum likelihood function:

$$
L(w)=L(w;X,y)=\prod^N_{i=1}p(y_i|x_i,w)
$$

It is very complex to do the exp---do the logarithm, it won't affect w

$$
\mathscr l (w_{MLE})=\ln L(w)=\prod_{i=1}^N\ln p(y_i|x_i,w)=\sum_{i=1}^N\ln p(y_i|x_i,w)\\=N\ln \frac{1}{\sqrt{2\pi\sigma^2}}-\frac{1}{2\sigma^2}\sum_{i=1}^N(y_i-x_iw)^2\\ w_{MLE}=\arg\min_w\sum_{i=1}^N(y_i-x_iw)^2
$$

The same formular as the OLS! So what's the difference between them? They are both ways of parameter estimation, but are totally different. **MLE基于概率让其最大; OLS基于距离/loss/误差让其最小**

## Maximum a Posteriori Probability (MAP)

If w itself follows a posteriori distribution, using MAP to estimate the parameter.

has the connection with Bayes' Theorem ---

$$
p(w|y,X)=\frac{p(y|X,w)p(w)}{p(y|X)}
$$

p(y\|X) is the same, so $L(w)=p(w\|y,X)=p(y\|X,w)p(w)=\prod_{i=1}^N p(y_i\|x_i,w)p(x)$

and do the logarithm, it won't affect w: $\mathscr l (w_{MAP})=\ln L(w)=\sum_{i=1}^N\ln p(y_i\|x_i,w)+\ln p(w)$

### Ridge Regression

when $w_i \sim N(0,\tau^2)$:

$$
\mathscr l (w_{MAP})=N\ln \frac{1}{\sqrt{2\pi\sigma^2}}+D\ln \frac{1}{\sqrt{2\pi\tau^2}}-\frac{1}{2\sigma^2}\sum_{i=1}^N(y_i-x_iw)^2-\frac{1}{2\tau^2}\sum_{j=1}^Dw_j^2\\
w_{MAP}=\arg\min_w \sum_{i=1}^N(y_i-x_iw)^2+\lambda\sum_{j=1}^Dw_j^2\\=\arg\min_w||y-Xw||^2_2+\lambda||w||^2_2
$$

consider from the loss function: add a **L2 regulation**

### LASSO Regression

when $w_i \sim Laplace(0,b)$:

$$
\mathscr l (w_{MAP})=N\ln \frac{1}{\sqrt{2\pi\sigma^2}}+D\ln \frac{1}{2b}-\frac{1}{2\sigma^2}\sum_{i=1}^N(y_i-x_iw)^2-\frac{1}{b}\sum_{j=1}^D|w_j|\\
w_{MAP}=\arg\min_w \sum_{i=1}^N(y_i-x_iw)^2+\lambda\sum_{j=1}^D|w_j|\\=\arg\min_w||y-Xw||^2_2+\lambda||w||_1
$$

consider from the loss function: add a **L1 regulation**

