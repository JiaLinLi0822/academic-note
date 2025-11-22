## Statistical decision theory
**Statistical model:**
A family of distributions $(P_{\theta})_{\theta \in \Theta}$
- parametric: $\dim(\Theta) < \infty$
- nonparametric: $\dim(\Theta)=\infty$
- semiparametric: $\Theta = \Theta_{1} \times \Theta_{2}$ with $\dim(\Theta_{1}) < \infty, \dim(\Theta_{2})=\infty$

**Observation**: $X\sim P_{\theta}$ with an unknown $\theta \in \Theta$
**Decision rule/estimator**: a (possibly random) map $\hat{\theta}: \mathcal{X} \rightarrow \mathcal{A}$ (called "action space")
**Loss**: a given function $L: \Theta \times \mathcal{A} \rightarrow \mathbb{R}_{+}$
**Risk (expected loss)**: The risk of an estimator $\hat{\theta}$ under $L$ is
$r(\hat{\theta};\theta) = E_{X\sim P_{\theta}}[L(\theta, \hat{\theta}(x))]$
(usually abbreviated as $\mathbb{E}_{\theta}$)

Although originally proposed by Wald for statistical estimation, this framework is also general enough to encapsulate many other scenarios.

***

**Example (Density estimation)**
$x_{1}, \dots, x_{n}$ i.i.d. $\sim f$, so $\theta=f$. $P_{\theta}=f^{\otimes n}$
Different losses capture different goals, such as:
- Density at a point: $L(f,a)=|a-f(\cdot)|$
- Global estimation: $L_{2}(f,a)=\int|f(x)-a(x)|^{2}dx$
- Functional estimation: $L_{3}(f,a)=|a-\int h(f(x))dx|$

***

**Example (Linear regression)**
$X_1, \dots, X_n$ either fixed or random design
$P_{Y|X}$ satisfies $E[Y|X]=\langle\theta, X\rangle$
Losses include:
- Estimation error: $L(\theta, \hat{\theta})=||\hat{\theta}-\theta||^{2}$
- Prediction error: $L_{2}(\theta, \hat{\theta})=E_{x\sim P_{x}}[(\langle\theta, x\rangle - \langle\hat{\theta}, x\rangle)^{2}]$

***

**Example (Learning theory)**
$(X_{1},Y_{1}), \dots, (X_{n},Y_{n})\sim P_{XY}$
Loss to capture excess risk w.r.t. a given function class $\mathcal{F}$:
$L(P_{XY}, \hat{f}) = E_{P_{XY}}[(Y-\hat{f}(x))^{2}] - \inf_{f \in \mathcal{F}}E_{P_{XY}}[(Y-f(x))^{2}]$

***

**Example (Optimization)**
- **Parameter**: function $f$ to be minimized
- **Action**: a query strategy $x_{t+1}=\phi(x^{t}, y^{t})$
- **Observation**: queries $x^{T}$ and answers $y^{T}$ (e.g. $y_t=f(x_t)+w_t$)
- **Loss**: $L(f, x_{T+1}) = f(x_{T+1}) - \min f$

---
## Comparison of estimators
For an estimator $\hat{\theta}$, recall that its risk $r(\hat{\theta};\theta)$ is a function of $\theta$. How to compare two estimators $\hat{\theta}_1$ and $\hat{\theta}_2$?

**Option I**: $\hat{\theta}_2$ is inferior to $\hat{\theta}_1$ if $r(\hat{\theta}_2;\theta) \ge r(\hat{\theta}_1;\theta)$ for every $\theta \in \Theta$, and $r(\hat{\theta}_2;\theta) > r(\hat{\theta}_1;\theta)$ for some $\theta$.
- In this case, $\hat{\theta}_2$ is called **inadmissible**.
- However, admissibility is a weak notion: e.g. $\hat{\theta} \equiv \theta_0$ is admissible.

**Option II**: given a probability distribution $\pi(\theta)$ on $\Theta$ (called the **prior**), look at the weighted average risk:
$r_{\pi}(\hat{\theta}) = \int \pi(\theta)r(\hat{\theta};\theta)d\theta$
- The minimizer of $\hat{\theta} \mapsto r_{\pi}(\hat{\theta})$ is called the **Bayes estimator** under $\pi$.

**Option III**: look at the worst-case risk:
$r^{*}(\hat{\theta}) = \max_{\theta} r(\hat{\theta};\theta)$
- The minimizer of $\hat{\theta} \mapsto r^{*}(\hat{\theta})$ is called the **minimax estimator**.

***

