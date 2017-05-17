# Properties of $R_{xy}(\tau)$

- The cross-correlation of functions f(t) and g(t) is equivalent to the convolution of f*(−t) and g(t). That is:

$$f\star g=f^{\star}(-t)\star g$$

- If $f$ is a Hermitian function, then $f\star g=f*g$.
- If both $f$ and $g$ are Hermitian, then $f\star g=g\star f$.

$$(f\star g)\star (f\star g)=(f\star f)\star (g\star g)$$

- Analogous to the convolution theorem, the cross-correlation satisfies

$${\mathcal {F}}\{f\star g\}=({\mathcal {F}}\{f\})^{\star}\cdot {\mathcal {F}}\{g\},$$

where ${\mathcal {F}}$ denotes the Fourier transform, and an asterisk again indicates the complex conjugate. Coupled with fast Fourier transform algorithms, this property is often exploited for the efficient numerical computation of cross-correlations [7] (see circular cross-correlation).
- The cross-correlation is related to the spectral density (see Wiener–Khinchin theorem).
- The cross-correlation of a convolution of $f$ and $h$ with a function $g$ is the convolution of the cross-correlation of $g$ and $f$ with the kernel $h$:
$$g\star (f*h)=(g\star f)\star h$$

## Time series analysis
In time series analysis, as applied in statistics, the cross-correlation between two time series is the normalized cross-covariance function.

Let $(X_{t},Y_{t})$ represent a pair of stochastic processes that are jointly wide-sense stationary. Then the cross-covariance and the cross-correlation are given by

Cross-covariance	$\gamma_{XY}(\tau )=E[(X_{t}-\mu_{X})(Y_{t+\tau }-\mu_{Y})]$
Cross-correlation	$$\rho_{XY}(\tau )={\frac {1}{\sigma_{X}\sigma_{Y}}}E[(X_{t}-\mu_{X})\,(Y_{t+\tau }-\mu_{Y})],$$
where $\mu_{X}$ and $\sigma_{X}$ are the mean and standard deviation of the process $(X_{t})$, which are constant over time due to stationarity; and similarly for $(Y_{t})$, respectively. $E[\ ]$ indicates the expected value. That the cross-covariance and cross-correlation are independent of $t$ is precisely the additional information (beyond being individually wide-sense stationary) conveyed by the requirement that $(X_{t},Y_{t})$ are jointly wide-sense stationary.

The cross-correlation of a pair of jointly wide sense stationary stochastic processes can be estimated by averaging the product of samples measured from one process and samples measured from the other (and its time shifts). The samples included in the average can be an arbitrary subset of all the samples in the signal (e.g., samples within a finite time window or a sub-sampling[which?] of one of the signals). For a large number of samples, the average converges to the true cross-correlation.

# Properties of $E[\cdot]$

## Constants

The expected value of a constant is equal to the constant itself; i.e., if $c$ is a constant, then $E[c]=c$. Particularly, this means: $E[E[X]]=E[X]$.

## Monotonicity

If $X$ and $Y$ are random variables such that $X\le Y$ almost surely, then $E[X]\le E[Y]$.

## Linearity

The expected value operator (or expectation operator) E[\cdot ]} \operatorname {E}[\cdot ] is linear in the sense that

$$E[X+Y]=E[X]+E[Y],\\E[aX]=aE[X].$$
The first result is valid even if $X$ is not statistically independent of Y} Y. Combining the two equations with the expectation of a constant, we can see that

$$E[aX+bY+c]=aE[X]+bE[Y]+c\,$$
for any two random variables $X$ and $Y$ (which need to be defined on the same probability space) and any real numbers a, b and c.

## Iterated expectation

Iterated expectation for discrete random variables
For any two discrete random variables X} X, $Y$ one may define the conditional expectation:[7]

$$E[X\mid Y=y]=\sum \limits_{x}x\cdot P(X=x\mid Y=y)$$
Here E[X | $Y$ = y] is a function of y. Let g(y) be that function of y; then E[X | Y] is a random variable in its own right and is equal to g(Y).

Lemma. The expectation of $X$ satisfies:[8]

$$E[X]=E\left[E[X\mid Y]\right].$$


## Iterated expectation for continuous random variables
In the continuous case, the results are completely analogous. The definition of conditional expectation would use inequalities, density functions, and integrals to replace equalities, mass functions, and summations, respectively. However, the main result still holds:

$$E[X]=E[E[X\mid Y]]$$

## Inequality

If a random variable $X$ is always less than or equal to another random variable Y, the expectation of $X$ is less than or equal to that of Y:

If $X$ ≤ Y, then E[X] ≤ E[Y].
In particular, if we set $Y$ to |X| we know $X$ ≤ $Y$ and −X ≤ Y. Therefore, we know E[X] ≤ E[Y] and E[−X] ≤ E[Y]. From the linearity of expectation we know −E[X] ≤ E[Y]. Therefore, the absolute value of expectation of a random variable is less than or equal to the expectation of its absolute value:

$${\big |}E[X]{\big |}\leq E{\big [}|X|{\big ]}$$
This is a special case of Jensen's inequality.

## Non-multiplicativity

If one considers the joint probability density function of $X$ and $Y$, say $j(x,y)$, then the expectation of $XY$ is

$$E[XY]=\iint xy\,j(x,y)\,dx\,dy$$
In general, the expected value operator is not multiplicative, i.e. $E[XY]$ is not necessarily equal to $E[X]·E[Y]$. The amount by which multiplicativity fails is called the covariance:

$$\text{Cov}(X,Y)=E[XY]-E[X]E[Y]$$
Thus multiplicativity holds precisely when $\text{Cov}(X, Y) = 0$, in which case $X$ and $Y$ are said to be uncorrelated.

Independent variables are a notable case of uncorrelated variables. If $X$ and $Y$ are independent, then by definition j(x,y) = f(x)g(y) where $f$ and $g$ are the marginal PDFs for $X$ and Y. Then

$$E[XY]=\iint xy\,j(x,y)\,dx\,dy=\iint xyf(x)g(y)\,dy\,dx\\=\left[\int xf(x)\,dx\right]\left[\int yg(y)\,dy\right]=E[X]E[Y]$$
and $\text{Cov}(X, Y) = 0$.

Observe that independence of $X$ and $Y$ is required only to write j(x, y) = f(x)g(y), and this is required to establish the second equality above. The third equality follows from a basic application of the Fubini–Tonelli theorem.

## Functional non-invariance
In general, with the exception of linear functions, the expectation operator and functions of random variables do not commute; that is

$$E[g(X)]=\int_{\Omega }g(X)\,dP\neq g(E[X])$$
A notable inequality concerning this topic is Jensen's inequality, involving expected values of convex (or concave) functions.

Relation to characteristic function
The probability density function of a scalar random variable $X$ is related to its characteristic function φX by the inversion formula:

$$f_{X}(x)={\frac {1}{2\pi }}\int_{\mathbf {R} }e^{-itx}\varphi_{X}(t)\,dt$$
We can use this inversion formula in expected value of a function g(X) to obtain

$$E[g(X)]={\frac {1}{2\pi }}\iint g(x)e^{-itx}\varphi_{X}(t)\,dt\,dx$$
Changing the order of integration, we get

$$E[g(X)]={\frac {1}{2\pi }}\int G(t)\varphi_{X}(t)\,dt$$
where

$$G(t)=\int g(x)e^{-itx}\,dx$$
is the Fourier transform of $g(x)$. This can also be seen as a direct consequence of Plancherel theorem.
