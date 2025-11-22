### Recall: Shannon-type inequalities
i.e. all entropy inequalities that can be derived using:
1.  monotonicity: $H(X)\le H(X,Y)$
2.  submodularity: $I(X;Y|Z)\ge0$

This lecture will cover some non-Shannon-type inequalities.

---

> [!NOTE] Def (differential entropy)
> For a RV X with a density f on $\mathbb{R}^{d}$, its differential entropy is defined as
$$ h(X):=h(f):=\int_{\mathbb{R}^{d}}-f(x)\log f(x)dx. $$ 

Note:
1.  $h(X)\in \mathbb{R}\cup\{\pm\infty\}$. In particular, it can be negative.
2.  $h(aX)=h(X)+\log|a|$, for $a\in \mathbb{R}$
3.  $h(X)\le h(X,Y)$ no longer holds. However, it's still true that $I(X;Y)=h(X)+h(Y)-h(X,Y)\ge0$.

**Example.** If $X\sim N(\mu,\Sigma)$ then $f(x)=\frac{1}{\sqrt{(2\pi)^{d}\det(\Sigma)}}\exp[-\frac{1}{2}(x-\mu)^{T}\Sigma^{-1}(x-\mu)]$, so
$$ h(X)=\mathbb{E}[log \frac{1}{f(x)}] = E\left[\frac{1}{2}\log((2\pi)^{d}\det(\Sigma))+\frac{1}{2}(x-\mu)^{T}\Sigma^{-1}(x-\mu)\right] $$
$$ =\frac{d}{2}\log(2\pi e)+\frac{1}{2}\log \det \Sigma. $$

---


> [!Tip] Theorem (entropy power inequality, EPI)
> For independent RVs X,Y on $\mathbb{R}^d$,
$$ e^{\frac{2}{d}h(X+Y)} \ge e^{\frac{2}{d}h(X)} + e^{\frac{2}{d}h(Y)} $$

Note:
1.  Equality holds iff X, Y are Gaussian, and $\Sigma_{X}=c\Sigma_{Y}$.
2.  EPI shows that, for given values of $h(X)$ and $h(Y)$, $h(X+Y)$ is minimized when X, Y are Gaussian.

We will present the proof in Stam (1959).