Define:
- (Bayes risk) $r_{\pi}^{*} = \inf_{\hat{\theta}} r_{\pi}(\hat{\theta}) = \inf_{\hat{\theta}} E_{\theta \sim \pi}[r(\hat{\theta};\theta)]$
- (minimax risk) $r^{*} = \inf_{\hat{\theta}} r^{*}(\hat{\theta}) = \inf_{\hat{\theta}} \sup_{\theta} r(\hat{\theta};\theta)$

**Theorem**:
$r^{*} \ge r_{\pi}^{*}$ for all $\pi$.
$r^{*} = \sup_{\pi} r_{\pi}^{*}$ under regularity conditions (minimax theorem, and the maximizer $\pi^{*}$ is called the least favorable prior).

**Proof**: $\sup_{\theta} r(\hat{\theta};\theta) \ge \int \pi(\theta) r(\hat{\theta};\theta) d\theta$. $(\max \ge \text{average}) \Rightarrow r^{*} \ge r_{\pi}^{*}$

For the other direction, recall that a randomized estimator is a probability distribution $p(\cdot|x)$ over actions. We have
$\sup_{\pi} r_{\pi}^{*} = \sup_{\pi} \inf_{p} E_{\theta \sim \pi} E_{x} E_{a \sim p(\cdot|x)} L(\theta, a)$ (affine in both $\pi$ and $p$)
$= \inf_{p} \sup_{\pi} E_{\theta \sim \pi} E_{x} E_{a \sim p(\cdot|x)} L(\theta, a)$ (by Sion's minimax theorem)
$= \inf_{p} \sup_{\theta} E_{x} E_{a \sim p(\cdot|x)} L(\theta, a) = r^{*}$

- **Finding the Bayes estimator** is statistically easy: the prior $\pi(\theta)$ induces a joint distribution $\pi(\theta)P_{\theta}(x)$ on $(\Theta, X)$, which therefore admits the posterior $\pi(\theta|x) \propto \pi(\theta)p_{\theta}(x)$. Then the Bayes estimator is the barycenter of $\pi(\theta|x)$ under $L$, i.e., $\hat{\theta}_{\pi}(x) = \arg\min_{a} E_{\theta \sim \pi(\cdot|x)}[L(\theta, a)]$. However, it can be computationally hard.

- **Finding the minimax estimator** can be statistically hard, and is only feasible for a few examples. Therefore, one is often interested in asymptotically minimax estimators or rate-optimal results, i.e., find $\hat{\theta}$ s.t. $r^{*}(\hat{\theta}) \le Cr^{*}$ for some constant $C$.

---
### Examples

**Binomial**: Let $X\sim B(n, \theta)$ and $L(\theta, a) = (\theta-a)^2$.
To find the least favorable prior, try $\pi(\theta) \propto \theta^{b-1}(1-\theta)^{b-1}$ (Beta(b,b)).
Then the posterior is $\pi(\theta|X) \propto \pi(\theta)\cdot\theta^{X}(1-\theta)^{n-X} = \theta^{b+X-1}(1-\theta)^{b+n-X-1}$ (Beta($b+X, b+n-X$)).
The Bayes estimator is $\hat{\theta}(x) = E_{\pi}[\theta|x] = \frac{X+b}{n+2b}$.
The risk function of $\hat{\theta}$ is
$r(\hat{\theta};\theta) = E_{\theta}(\hat{\theta}-\theta)^{2} = \text{Bias}^2 + \text{Var}$
$= (\frac{n\theta+b}{n+2b}-\theta)^{2} + \frac{n\theta(1-\theta)}{(n+2b)^{2}}$
$= \frac{1}{(n+2b)^{2}}[b^{2}(1-4\theta(1-\theta)) + n\theta(1-\theta)]$
By choosing $b = \frac{\sqrt{n}}{2}$, we have $r(\hat{\theta};\theta) \equiv \frac{1}{4(\sqrt{n}+1)^{2}}$.
Therefore, $\hat{\theta} = \frac{X+\sqrt{n}/2}{n+\sqrt{n}}$ attains the worst-case risk $r^{*}(\hat{\theta}) = \frac{1}{4(\sqrt{n}+1)^{2}}$, and $r^{*} \le r^{*}(\hat{\theta}) = r_{\pi}(\hat{\theta}) = r_{\pi}^{*} \le r^{*} \Rightarrow r^{*} = \frac{1}{4(\sqrt{n}+1)^{2}}$.

**GLM**: Let $X\sim N(\theta, I_d)$ and $L(\theta, a) = \rho(\theta-a)$ where $\rho: \mathbb{R}^d \rightarrow \mathbb{R}_{+}$ is a continuous and bowl-shaped loss (i.e., $\rho(x) = \rho(-x)$ and $\rho$ is quasi-convex).
**Claim**: $\hat{\theta}=X$ is the minimax estimator, with risk $r^{*} = E[\rho(Z)]$, $Z \sim N(0, I_d)$.
**Proof**: Try prior $\pi = N(0, \tau^2 I_d)$, then posterior $\pi(\theta|X) \propto \exp(-\frac{||\theta||^2}{2\tau^2} - \frac{||X-\theta||^2}{2})$ is $N(\frac{\tau^2 X}{1+\tau^2}, \frac{\tau^2 I_d}{1+\tau^2})$.
So $r^{*} \ge r_{\pi}^{*} = E_X[\min_{a\in\mathbb{R}^d} E_{\theta \sim N(\frac{\tau^2 X}{1+\tau^2}, \frac{\tau^2 I_d}{1+\tau^2})} \rho(\theta-a)]$.
$= E_X[\rho(\sqrt{\frac{\tau^2}{1+\tau^2}}Z)]$ (by Anderson's lemma below).
Let $\tau \rightarrow \infty$ gives $r^{*} \ge E[\rho(Z)]$.

***

**Lemma (Anderson)**
If $X \sim N(0, \Sigma)$ and $\rho$ is bowl-shaped, then $\min_{a \in \mathbb{R}^d} E[\rho(X+a)] = E[\rho(X)]$.
**Proof**: Let $K_c = \{x: \rho(x) \le c\}$. Since $\rho$ is bowl-shaped, $K_c$ is convex, and $K_c = -K_c$.
Then $E[\rho(X+a)] = \int_0^\infty P(\rho(X+a) > c) dc = \int_0^\infty (1 - P(X+a \in K_c)) dc$
$\ge \int_0^\infty (1 - P(X \in K_c)) dc$ (see below)
$= E[\rho(X)]$
where $P(X \in K_c) = P(X \in \frac{1}{2}(K_c+a) + \frac{1}{2}(K_c-a))$ ($\supseteq \frac{1}{2}(K_c+a) + \frac{1}{2}(K_c-a)$ by convexity)
$\ge \sqrt{P(X \in K_c+a)P(X \in K_c-a)}$ ($X$ has a log-concave distribution)
$= \sqrt{P(X \in K_c+a)P(X \in -K_c-a)}$ ($K_c = -K_c$)
$= P(X \in K_c+a)$ (distribution of $X$ is symmetric around 0).

---
## Hájek-Le Cam classical asymptotics: $X_1, \dots, X_n \sim P_{\theta}$ with $n \rightarrow \infty$

**Regular models: differentiable in quadratic mean (QMD)**

**Definition (QMD)**: A statistical model $(P_{\theta})_{\theta \in \Theta}$ is called QMD at $\theta$ if there exists a score function $s_{\theta}(x)$ s.t.
$\int [\sqrt{p_{\theta+h}} - \sqrt{p_{\theta}} - \frac{1}{2}h \cdot s_{\theta}\sqrt{p_{\theta}}]^2 d\mu = o(||h||^2)$,
where $\mu$ is any dominating measure for $(P_{\theta})$, and $p_{\theta} = \frac{dP_{\theta}}{d\mu}$.

**Note**:
1. When $h \mapsto \sqrt{p_{\theta+h}(x)}$ is differentiable everywhere, then $s_{\theta}(x) := \frac{2}{\sqrt{p_{\theta}(x)}} \frac{\partial}{\partial\theta}\sqrt{p_{\theta}(x)} = \frac{\frac{\partial}{\partial\theta}p_{\theta}(x)}{p_{\theta}(x)} = \frac{\partial}{\partial\theta}\log p_{\theta}(x)$.
2. Since $\int[\sqrt{p_{\theta+h}}-\sqrt{p_{\theta}}]^2 d\mu = H^2(P_{\theta+h}, P_{\theta}) \le 2$, QMD implies that the **Fisher information** $I(\theta) := E_{\theta}[s_{\theta}s_{\theta}^T]$ exists.

***

### History of asymptotic theorems: Fisher's program
1. The MLE $\hat{\theta}_n$ satisfies $\sqrt{n}(\hat{\theta}_n - \theta) \xrightarrow{d} N(0, I(\theta)^{-1})$, where $I(\theta)$ is the Fisher information matrix of $(P_\theta)$.
2. For any other sequence of estimators $T_n$ with $\sqrt{n}(T_n - \theta) \xrightarrow{d} N(0, \Sigma_\theta)$, $\forall\theta\in\Theta$, then $\Sigma_{\theta} \ge I(\theta)^{-1}$. (In other words, the MLE attains the asymptotically smallest variance).

While (1) is true under mild regularity conditions, (2) is unfortunately not true as witnessed by **Hodges' estimator (1951)**.
Let $X_1, \dots, X_n \sim N(\theta, 1)$.
$\hat{\theta}_n = \begin{cases} \bar{X}_n & \text{if } |\bar{X}_n| \ge n^{-1/4} \\ 0 & \text{if } |\bar{X}_n| < n^{-1/4} \end{cases}$
It's easy to show that $\sqrt{n}(\hat{\theta}_n - \theta) \xrightarrow{d} \begin{cases} N(0, 1) & \text{if } \theta \ne 0 \\ 0 & \text{if } \theta = 0 \end{cases}$
So (2) in Fisher's program doesn't hold when $\theta=0$.

Hodges' example shows that cautions need to be taken when defining the "optimality" of the MLE or inverse Fisher information. It then took statisticians ~20 years to find the right definitions, through the following angles:
1. Hodges' estimator is not "regular" (restricting the class of estimators).
2. The set of violations has Lebesgue measure 0 ("superefficiency" occurs rarely).
3. The performance of Hodges' estimator is bad when $\theta \approx n^{-1/4}$ (a large asymptotic local risk).

---
## A collection of asymptotic theorems
**Convolution Theorem**: Let $(P_{\theta})$ be QMD. If $\sqrt{n}(T_n - \psi(\theta)) \xrightarrow{d} L_{\theta}$ under $P_{\theta}^{\otimes n}$ and its is regular in the sense that $\sqrt{n}(T_n - \psi(\theta+\frac{h}{\sqrt{n}})) \xrightarrow{d} L_{\theta}$ under $P_{\theta+\frac{h}{\sqrt{n}}}^{\otimes n}$ $\forall h\in\mathbb{R}^d$. Then there exists a probability measure $M_{\theta}$ s.t.
$L_{\theta} = N(0, \nabla\psi(\theta)^T I(\theta)^{-1} \nabla\psi(\theta)) * M_{\theta}, \forall\theta$
where $*$ denotes the convolution. (Convolution makes the distribution more noisy).

**Almost everywhere convolution theorem**: Under all above conditions except for the regularity of $T_n$, then $L_{\theta} = N(0, \nabla\psi(\theta)^T I(\theta)^{-1} \nabla\psi(\theta)) * M_{\theta}$ for Lebesgue almost every $\theta$.

**Local asymptotic minimax (LAM) theorem**: For every continuous and bowl-shaped loss $\rho$, and any sequence of estimators $\{T_n\}$
$\lim_{c\rightarrow\infty} \lim_{n\rightarrow\infty} \sup_{||h||\le c} E_{\theta+\frac{h}{\sqrt{n}}}[\rho(\sqrt{n}(T_n - \psi(\theta+\frac{h}{\sqrt{n}})))] \ge E[\rho(Z)]$,
with $Z \sim N(0, \nabla\psi(\theta)^T I(\theta)^{-1} \nabla\psi(\theta))$.
(This is a lower bound on the minimax risk of the local family $(P_{\theta+\frac{h}{\sqrt{n}}})_{||h||\le c}$ under the loss $L(\theta, a) = \rho(\sqrt{n}(a-\psi(\theta)))$.)

The proofs rely on the asymptotic equivalence between models $(P_{\theta+\frac{h}{\sqrt{n}}}^{\otimes n})_{||h||\le c}$ and the GLM $(N(h, I(\theta)^{-1}))_{||h||\le c}$.

---
## A special case of LAM via Bayesian Cramer-Rao
**Bayesian CR in 1D (van-Trees inequality)**
Let $\theta \in [a,b]$, and $\pi(\cdot)$ be a differentiable prior density on $[a,b]$ with $\pi(a)=\pi(b)=0$ and $J(\pi) = \int_a^b \frac{\pi'(\theta)^2}{\pi(\theta)}d\theta < \infty$. Then for any estimator $\hat{\theta}$,
$E_{\pi}E_{\theta}[(\hat{\theta}-\theta)^2] \ge \frac{1}{E_{\pi}[I(\theta)] + J(\pi)}$.
(Compare with the usual CR $E_{\theta}[(\hat{\theta}-\theta)^2] \ge \frac{1}{I(\theta)}$ for unbiased estimators).

**Proof**: $E_{\pi}E_{\theta}[(\hat{\theta}-\theta)\partial_{\theta}(\log \pi(\theta)p_{\theta}(x))]$
$= -\int_{\mathcal{X}} \int_a^b (\hat{\theta}-\theta) \partial_{\theta}(\pi(\theta)p_{\theta}(x)) d\theta d\mu(x)$
$= \int_{\mathcal{X}} \int_a^b \pi(\theta)p_{\theta}(x) d\theta d\mu(x)$ (integration by parts)
$= 1$.
Then BCR follows from Cauchy-Schwarz and
$E_{\pi}E_{\theta}[\partial_{\theta}(\log \pi(\theta)p_{\theta}(x))^2] = E_{\pi}[(\frac{\pi'(\theta)}{\pi(\theta)})^2] + E_{\pi}E_{\theta}[(\frac{p_{\theta}'(x)}{p_{\theta}(x)})^2] + 2E_{\pi}E_{\theta}[\frac{\pi'(\theta)}{\pi(\theta)}\frac{\partial_{\theta}p_{\theta}(x)}{p_{\theta}(x)}]$
The cross term is 0 assuming $\int \partial_{\theta} p_{\theta}(x) d\mu(x) = \partial_{\theta} \int p_{\theta}(x) d\mu(x) = 0$.
So the expression equals $J(\pi) + E_{\pi}[I(\theta)]$.

**Multivariate BCR**: Let $\pi = \prod_{i=1}^d \pi_i$ be a differentiable prior density on $\prod_{i=1}^d [a_i, b_i]$ vanishing on the boundary, and $J(\pi) = \text{diag}(J(\pi_1), \dots, J(\pi_d))$. Then for any $\hat{\theta}$,
$E_{\pi}E_{\theta}[||\hat{\theta}-\theta||^2] \ge Tr[(E_{\pi}[I(\theta)] + J(\pi))^{-1}]$.

**Proof**: Similar to the 1D proof. Can show $\forall k=1, \dots, d$,
$E_{\pi}E_{\theta}[(\hat{\theta}_k - \theta_k)\partial_{\theta_k} \log(\pi(\theta)p_{\theta}(x))] = e_k$ (k-th basis vector).
Let $\Sigma = E[\nabla_{\theta}\log(\pi(\theta)p_{\theta}(x))\nabla_{\theta}\log(\pi(\theta)p_{\theta}(x))^T] = E_{\pi}[I(\theta)]+J(\pi)$. By Cauchy-Schwarz we have
$E_{\pi}E_{\theta}[(\hat{\theta}_k - \theta_k)^2] \ge \sup_{u\ne 0} \frac{\langle u, e_k \rangle^2}{u^T \Sigma u} = (\Sigma^{-1})_{kk}$.

### Deriving LAM from BCR
When $\psi(\theta)=\theta, \rho(x)=||x||^2$.
First, note that if $\pi(\theta) = \frac{2}{b-a}\cos^2(\frac{\pi}{2}\frac{2\theta-(a+b)}{b-a})$, then $\pi(a)=\pi(b)=0$, and $J(\pi) = \frac{4\pi^2}{(b-a)^2}$. (This choice of $\pi$ minimizes $J(\pi)$).
Next, choosing the above $\pi_i$ on $[\theta_i - \frac{c}{\sqrt{n}}, \theta_i + \frac{c}{\sqrt{n}}]$, BCR gives
$\inf_{\hat{\theta}} \sup_{||h||\le c} E_{\theta+\frac{h}{\sqrt{n}}}[||\hat{\theta} - (\theta+\frac{h}{\sqrt{n}})||^2]$
$\ge \inf_{\hat{\theta}} E_{\pi}E_{\theta+\frac{h}{\sqrt{n}}}[||\hat{\theta} - (\theta+\frac{h}{\sqrt{n}})||^2]$
$\ge Tr[(n E_{\pi}[I(\theta)] + \frac{n\pi^2}{c^2}I)^{-1}]$ (Fisher info for $X_1, \dots, X_n \sim P_{\theta}$ is $nI(\theta)$)
$= \frac{1+o(1)}{n} Tr(I(\theta)^{-1})$ as $n\rightarrow\infty$ and $c\rightarrow\infty$, assuming that $\theta \mapsto I(\theta)$ is continuous at $\theta$.

---
## Application of LAM
Since the global minimax risk is always lower bounded by the local minimax risk, LAM gives asymptotic lower bounds.

**Example (Binomial)**: Revisit $X\sim B(n, \theta)$. Then
$r_n^{*} = \inf_{\hat{\theta}} \sup_{\theta \in [0,1]} E_{\theta}[(\hat{\theta}-\theta)^2]$
$\ge \inf_{\hat{\theta}} \sup_{|h| \le c_n} E_{\frac{1}{2}+\frac{h}{\sqrt{n}}} [(\hat{\theta} - (\frac{1}{2}+\frac{h}{\sqrt{n}}))^2]$
$\ge \frac{1-o_n(1)}{n I(\frac{1}{2})} = \frac{1-o_n(1)}{4n}$.
This is consistent with the exact expression of $r_n^{*} = \frac{1}{4(\sqrt{n}+1)^2}$.

**Example (nonparametric entropy estimation)**: Let $x_1, \dots, x_n \sim f$, a density on $[0,1]$.
The target is to estimate the differential entropy $h(f) = \int_0^1 -f(x)\log f(x) dx$ under the squared loss.
**Challenge**: This is not a finite-dimensional model, so LAM doesn't directly apply.
**Solution**: Consider a one-parameter subfamily $(f_0+tg)_{|t|\le \epsilon}$. Then
$\frac{d}{dt}h(f_0+tg)|_{t=0} = -\int_0^1 (1+\log f_0(x))g(x)dx$.
$I(0) = \int_0^1 \frac{g(x)^2}{f_0(x)}dx$.
LAM applied to this subfamily at $t=0$ gives
$r_n^{*} \ge \frac{1-o_n(1)}{n}(\int_0^1 \frac{g(x)^2}{f_0(x)} dx)^{-1}(-\int_0^1 (1+\log f_0(x))g(x)dx)^2 =: \frac{1-o_n(1)}{n}V(f_0,g)$.
We can maximize this lower bound w.r.t. $g$. Since $\int g=0$ (as $f_0+tg$ is a density), Cauchy-Schwarz gives
$V(f_0,g) = (\int_0^1 \frac{g(x)^2}{f_0(x)}dx)^{-1}(-\int_0^1 (\log f_0(x) - h(f_0))g(x)dx)^2$
$\le \int_0^1 f_0(x)(\log f_0(x)-h(f_0))^2 dx = \int_0^1 f_0(x)\log^2 f_0(x)dx - h(f_0)^2$,
where equality holds when $g(x) = f_0(x)(\log f_0(x)-h(f_0))$.
Therefore, $r_n^{*} \ge \frac{1-o_n(1)}{n} \sup_{f_0} (\int_0^1 f_0(x)\log^2 f_0(x)dx - h(f_0)^2)$.

***

### Pros and cons for asymptotic theorems
- **Pro 1**: plug-and-play bound for essentially all statistical models.
- **Pro 2**: exact constant for the asymptotic risk.
- **Con 1**: bounds are asymptotic, assuming $n \rightarrow \infty$ while $d$ fixed.
- **Con 2**: bounds are for asymptotic variance, while for high-dimensional scenarios bias can be the dominating factor.
This is the reason to study techniques for non-asymptotic lower bounds.

---
## Special topic: Le Cam's distance between statistical models
For two models $(P_{\theta})_{\theta \in \Theta}$ and $(Q_{\theta})_{\theta \in \Theta}$ with the same parameter set $\Theta$, how to compare the strengths between them? (Throughout let's assume that $\Theta$ is a finite set).

**Definition (deficiency)**: A model $\mathcal{M}=(P_{\theta})_{\theta \in \Theta}$ is called $\epsilon$-deficient w.r.t. $\mathcal{N}=(Q_{\theta})_{\theta \in \Theta}$ if for every finite decision space $A$, bounded loss $L(\theta,a)\in[0,1]$, and (randomized) estimator $\hat{\theta}_{\mathcal{N}}$ under $\mathcal{N}$, there exists an estimator $\hat{\theta}_{\mathcal{M}}$ under $\mathcal{M}$ s.t.
$r(\hat{\theta}_{\mathcal{M}};\theta) \le r(\hat{\theta}_{\mathcal{N}};\theta) + \epsilon, \forall \theta \in \Theta$.

**Theorem (Randomization criterion)**: The following are equivalent:
1. $\mathcal{M}$ is $\epsilon$-deficient w.r.t $\mathcal{N}$;
2. for every finite action set $A$, bounded loss $L(\theta,a)\in[0,1]$, and prior $\pi$ on $\Theta$, the Bayes risks satisfy $r_{\pi}^{*}(\mathcal{M}) \le r_{\pi}^{*}(\mathcal{N}) + \epsilon$;
3. there exists a kernel $K$ from $\mathcal{X} \to \mathcal{Y}$ s.t. $TV(KP_{\theta}, Q_{\theta}) \le \epsilon, \forall \theta \in \Theta$. ($KP_{\theta}(y) = \sum_x P_{\theta}(x)K(y|x)$)

**Proof sketch**:
- (3) $\Rightarrow$ (1): upon observing $X$ under $\mathcal{M}$, apply the kernel $K$ to simulate $Y$ and apply the estimator $\hat{\theta}_{\mathcal{N}}(y)$.
- (2) $\Rightarrow$ (3): let $A=\Theta$ and $\hat{\theta}_{\mathcal{N}}(y)=y$:
$\inf_K \sup_{L, \pi} E_{\theta \sim \pi}[E_{x \sim P_{\theta}}E_{a \sim K(\cdot|x)} L(\theta,a) - E_{a \sim Q_{\theta}} L(\theta,a)] \le \epsilon$.
This objective is linear in $K(\cdot|x)$ and $(\pi, L)$, by minimax theorem,
$\inf_K \sup_{\theta \in \Theta} TV(KP_{\theta}, Q_{\theta}) \le \epsilon$.

**Definition (Le Cam's distance)**: For finite models $\mathcal{M}=(P_{\theta})_{\theta\in\Theta}$ and $\mathcal{N}=(Q_{\theta})_{\theta\in\Theta}$ define Le Cam's distance as
$\Delta(\mathcal{M}, \mathcal{N}) = \min\{\epsilon: \mathcal{M} \text{ is } \epsilon\text{-deficient to } \mathcal{N}, \mathcal{N} \text{ is } \epsilon\text{-deficient to } \mathcal{M}\}$.

**Example (sufficiency)**: For $\mathcal{M}=(P_{\theta})_{\theta\in\Theta}$ and a function $T=T(x)$, define the T-induced model $\mathcal{N}=(T_{\#}P_{\theta})_{\theta\in\Theta}$. By randomization criterion, $\Delta(\mathcal{M}, \mathcal{N})=0$ iff $\mathcal{M}$ and $\mathcal{N}$ are mutual randomizations, iff both $\theta-X-T$ and $\theta-T-X$ are Markov chains, iff $T$ is a sufficient statistic for $X$. (Factorization Thm: T is sufficient $\iff P_{\theta}(x)=g(x)h(\theta, T)$ for some $g, h$).

***

For a sequence of models $(\mathcal{M}_n)_{n \ge 1}$ and $(\mathcal{N}_n)_{n \ge 1}$, how to show asymptotic equivalence $\Delta(\mathcal{M}_n, \mathcal{N}_n) \rightarrow 0$?

**Definition (standard model)**: Let $\mathcal{M}=\{P_1, \dots, P_m\}$ be a finite model, and $\bar{P} := \frac{1}{m}\sum_{i=1}^m P_i$. Then $T(x) = (\frac{P_1}{\bar{P}}(x), \dots, \frac{P_m}{\bar{P}}(x))$ is sufficient and lies on $\Delta_m := \{u \in \mathbb{R}_+^m; 1^T u=m\}$.
$\mathcal{M}$ is equivalent to the T-induced model $\mathcal{N}=\{\mu_1, \dots, \mu_m\}$ with $\frac{d\mu_i}{d\mu}(T)=T_i$ where $\mu$ is the distribution of $T$ under $\bar{P}$, known as the standard distribution.
**Implication**: standard model unifies all statistical models of size $m$ to standard distributions $\mu$ on $\Delta_m$.

**Theorem**: If $\mu_n \xrightarrow{d} \mu$, then $\Delta(\mathcal{M}_n, \mathcal{M}) \rightarrow 0$.
**Proof**: By (2) in the randomization criterion, it suffices to check $\sup_{A, \pi, L} |r_{\pi}^{*}(\mu_n) - r_{\pi}^{*}(\mu)| \xrightarrow{n\rightarrow\infty} 0$.
In a standard model, $r_{\pi}^{*}(\mu) = \inf_{\hat{\theta}} \sum_{i=1}^m \pi_i E_{\mu_i}[L(i, \hat{\theta}(T))] = E_{\mu}[\inf_{c \in C} \langle c, T \rangle]$, where $C := \text{conv}(\{(\pi_i L(i, a))_{i=1}^m\}_{a \in A})$.
Since $f(T)=\inf_{c\in C}\langle c,T\rangle$ is bounded by $m$ and 1-Lip,
$\sup_{A, \pi, L} |r_{\pi}^{*}(\mu_n) - r_{\pi}^{*}(\mu)| \le \sup_{||f||_\infty \le m, ||f||_{Lip} \le 1} |E_{\mu_n}f - E_{\mu}f| \rightarrow 0$.

***

**Theorem**: Let $\mathcal{M}_n = \{P_{1,n}, \dots, P_{m,n}\}$ and $\mathcal{M}=\{P_1, \dots, P_m\}$.
Let $L_n = (\frac{P_{2,n}}{P_{1,n}}, \dots, \frac{P_{m,n}}{P_{1,n}})$ and $L = (\frac{P_2}{P_1}, \dots, \frac{P_m}{P_1})$.
Suppose $\mathcal{M}$ is homogeneous (i.e., $P_i$ and $P_j$ are mutually absolutely continuous). Then
$\text{Law}(L_n | P_{1,n}) \xrightarrow{d} \text{Law}(L | P_1) \Rightarrow \Delta(\mathcal{M}_n, \mathcal{M}) \rightarrow 0$.
(In other words, weak convergence of likelihood ratios implies asymptotic equivalence).

**Proof**: Suffice to show that the standard distributions $\mu_n \xrightarrow{d} \mu$.
By compactness of $\Delta_m$ and Prokhorov's Thm, it suffices to show that if $\mu_{n_k} \xrightarrow{d} \nu$ along some subsequence, then $\nu=\mu$.
For $s=(s_2, \dots, s_m)$ with $s_i > 0$ and $\sum_{i=2}^m s_i < 1$, then $f_s(L) = \prod_{i=2}^m L_i^{s_i}$ is a continuous function of L. The sequence of RVs $f_s(L_n)$ is uniformly integrable. Therefore, by weak convergence, $E_{\mu}[T_1^{s_1}\dots T_m^{s_m}] = E_{P_1}[f_s(L)] = \lim_{n\rightarrow\infty} E_{P_{1,n}}[f_s(L_n)]$.
On the other hand, as $\mu_{n_k} \rightarrow \nu$, $E_{P_{1,n_k}}[f_s(L_n)] = E_{\mu_{n_k}}[T_1^{s_1}\dots T_m^{s_m}] \rightarrow E_{\nu}[T_1^{s_1}\dots T_m^{s_m}]$.
So $E_{\mu}[T_1^{s_1}\dots T_m^{s_m}] = E_{\nu}[T_1^{s_1}\dots T_m^{s_m}]$ for all valid $s_i$.
By uniqueness for MGFs, this implies $\mu = \nu$.

***

Finally, we show that if $(P_{\theta})$ is QMD then for any finite set $\mathcal{I}$,
$\mathcal{M}_n := \{P_{\theta_0+\frac{h}{\sqrt{n}}}^{\otimes n}\}_{h \in \mathcal{I}}$ is asymptotically equivalent to $\mathcal{M}=\{N(h, I(\theta_0)^{-1})\}_{h \in \mathcal{I}}$.
This is called **local asymptotic normality (LAN)**.

**Proof sketch**: Check the likelihood ratio.
In the limiting Gaussian model, $\log \frac{dN(h,I(\theta_0)^{-1})}{dN(0,I(\theta_0)^{-1})}(Z) = h^T I(\theta_0)Z - \frac{1}{2}h^T I(\theta_0)h$ with $I(\theta_0)Z \sim N(0, I(\theta_0))$.

For the product model, let $W_{ni} = 2(\sqrt{\frac{p_{\theta_0+h/\sqrt{n}}}{p_{\theta_0}}}(X_i) - 1)$.
$\log\frac{P_{\theta_0+h/\sqrt{n}}^{\otimes n}}{P_{\theta_0}^{\otimes n}}(X^n) = -2\sum_{i=1}^n \log(1+\frac{1}{2}W_{ni}) = \sum_{i=1}^n W_{ni} - \frac{1}{4}\sum_{i=1}^n W_{ni}^2 + \sum_{i=1}^n o(W_{ni}^2)$.
By QMD, $E_{P_{\theta_0}}[(W_{ni} - \frac{1}{\sqrt{n}}h^T s_{\theta_0}(X_i))^2] = o(\frac{1}{n})$, thus $\text{Var}_{P_{\theta_0}^{\otimes n}}(\sum_{i=1}^n W_{ni} - \frac{1}{\sqrt{n}}\sum_{i=1}^n h^T s_{\theta_0}(X_i)) = o(1)$.
And $E\sum_{i=1}^n W_{ni} = -n\int(\sqrt{p_{\theta_0+h/\sqrt{n}}}-\sqrt{p_{\theta_0}})^2 d\mu \rightarrow -\frac{1}{4}h^T E[s_{\theta_0}s_{\theta_0}^T]h = -\frac{1}{4}h^T I(\theta_0)h$.
Moreover, $\sum_{i=1}^n W_{ni}^2 \rightarrow h^T I(\theta_0)h$ by LLN.
Therefore, $\log \frac{P_{\theta_0+h/\sqrt{n}}^{\otimes n}}{P_{\theta_0}^{\otimes n}}(X^n) = h^T(\frac{1}{\sqrt{n}}\sum_{i=1}^n s_{\theta_0}(X_i)) - \frac{1}{2}h^T I(\theta_0)h + o_p(1)$, where the first term converges to $N(0, h^T I(\theta_0)h)$ by CLT.

Combining Anderson's lemma and the limiting Gaussian model above... we arrive at the local asymptotic minimax theorem.