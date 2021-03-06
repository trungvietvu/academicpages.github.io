---
title: "Estimating a Dirichlet Distribution using Expectation–Maximization"
collection: notes
permalink: /notes/2017/DirichletDist
date: 2017-01-12
enable: true
excerpt: This note describes the EM algorithm for estimating a Dirichlet distribution. One result that I found particularly interesting is the posterior distributions of gamma-distributed random variables given the observation of their corresponding Dirichlet distribution are also gamma-distributed.
---


# 1. Dirichlet distribution
Let $$\newcommand{\E}{\mathbb E} \newcommand{\bm}[1]{\boldsymbol#1} \bm \alpha = (\alpha_1, \ldots, \alpha_K)$$ be a vector of positive reals. For each $$k=1,\ldots,K$$, denote $$X_k$$ the random variable that is gamma-distributed with shape $$\alpha_k$$ and scale $$1$$ (or $X_k \sim \text{Gamma}(\alpha_k,1)$). The density of $X_k$ is given by

$$ f(x_k  \mid \alpha_k) = \frac{x_k^{\alpha_k-1} e^{-x_k}}{\Gamma(\alpha_k)} , \tag{1} \label{gamma} $$

where the [gamma function](https://en.wikipedia.org/wiki/Gamma_function) is define as $$ \Gamma(z)=\int_0^{\infty} x^{z-1} e^{-x}dx $$. Then the random vector $$\bm{P} =(P_1,\ldots,P_K)$$, defined by $$P_k = \frac{X_k}{\sum_{j=1}^K X_j}$$ for $k=1,\ldots,K$, follows a Dirichlet distribution of order $$K$$ with parameter $$\bm{\alpha}$$ (or $\bm P \sim \text{Dir}(\bm \alpha)$):

\begin{aligned}
f(\bm p \mid \bm \alpha) &= \frac{\Gamma\bigl(\sum_{k=1}^K \alpha_k \bigr)}{\prod_{k=1}^K \Gamma(\alpha_k)} \prod_{k=1}^K p_k^{\alpha_k-1} .
\end{aligned}

Lemma 1.
: Let $X \sim \text{Gamma}(\alpha,\theta)$. Then $\E[\log X] = \psi(\alpha) + \log \theta$, where $\psi(z) = \frac{d}{dz} \log \Gamma(z) = \frac{\Gamma'(z)}{\Gamma(z)}$ is the [digamma function](https://en.wikipedia.org/wiki/Digamma_function).

Lemma 2.
: Let $\bm X$ and $\bm P$ be random vectors defined as above. Given one observation $$\bm p=(p_1,\ldots,p_K)$$, we have 
\begin{align} X_k \mid \bm P,\bm \alpha \sim \text{Gamma}\Bigl(\sum_{k=1}^K \alpha_k, p_k \Bigr) . \end{align}

(See [Appendix](#appendix))


# 2. Single sample estimation
Let us first consider the problem of estimating the parameter $$\bm \alpha$$ of a Dirichlet distribution given *one* observation $$\bm p=(p_1,\ldots,p_K)$$ such that $$\sum_{k=1}^K p_k=1$$. Following the principle of maximum likelihood estimation (MLE), we compute the incomplete log likelihood as

$$ l(\bm \alpha; \bm p) = \log f(\bm p \mid \bm \alpha) = \sum_{k=1}^K \alpha_k \log p_k - \sum_{k=1}^K \log \Gamma(\alpha_k) + \log \Gamma\bigl(\sum_{k=1}^K \alpha_k \bigr) + const . $$

One can try gradient ascent to maximize $l(\bm \alpha; \bm p)$ by computing its derivative:

\begin{aligned}
\frac{\partial l}{\partial \alpha_k} = n \bigg( t_k - \psi(\alpha_k) + \psi \bigl(\sum_{k=1}^K \alpha_k \bigr) \bigg) .
\end{aligned}

However, in this note we focus on an alternative approach that uses [expectation–maximization](https://en.wikipedia.org/wiki/Expectation%E2%80%93maximization_algorithm) (EM) algorithm. Consider the complete log likelihood instead: 

\begin{aligned}
l(\bm \alpha; \bm p, \bm x) &= \log f(\bm p, \bm x \mid \bm \alpha) = \log f(\bm p \mid \bm x) + \log f(\bm x \mid \bm \alpha) \\\\\\
&= \sum_{k=1}^K \log f(p_k \mid \bm x) + \sum_{k=1}^K \log f(x_k|\alpha_k) \\\\\\
&= \sum_{k=1}^K \alpha_k \log x_k - \sum_{k=1}^K \log \Gamma(\alpha_k) + const .
\end{aligned}

In E-step, we identify the surrogate function as follows:

\begin{aligned} Q(\bm \alpha \mid \bm \alpha') &= \E_{\bm X \mid \bm P,\bm \alpha'} [l(\bm \alpha; \bm p, \bm x)] \\\\\\
&= \sum_{k=1}^K \alpha_k \E_{\bm X \mid \bm P,\bm \alpha'} [\log x_k] - \sum_{k=1}^K \log \Gamma(\alpha_k) + const \\\\\\
&= \sum_{k=1}^K \alpha_k \E_{X_k \mid \bm P,\bm \alpha'} [\log x_k] - \sum_{k=1}^K \log \Gamma(\alpha_k) + const \\\\\\
&= \sum_{k=1}^K \alpha_k \bigg( \psi\bigl(\sum_{k=1}^K \alpha_k^\prime \bigr) + \log p_k \bigg) - \sum_{k=1}^K \log \Gamma(\alpha_k) + const \\\\\\
&= \sum_{k=1}^K \Bigl( \alpha_k \log p_k - \log \Gamma(\alpha_k) \Bigr) + \Bigl( \sum_{k=1}^K \alpha_k \Bigr) \psi\bigl(\sum_{k=1}^K \alpha_k^\prime \bigr) + const . \end{aligned}

where the second last equality stems from the fact that $X_k \mid \bm P,\bm \alpha' \sim \text{Gamma}\Bigl(\sum_{k=1}^K \alpha_k^\prime, p_k \Bigr)$. This non-trivial fact is shown in Lemma 2.


# 3. Multiple samples estimation
Similarly, suppose there are $$n$$ observations $$\{ \bm p_1, \ldots, \bm p_n \}$$. We have

\begin{aligned}
l(\bm \alpha;\bm p) &= n\Bigg( \sum_{k=1}^K \bigg( \alpha_k t_k - \log \Gamma(\alpha_k) \bigg) + \log \Gamma(\sum_{k=1}^K \alpha_k) \Bigg) + const \\\\\\
Q(\bm \alpha \mid \bm \alpha') &= n \sum_{k=1}^K \Bigg( \alpha_k \bigg( \psi \bigl(\sum_{k=1}^K \alpha_k^\prime \bigr) + t_k \bigg) - \log \Gamma(\alpha_k) \Bigg) + const \\\\\\
&= n\Bigg( \sum_{k=1}^K \bigg( \alpha_k t_k - \log \Gamma(\alpha_k) \bigg) + \bigg( \sum_{k=1}^K \alpha_k \bigg) \psi \bigl(\sum_{k=1}^K \alpha_k^\prime \bigr) \Bigg) + const \\\\\\
\end{aligned}

where $$t_k = \frac{1}{n}\sum_{i=1}^n \log p_{ik}$$ is the *observed sufficient statistics*.

Another way to construct the surrogate function is to use a lower bound on $$\Gamma(\sum_{k=1}^K \alpha_k)$$:

\begin{aligned}
\Gamma(x) \geq \Gamma(y) e^{(x-y)\psi(y)}.
\end{aligned}

This approach is known as the fixed-point iteration (see *[1]*).


# 3. Maximization step
In M-step, we maximize $$Q$$ by setting its gradient to zero.

\begin{aligned}
\frac{\partial Q}{\partial \alpha_k} &= n \bigg( t_k - \psi(\alpha_k) + \psi \bigl(\sum_{k=1}^K \alpha_k^\prime \bigr) \bigg) = 0 \\\\\\
\Rightarrow \alpha_k &= \psi^{-1} \bigg( \psi \bigl(\sum_{k=1}^K \alpha_k^\prime \bigr) + t_k \bigg).
\end{aligned}



---
# Appendix

**Proof of Lemma 2.** Without loss of generality, we will prove the case with $k=1$. Then

\begin{aligned}
f(x_1, \bm p \mid \bm \alpha) &= \int_{x_2} \ldots \int_{x_K} f(x_1,\ldots,x_K,p_1,\ldots,p_K | \alpha_1,\ldots,\alpha_K) dx_2 \ldots dx_K \\\\\\
&= \int_{x_2} \ldots \int_{x_K} \prod_{k=1}^K \bigg( f(x_k | \alpha_k) f(p_k \mid \bm x) \bigg) dx_2 \ldots dx_K \\\\\\
&= \int_{x_2} \ldots \int_{x_K} \prod_{k=1}^K f(x_k | \alpha_k) \prod_{k=1}^K \delta \Bigg( p_k-\frac{x_k}{\sum_{k=1}^K x_k} \Bigg) dx_2 \ldots dx_K \\\\\\
&= f(x_1|\alpha_1) \int_s \delta(p_1-\frac{x_1}{s}) \delta(s-\sum_{i=1}^K x_i) \int_{x_K} f(x_K|\alpha_K) \delta(p_K-\frac{x_K}{s}) \\\\\\
&\qquad \ldots \int_{x_2} f(x_2|\alpha_2) \delta(p_2-\frac{x_2}{s}) dx_2 \ldots dx_K ds ,
\end{aligned}

where $\delta(.)$ is the [Dirac delta function](https://en.wikipedia.org/wiki/Dirac_delta_function). Using the scaling and translation property, i.e., $\delta(\alpha t) = \frac{1}{\alpha} \delta(t)$ and $\int_{t} f(t)\delta(t-T)dt = f(T)$, we have

\begin{aligned}
f(x_1, \bm p \mid \bm \alpha) &= f(x_1|\alpha_1) \int_s \delta(p_1-\frac{x_1}{s}) \delta(s-x_1-\sum_{k=2}^K x_k) \int_{x_K} f(x_K|\alpha_K) \delta(p_K-\frac{x_K}{s}) \\\\\\
&\qquad \ldots \int_{x_2} f(x_2|\alpha_2) s \delta(x_2 - sp_2) dx_2 \ldots dx_K ds \\\\\\
&= f(x_1|\alpha_1) \int_s s f(sp_2 | \alpha_2)  \delta(p_1-\frac{x_1}{s}) \delta(s-x_1-sp_2 - \sum_{k=3}^K x_k) \int_{x_K} f(x_K|\alpha_K) \delta(p_K-\frac{x_K}{s}) \\\\\\
&\qquad \ldots \int_{x_3} f(x_3|\alpha_3) s \delta(x_3 - sp_3) dx_3 \ldots dx_K ds \\\\\\
&\qquad \vdots \\\\\\
&= f(x_1|\alpha_1) \int_s s^{K-1} f(sp_2 | \alpha_2) \ldots f(sp_K | \alpha_K)  \delta(p_1-\frac{x_1}{s}) \delta(s-x_1-sp_2-\ldots-sp_K) ds 
\end{aligned}

Now using (\ref{gamma}) and the fact that $\sum_{k=1}^K p_k=1$, we obtain

\begin{aligned}
f(x_1, \bm p \mid \bm \alpha) &= \frac{x_1^{\alpha_1-1}e^{-x_1}}{\Gamma(\alpha_1)} \int_s s^{K-1} \prod_{k=2}^K \frac{(sp_k)^{\alpha_k-1}e^{-sp_k}}{\Gamma(\alpha_k)} \delta(p_1-\frac{x_1}{s}) \delta(x_1-sp_1) ds \\\\\\
&= \frac{x_1^{\alpha_1-1} e^{-x_1} \prod_{k=2}^K p_k^{\alpha_k-1}}{\prod_{k=1}^K \Gamma(\alpha_k)} \int_s s^{\sum_{k=2}^K \alpha_k} e^{-s(1-p_1)} \delta(p_1-\frac{x_1}{s}) \frac{\delta(s-\frac{x_1}{p_1})}{p_1}ds \\\\\\
&= \frac{\Gamma(\sum_{k=1}^K \alpha_k)}{\prod_{k=1}^K \Gamma(\alpha)} \prod_{k=1}^K p_k^{\alpha_k-1} e^{-x_1} x_1^{\alpha_1-1} \bigg( \frac{x_1}{p_1} \bigg)^{\sum_{k=2}^K \alpha_k} e^{-\frac{x_1}{p_1} + x_1} \\\\\\
&= f(\bm p \mid \bm \alpha) \frac{x_1^{\sum_{k=1}^K \alpha_k-1} e^{-\frac{x_1}{p_1}}}{\Gamma(\sum_{k=1}^K \alpha_k) p_1^{\sum_{k=1}^K \alpha_k}} .
\end{aligned}

Rearranging terms gives us the probability density function that follows a gamm distribution:

\begin{aligned}
f(x_1 \mid \bm p, \bm \alpha) &= \frac{f(x_1, \bm p \mid \bm \alpha)}{f(\bm p \mid \bm \alpha)} = \frac{x_1^{\sum_{k=1}^K \alpha_k-1} e^{-\frac{x_1}{p_1}}}{\Gamma(\sum_{k=1}^K \alpha_k) p_1^{\sum_{k=1}^K \alpha_k}}. \qquad \blacksquare
\end{aligned}


## References
> 1. T. Minka. Estimating a Dirichlet distribution, 2000.