### Detour: Fisher information
For a RV X with density f, the Fisher information is
$$ J(X) := \int_{\mathbb{R}} \frac{(f'(x))^{2}}{f(x)} dx. $$

Recall Fisher information $I(\theta)$ in Lec 3: for $Y\sim P_\theta$,
$$ I(\theta):=I^{Y}(\theta):=\int\frac{(\frac{\partial}{\partial\theta}p_{\theta})^{2}}{p_{\theta}}dx $$
They are connected via $I^{Y}(\theta)\equiv J(X)$ when $Y=\theta+X$.

**Properties:**
1. $I^{Y}(\theta)\equiv J(X)$ when $Y=\theta+X$
2.  $J(aX)=\frac{1}{a^{2}}J(X)$
3.  DPI: $I^{Y}(\theta)\le I^{X}(\theta)$ if $\theta-X-Y$ is a Markov chain.
    (Pf: $I^{Y}(\theta)=\lim_{\Delta\rightarrow0}\frac{1}{\Delta^{2}}\chi^{2}(P_{Y|\theta+\Delta}||P_{Y|\theta})\le \lim_{\Delta\rightarrow 0}\frac{1}{\Delta^{2}}\chi^{2}(P_{X|\theta+\Delta}||P_{X|\theta})=I^{X}(\theta)$)

> [!Tip] Theorem (Stam)
> For independent $X_1, X_2$:
$$ \frac{1}{J(X_1+X_2)} \ge \frac{1}{J(X_1)} + \frac{1}{J(X_2)} $$
or equivalently,
$$ (a+b)^{2}J(X_1+X_2) \le a^{2}J(X_1)+b^{2}J(X_2), \forall a, b>0. $$

Pf. 
Write $Y_1=a\theta+X_1$, $Y_2=b\theta+X_2$, then
$I^{Y_1}(\theta) = I^{X_1/a}(\theta) = J(\frac{X_1}{a})=a^{2}J(X_1)$.
Therefore, $(a+b)^{2}J(X_1+X_2)=I^{Y_1+Y_2}(\theta)\le I^{Y_1,Y_2}(\theta)=I^{Y_1}(\theta)+I^{Y_2}(\theta)=a^{2}J(X_1)+b^{2}J(X_2)$.

**Proof.**
Let \( a,b > 0 \).
$$
(a+b)^2 J(X_1+X_2) \le a^2 J(X_1) + b^2 J(X_2).
$$
Define
$$
a^2 J(X_1) = J(X_{1/a}) = I^{\theta + \tfrac{X_1}{a}}(\theta) - I^{\theta}(\theta), b^2 J(X_2) = I^{Y_2}(\theta).
$$
Let
$$
Y_1 = a\theta + X_1, \quad Y_2 = b\theta + X_2.
$$
Then
$$
Y_1 + Y_2 = (a+b)\theta + X_1 + X_2,
$$
so that
$$
a^2 J(X_1) + b^2 J(X_2)

= I^{(Y_1,Y_2)}(\theta)

\ge I^{Y_1 + Y_2}(\theta)

= J\!\left(\tfrac{X_1 + X_2}{a+b}\right)

= (a+b)^2 J(X_1+X_2).
$$


> [!Tip]  Theorem (de Bruijn)
> For $Z\sim N(0,1)$ independent of X, then for $a>0$
$$ \frac{d}{da} h(X+\sqrt{a}Z) = \frac{1}{2} J(X+\sqrt{a}Z). $$
Pf. Let $p_a=P*N(0,a)$ be the density of $X+\sqrt{a}Z$, then
$$ \frac{\partial p_{a}}{\partial a}=\frac{1}{2}p_{a}^{\prime\prime} \quad (*) $$


---

To see (*), just note that for any test function f,
$$ \frac{\partial}{\partial a}E_{p_{a}}[f] = \lim_{\Delta\rightarrow0}\frac{1}{\Delta}E[f(X+\sqrt{a+\Delta}Z)-f(X+\sqrt{a}Z)] $$
(Z' independent copy of Z)
$$ = \lim_{\Delta\rightarrow0}\frac{1}{\Delta}E[f(X+\sqrt{a}Z+\sqrt{\Delta}Z')-f(X+\sqrt{a}Z)] $$
$$ = \lim_{\Delta\rightarrow 0}\frac{1}{\Delta} E[ f'(X+\sqrt{a}Z)\sqrt{\Delta}Z' + \frac{1}{2}f''(X+\sqrt{a}Z)\Delta Z'^{2}+o(\Delta)] $$
$$ =\frac{1}{2}E[f'']=\frac{1}{2}\int f p_{a}^{\prime\prime} dx \text{ (integration by parts)} $$
Therefore,
$$ \frac{d}{da} h(X+\sqrt{a}Z) = -\int(1+\log p_{a})\frac{\partial p_{a}}{\partial a}dx = -\frac{1}{2}\int(1+\log p_{a})p_{a}^{\prime\prime}dx $$
(integration by parts)
$$ = \frac{1}{2} \int \frac{(p_a')^2}{p_a} dx = \frac{1}{2}J(X+\sqrt{a}Z). $$


---

### Proof of EPI
1.  **d=1:** Let $X_\lambda = X*N(0, f(\lambda))$, $Y_\lambda = Y*N(0, g(\lambda))$ for some functions $f, g$ TBD. We have
    $$ \frac{d}{d\lambda}[e^{2H(X_{\lambda})}] = 2e^{2H(X_{\lambda})}H'(X_\lambda) = e^{2H(X_{\lambda})}J(X_{\lambda})f'(\lambda) $$
    $$ \frac{d}{d\lambda}\left[\frac{e^{2H(X_{\lambda})}+e^{2H(Y_{\lambda})}}{e^{2H(X_{\lambda}+Y_{\lambda})}}\right] = \frac{1}{e^{2H(X_{\lambda}+Y_{\lambda})}} ( e^{2H(X_{\lambda})}J(X_{\lambda})f'(\lambda)+e^{2H(Y_{\lambda})}J(Y_{\lambda})g'(\lambda) - (e^{2H(X_{\lambda})}+e^{2H(Y_{\lambda})})J(X_{\lambda}+Y_{\lambda})(f'(\lambda)+g'(\lambda)) ) $$
    Choosing $f'(\lambda)=e^{2H(X_{\lambda})}$ and $g'(\lambda)=e^{2H(Y_{\lambda})}$, then by Stam's inequality, the derivative is $\ge 0$ for all $\lambda > 0$.
    As $\lambda \to \infty$, both $X_\lambda$ and $Y_\lambda$ become more and more Gaussian, so the ratio approaches 1.
    Therefore, the ratio at $\lambda=0$ must be $\le 1$, which is the EPI.

2.  **General $d \ge 2$ by induction:**
    $$ h(X^d+Y^d) = h(X^{d-1}+Y^{d-1}) + h(X_d+Y_d|X^{d-1}+Y^{d-1}) $$
    $$ \ge h(X^{d-1}+Y^{d-1}) + h(X_d+Y_d|X^{d-1},Y^{d-1}) \quad \text{(conditioning reduces entropy)} $$
    $$ \ge \frac{d-1}{2}\log(e^{\frac{2}{d-1}h(X^{d-1})}+e^{\frac{2}{d-1}h(Y^{d-1})}) \quad \text{(induction hypothesis)} $$
    $$ + \frac{1}{2}E_{X^{d-1},Y^{d-1}}\log(e^{2h(X_d|X^{d-1}=x^{d-1})}+e^{2h(Y_d|Y^{d-1}=y^{d-1})}) \quad \text{(X indep. Y)} $$
    $$ \ge \frac{d-1}{2}\log(e^{\frac{2}{d-1}h(X^{d-1})}+e^{\frac{2}{d-1}h(Y^{d-1})}) + \frac{1}{2}\log(e^{2h(X_d|X^{d-1})}+e^{2h(Y_d|Y^{d-1})}) \quad \text{by convexity of } (x,y)\mapsto \log(e^x+e^y) $$
    $$ \ge \frac{d}{2}\log(e^{\frac{2}{d}h(X^{d-1})+\frac{2}{d}h(X_d|X^{d-1})}+e^{\frac{2}{d}h(Y^{d-1})+\frac{2}{d}h(Y_d|Y^{d-1})}) \quad \text{by convexity of } (x,y)\mapsto \log(e^x+e^y) \text{ again} $$
    $$ = \frac{d}{2}\log(e^{\frac{2}{d}h(X^d)}+e^{\frac{2}{d}h(Y^d)}). $$

---

### Example: Entropic CLT
Let $X_1, X_2, \ldots$ be i.i.d., $E[X_1]=0$, $Var(X_1)=1$, and $h(X_1)>-\infty$.
Let $T_n = \frac{1}{\sqrt{n}}\sum_{i=1}^n X_i$ be the standardized sum. Then by EPI,
$$ h(T_{n+m}) = h\left(\sqrt{\frac{m}{n+m}} \frac{1}{\sqrt{m}}\sum_{i=1}^m X_i + \sqrt{\frac{n}{n+m}} \frac{1}{\sqrt{n}}\sum_{i=m+1}^{m+n} X_i\right) $$
$$ \ge \frac{1}{2}\log\left(e^{2h(\sqrt{\frac{m}{n+m}}T_m)} + e^{2h(\sqrt{\frac{n}{n+m}}T_n)}\right) $$
$$ = \frac{1}{2}\log\left(\frac{m}{n+m}e^{2h(T_m)} + \frac{n}{n+m}e^{2h(T_n)}\right) $$
In other words, the sequence $(n+m)e^{2h(T_{n+m})} \ge m e^{2h(T_m)} + n e^{2h(T_n)}$.
Moreover since $Var(T_n)=1$, the maximum entropy principle implies $h(T_n) \le \frac{1}{2}\log(2\pi e)$.
Therefore, $h(T_n)$ must have a limit $h^*$, and
$$ D_{KL}(P_{T_n} || N(0,1)) = -h(T_n) + \frac{1}{2}\log(2\pi e) \to D^* $$
Barron (1986) shows that $D^*=0$, a result known as the **entropic CLT**.

---

## Information and estimation in Gaussian model

Let X be a general RV, $Y_\nu = \sqrt{\nu}X + Z$, where $Z \sim N(0,1)$ is independent of X, and $\nu > 0$ is the SNR parameter.


> [!Tip] ### Thm (I-MMSE)
> $$ \frac{d}{d\nu} I(X;Y_\nu) = \frac{1}{2} E[(X-E[X|Y_\nu])^2] =: \frac{1}{2}\text{mmse}(X|Y_\nu). $$

Note:
1.  Perhaps the most surprising part is that this is an equality.
2.  $\text{mmse}(X|Y) = E[|X-E[X|Y]|^2] = \min_{f(\cdot)} E[|X-f(Y)|^2]$ is called the minimum mean squared error for estimating X based on Y.

There are several proofs, but the most generalizable one is via SDEs.

A more general result: if $dY_t = X_t dt + dB_t$, $t \in [0,T]$, then
$$ I(X^T;Y^T) = \frac{1}{2} \int_0^T E[(X_t - E[X_t|Y^t])^2] dt. $$
To see how it implies the I-MMSE formula, take $X_t \equiv X$. Then $Y_T$ is a sufficient statistic for X. Moreover, $\frac{Y_T}{\sqrt{T}} = \sqrt{T}X + N(0,1)$, so the SNR parameter is T.

---

### Proof Sketch via SDEs

**Lemma 1.** For $dY_t = f(t)dt + dB_t$ with $f(t)$ adapted to the filtration $F^Y$, then (Girsanov's Theorem)
$$ \log \frac{d P_{Y^T}}{d P_{B^T}}(\{\xi^T\}) = \int_0^T f(t)d\xi_t - \frac{1}{2}\int_0^T f(t)^2 dt $$

**Lemma 2.** For $dY_t = X_t dt + dB_t$, then
$$ \tilde{B}_t = Y_t - \int_0^t E[X_s|Y^s] ds $$
is a Brownian Motion adapted to $F^Y$. (This is the innovations process).

**Proof:**
$$ I(X^T;Y^T) = E_{P_{X^T,Y^T}}\left[\log\frac{P_{Y^T|X^T}}{P_{Y^T}}\right] = E\left[\log\frac{P_{Y^T|X^T}}{P_{B^T}}\right] - E\left[\log\frac{P_{Y^T}}{P_{B^T}}\right] $$
Using Lemma 1 for the first term (with $f(t) = X_t$):
$$ E\left[\log\frac{P_{Y^T|X^T}}{P_{B^T}}\right] = E\left[\int_0^T X_t dY_t - \frac{1}{2}\int_0^T X_t^2 dt\right]. $$
Using Lemma 1 & 2 for the second term (with $f(t) = E[X_t|Y^t]$ and BM $\tilde{B}_t$):
$$ E\left[\log\frac{P_{Y^T}}{P_{B^T}}\right] = E\left[\int_0^T E[X_t|Y^t] dY_t - \frac{1}{2}\int_0^T E[X_t|Y^t]^2 dt\right]. $$
Combining them:
$$ I(X^T,Y^T) = E\left[\int_0^T (X_t-E[X_t|Y^t])dY_t - \frac{1}{2}\int_0^T (X_t^2 - E[X_t|Y^t]^2)dt\right] $$
Substituting $dY_t = X_t dt + dB_t$ and noting that the expectation of the stochastic integral is zero:
$$ = E\left[\int_0^T (X_t-E[X_t|Y^t])X_t dt - \frac{1}{2}\int_0^T (X_t^2 - E[X_t|Y^t]^2)dt\right] $$
$$ = \frac{1}{2} \int_0^T E[(X_t - E[X_t|Y^t])^2] dt. $$

---

### How is I-MMSE useful?
It allows us to prove sharp phase transitions. If we can show that for SNR $\nu < \nu^*$, the mutual information is small (e.g., $I(X;Y_\nu) \approx \frac{A\nu}{2}$), then we must have $\text{mmse}(\nu') \approx A$ for $\nu' < \nu$. This means the estimation error does not decrease significantly before the threshold $\nu^*$.

**Example: Sparse Mean Estimation**
Consider $Y \sim N(\theta, 1)$ with $\theta \sim (1-\rho)\delta_0 + \rho \delta_\mu$ where $\rho=o(1)$.
**Thm:** If $\mu^2 \le 2(1-\epsilon)\log\frac{1}{\rho}$, then
$$ \text{mmse}(\theta|Y) \ge (1-o(1))E[\theta^2] = (1-o(1))\rho\mu^2. $$
(In other words, the MMSE is essentially the prior variance; the observation Y provides no help).

**Pf sketch:**
1.  Let $X \sim (1-\rho)\delta_0 + \rho \delta_1$ and $\mu = \sqrt{\nu}$. Then $Y = \sqrt{\nu}X + N(0,1)$.
2.  Compute the mutual information. For $\nu < 2(1-\epsilon)\log\frac{1}{\rho}$, one can show $I(X;Y_\nu) \approx \frac{\rho(1-\rho)}{2}\nu$.
3.  Using the I-MMSE program, this implies that $\text{mmse}(X|Y_\nu) \approx \text{Var}(X) = \rho(1-\rho)$ for this range of $\nu$.
4.  Scaling back: $\text{mmse}(\theta|Y) = \nu \cdot \text{mmse}(X|Y_\nu) \ge (1-o(1))\rho\mu^2$.

---

### Tensorization of I-MMSE
**Thm:** If $Y_\nu = \sqrt{\nu}X + N(0, I_n)$, then
$$ \frac{d}{d\nu} I(X;Y_\nu) = \frac{1}{2} E\left[\|X-E[X|Y_\nu]\|_2^2\right] =: \frac{1}{2}\text{mmse}(X|Y_\nu). $$
**Pf sketch:** Consider each component having a different SNR, $Y_i = \sqrt{\nu_i}X_i + N(0,1)$.
$$ \frac{\partial}{\partial \nu_i} I(X^n; Y^n) = \frac{\partial}{\partial \nu_i} I(X_{\sim i}; Y^n) + \frac{\partial}{\partial \nu_i} I(X_i; Y^n|X_{\sim i}) $$
The first term is zero. The second term, by conditioning on $X_{\sim i}$, reduces to the 1D I-MMSE formula.
$$ \frac{\partial}{\partial \nu_i} I(X^n; Y^n) = \frac{1}{2}\text{mmse}(X_i|Y^n) $$
The full derivative is the sum of partials evaluated at $\nu_i = \nu$.
$$ \frac{d}{d\nu} I(X;Y_\nu) = \sum_{i=1}^n \frac{\partial}{\partial \nu_i} I(X;Y_\nu)|_{\nu_i=\nu} = \frac{1}{2}\sum_i \text{mmse}(X_i|Y_\nu) = \frac{1}{2}\text{mmse}(X|Y_\nu). $$

---

## Area Theorem
A related result using a similar tensorization idea.
Consider communication over a Binary Erasure Channel (BEC) with erasure probability $\epsilon$.
Let $X^n$ be a codeword from a codebook $C$ of size $M=e^{nR}$.

**Defn (EXIT function)**
$$ h_i(\epsilon) = H(X_i | Y_{\sim i}), \quad \text{and} \quad h(\epsilon) = \frac{1}{n}\sum_{i=1}^n h_i(\epsilon). $$
$h_i(\epsilon)$ is the uncertainty in bit $i$ given all other channel outputs.

**Lemma.** $H(X_i | Y^n) = \epsilon h_i(\epsilon)$.
**Pf.** $H(X_i|Y^n) = (1-\epsilon)H(X_i|Y_{\sim i}, Y_i \ne ?) + \epsilon H(X_i|Y_{\sim i}, Y_i=?) = 0 + \epsilon H(X_i|Y_{\sim i}) = \epsilon h_i(\epsilon)$.

**Lemma.** $\frac{d}{d\epsilon} H(X^n | Y(\epsilon)^n) = n h(\epsilon)$.
**Pf.** Use the same tensorization trick as before.

---

### Area Thm (BEC):
$$ \int_0^1 h(\epsilon) d\epsilon = R $$
**Pf.**
$$ \int_0^1 h(\epsilon) d\epsilon = \frac{1}{n} \int_0^1 \frac{d}{d\epsilon} H(X^n | Y(\epsilon)^n) d\epsilon = \frac{H(X^n|Y(1)^n) - H(X^n|Y(0)^n)}{n} $$
$$ = \frac{H(X^n) - 0}{n} = R. $$

What does the area theorem tell us? For a capacity-achieving code, the average bit error rate must approach 0 when $\epsilon < 1-R$. This means $h(\epsilon) \approx 0$ for $\epsilon < 1-R$. Since the total area under the curve from 0 to 1 must equal R, and $h(\epsilon) \le 1$, it must be that $h(\epsilon) \approx 1$ for $\epsilon > 1-R$.
Therefore, any capacity-achieving code must have a **sharp transition** for the decoding error.

### Special topic: any "symmetric" linear code achieves the capacity of BEC
A **linear code** C is a linear subspace of $\mathbb{F}_2^n$.
A code is **"symmetric"** if for any coordinates $i,j,k,l$, there is a permutation $\pi$ that maps $i \to j$, $k \to l$, and leaves the codebook invariant ($\pi C = C$).

**Thm.** For every symmetric linear code with rate $R$, it attains the BEC capacity under bit-MAP decoding ($\hat{x}_i = \text{argmax}_{x_i \in \{0,1\}} P(x_i|y^n)$).

---

### Proof Ingredient I: Boolean functions
Let $\Omega \subseteq \{0,1\}^n$. We call $\Omega$:
* **monotone**: if $x \in \Omega$ and $x \le x'$, then $x' \in \Omega$.
* **symmetric**: if for all $i,j \in [n]$, there is a permutation $\pi$ with $\pi(i)=j$ and $\pi\Omega=\Omega$.

Define $p_\epsilon(\Omega) = P(\text{Bern}(\epsilon)^{\otimes n} \in \Omega)$.
The function $\epsilon \mapsto p_\epsilon(\Omega)$ has a sharp threshold.

**Thm.** $\epsilon(1-\delta) - \epsilon(\delta) = o(1)$ for $\delta \in (0, 1/2)$, where $\epsilon(\delta) = \max\{\epsilon: p_\epsilon(\Omega) \le \delta\}$.

**Pf sketch.** A classical result shows $\frac{d}{d\epsilon} p_\epsilon(\Omega) = \sum_{i=1}^n I_i(\Omega)$, where $I_i$ are influence functions. By symmetry, this is $nI_1(\Omega)$. The KKL theorem shows that if $p_\epsilon(\Omega)$ is not close to 0 or 1, then the total influence $nI_1(\Omega)$ is large ($\Omega(\log n)$), which forces a sharp change.

---

### Proof Ingredient II: Area theorem
For a given linear code C, define $\Omega_i = \{$all erasure patterns in the other $n-1$ positions such that $X_i$ cannot be decoded$\}$.
1.  $\Omega_i$ is monotone (more erasures can't help).
2.  $\Omega_i$ is symmetric (by symmetry of C).
3.  $p_\epsilon(\Omega_i) = P(Y_{\sim i} \text{ fails to decode } X_i) = h_i(\epsilon)$.
4.  By symmetry of C, $h_i(\epsilon) \equiv h(\epsilon)$.

By the previous part, $\epsilon \mapsto h(\epsilon) = p_\epsilon(\Omega_i)$ has a sharp threshold.
In addition, $\int_0^1 h(\epsilon)d\epsilon = R$ by the area theorem.
The only way to satisfy both conditions is for the sharp threshold to be at $\epsilon^* = 1-R$. This means the code is capacity-achieving!