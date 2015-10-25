---
title: The German Tank Problem For Competitive Intel
date: 2015-09-26
tags: chain, probability, statistics
---

#The German Tank Problem
##Or Why To Obfuscate Your User IDs

(note, this is pulled largely from the Wikipedia page, where there is additional derivation
of these equations.  This post started as an exercise in understanding by explaining, and ended
as an exercise in learning LaTex and seeing how high I could raise my blood pressure)

In World War II, Allied intelligence used conventional intelligence and statistical
estimation to try to guess the production of German tanks.

The statistical estimates, which were based on a handful of serial numbers from
captured tanks, turned out to be much, much more accurate.

From a frequentist perspective, this estimation is given by the formula:

\\[N = m + {m - k \over k}\\]

where m is the max serial number from the sample, and k is the size of the sample.
Intuitively, this is sample maximum plus the average gap between observations in the sample


A Bayesian analysis yields a probability mass function.

\\[P(N = n) =
\begin{cases}
  0 & \quad \text{if } n < m\\\\cr
  {k -1 \over k} {{ m - 1 \choose k - 1 } \over { n \choose k }} & \quad \text{otherwise}
\end{cases}\\]

> A probability mass function differs from a probability density function (pdf) in that the latter is associated with continuous rather than discrete random variables; the values of the latter are not probabilities as such: a pdf must be integrated over an interval to yield a probability.  (source: Wikipedia)


From this we can get the mean and standard deviation of the distribution in order to estimate.

\\[N \approx \mu \pm \sigma \\]

The mean of a probability distribution is:

\\[\mu = \sum_{\substack{x}} xP(x)\\]

(Note: in wikipedia, this is referred to as "order of magnitude" -- why?)

So, in this case it is:

\\[\mu = \sum_{\substack{n}} n \times (N = n \text{ | } M = m, K = k) \\]

Alright, so what's the probability that N = n, given m and k?

Conditional probability tells us that:

\\[(n | m,k) = (m | n,k){(n | k) \over (m | k)}\\]

The first term on the right-hand side is the probability that the max serial number
is equal to m, when n is known

The numerator of the second term is the credibility that the total number of tanks
is equal to n when k tanks have been observed but before the serial numbers have
been observed. Assume that it has a discrete uniform distribution.

The denominator is  the probability that the maximum serial number is equal to m
once k tanks have been observed but before the serial numbers have actually been observed.
\scriptstyle (m\mid k) can be re-written in terms of the other quantities by marginalizing
over all possible \scriptstyle n.

For k >= 4, there are formulas to get the mean and standard deviation (see wikipedia for derivation)
