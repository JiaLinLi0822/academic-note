---

---
### 1. Definition (f-divergence, Csiszár '63)

> [!Note] Definition
> Let $f: (0, \infty) \rightarrow \mathbb{R}$ be a convex function with $f(1)=0$. The **f-divergence** between two probability distributions P and Q on the same space is defined as:
> $$
> D_{f}(P \| Q) := E_{Q}\left[f\left(\frac{dP}{dQ}\right)\right] \text{}
> $$

**Remarks:**
1.  Some definitions additionally assume that $f'(1)=0$, which is without loss of generality (WLOG) because $f(x)$ and $f(x) + c(x-1)$ produce the same f-divergence.
2.  If P is not absolutely continuous with respect to Q ($P \not\ll Q$), we define $f(0) := \lim_{x\to0^+} f(x)$. The divergence is then defined as $D_{f}(P\|Q) = \int_{q>0} q f(\frac{p}{q})d\mu + f'(\infty)P(q=0)$, where $f'(\infty) := \lim_{x\to\infty} \frac{f(x)}{x}$.

**Examples:**
1.  **Total Variation (TV) Distance**: For $f(x) = \frac{1}{2}|x-1|$, the f-divergence is $D_{f}(P\|Q) = TV(P,Q) = \frac{1}{2}\int|dP-dQ|$.
2.  **Squared Hellinger Distance**: For $f(x) = (\sqrt{x}-1)^2$, the f-divergence is $D_{f}(P\|Q) = H^2(P,Q) = \int(\sqrt{dP}-\sqrt{dQ})^2$.
3.  **Kullback-Leibler (KL) Divergence**: For $f(x) = x \log x$, the f-divergence is $D_{f}(P\|Q) = D_{KL}(P\|Q) = \int dP \log\frac{dP}{dQ}$.
4.  **$\chi^2$-divergence**: For $f(x) = (x-1)^2$, the f-divergence is $D_{f}(P\|Q) = \chi^2(P\|Q) = \int\frac{(dP-dQ)^2}{dQ}$.
5.  **Le Cam Distance**: This distance is an f-divergence.
6.  **Jensen-Shannon (JS) Divergence**: For $f(x) = x \log x - (x+1)\log\frac{x+1}{2}$, the f-divergence is $D_{f}(P\|Q) = JS(P,Q) = D_{KL}(P\|\frac{P+Q}{2}) + D_{KL}(Q\|\frac{P+Q}{2})$.

**Basic Properties:**
* **Non-negativity**: $D_{f}(P\|Q) \ge 0$.
    * *Proof*: By Jensen's inequality, $D_{f}(P\|Q) = E_{Q}[f(\frac{dP}{dQ})] \ge f(E_{Q}[\frac{dP}{dQ}]) = f(1) = 0$.
* **Joint Convexity**: The mapping $(P,Q) \mapsto D_{f}(P\|Q)$ is jointly convex.
    * *Proof*: For a convex function $f$, its perspective transform $(x,y) \mapsto y f(\frac{x}{y})$ is also convex.
* **Data Processing Inequality**: If $Y$ is a function of $X$ (i.e., $P_X \to P_Y$), then $D_{f}(P_X\|Q_X) \ge D_{f}(P_Y\|Q_Y)$.
    * *Proof*: This follows from joint convexity, similar to the proof for KL divergence.

---

### 2. Why f-divergence? Binary hypothesis testing
Consider a simple hypothesis testing problem:
* **Null Hypothesis** $H_0: X \sim P$
* **Alternative Hypothesis** $H_1: X \sim Q$

A test is a function $T: \mathcal{X} \rightarrow \{0,1\}$. The errors are:
* **Type I error**: $P(T(X)=1)$
* **Type II error**: $Q(T(X)=0)$

> [!TIP] Theorem
> The minimum total error is related to the **Total Variation distance**:
> $$
> \inf_T (P(T(X)=1) + Q(T(X)=0)) = 1 - TV(P,Q) \text{}
> $$

**Remark:**
* $TV(P,Q) = 0$: P and Q are totally indistinguishable.
* $TV(P,Q) = 1$: P and Q are perfectly distinguishable ($P \perp Q$).
* $TV(P,Q) < 1$: P and **Q** are partially indistinguishable.
* TV is an important quantity for establishing minimax lower bounds.

### Why not just TV?
1.  It can be hard to compute.
2.  TV does not tensorize well. For product distributions, $TV(P^{\otimes n}, Q^{\otimes n}) \le n TV(P,Q)$ is the best possible general inequality, but it is often loose.
    * **Example**: For $P = \text{Bern}(\frac{1}{2})$ and $Q = \text{Bern}(\frac{1}{2}+\delta)$, the aforementioned inequality gives an upper bound of $n\delta$. Using Pinsker's inequality provides a tighter bound: $TV(P^{\otimes n}, Q^{\otimes n}) \le \sqrt{\frac{1}{2}D_{KL}(P^{\otimes n}\|Q^{\otimes n})} = \sqrt{\frac{n}{2}D_{KL}(P\|Q)} = O(\sqrt{n}\delta)$.

**Popular f-divergences that tensorize:**
* **$H^2$**: $1 - \frac{1}{2}H^2(\prod P_i, \prod Q_i) = \prod (1 - \frac{1}{2}H^2(P_i, Q_i))$
* **KL**: $D_{KL}(\prod P_i \| \prod Q_i) = \sum D_{KL}(P_i \| Q_i)$
* **$\chi^2$**: $\chi^2(\prod P_i \| \prod Q_i) + 1 = \prod (\chi^2(P_i \| Q_i) + 1)$
* **Remark**: These properties follow from the tensorization of Rényi divergences, where $D_\lambda(\prod P_i \| \prod Q_i) = \sum D_\lambda(P_i \| Q_i)$ for $D_\lambda(P\|Q) := \frac{1}{\lambda-1} \log E_Q[(\frac{dP}{dQ})^\lambda]$. For $\lambda = 1/2, 1, 2$, $D_\lambda$ corresponds to $H^2$, KL, and $\chi^2$ respectively.

---

### 3. Similarities and Differences Between f-divergences

1. **Locally $\chi^2$-like:**
	* When $P \approx Q$ and $f''(1)$ exists (assuming $f'(1)=0$), a Taylor expansion shows: $$
	D_f(P\|Q) = E_Q\left[f\left(\frac{dP}{dQ}\right)\right] \approx \frac{f''(1)}{2} \chi^2(P\|Q) \text{} $$
2. **In Parametric Models: Fisher Information:**
	* For a regular parametric model $\{P_\theta\}$, the $\chi^2$ divergence is locally related to the **Fisher information matrix** $I(\theta)$: $$
	\chi^2(P_{\theta+th} \| P_\theta) \approx t^2 h^T I(\theta) h \text{}$$
	where $I(\theta) = E\left[(\nabla_\theta \log f_\theta(X))(\nabla_\theta \log f_\theta(X))^T\right] = E[-\nabla_\theta^2 \log f_\theta(X)]$.
	
3. **f-divergence as "average statistical information"**
	* In binary hypothesis testing with prior $\pi = P(H_0)$, the Bayes error is $$
	B_\pi(P,Q) = \inf_T (\pi P(T=1) + (1-\pi)Q(T=0)) = \int (\pi dP \wedge (1-\pi)dQ)$$
	* The statistical information is the reduction in Bayes error, $$I_\pi(P,Q) = \pi\wedge(1-\pi) - B_\pi(P,Q)$$This is an f-divergence with $f_\pi(t) = \pi\wedge(1-\pi) - (\pi t)\wedge(1-\pi)$.

> [!tip] Theorem (Liese & Vajda '06)
> Any f-divergence can be expressed as an average of statistical information over different priors, weighted by a measure $\Gamma_f$.
> $$
> D_f(P\|Q) = \int_{0}^{1} I_\pi(P,Q) \Gamma_f(d\pi) \quad \forall P,Q \text{}
> $$

### Different Guarantees on Contiguity
**Contiguity** ($\{P_n\} \triangleleft \{Q_n\}$) means that if $Q_n(A_n) \to 0$, then $P_n(A_n) \to 0$.
* $TV(P_n, Q_n) \to 0$ implies contiguity.
* $KL(P_n \| Q_n) \le C$ also establishes contiguity.
* $\chi^2(P_n \| Q_n) \le C$ gives a stronger guarantee: $P_n(A_n) \le Q_n(A_n) + \sqrt{C Q_n(A_n)}$.
* Different f-divergences have varying power in establishing contiguity, based on the growth of $f(t)$ as $t \to \infty$. Bounding $\chi^2$ is a popular technique known as the "second moment method".

---

### 4. Dual Representations of f-divergence
* f-divergences have dual representations based on the convex conjugate $f^*(y) = \sup_x (xy - f(x))$.

1. **Properties of Conjugate Functions:**
	* $f^*$ is convex.
	* $f^{**} = f$.
	* Young's inequality: $xy \le f(x) + f^*(y)$.

2. **Theorem**:
$$
D_f(P\|Q) = \sup_g \left( E_P[g] - E_Q[f^* \circ g] \right) \text{}
$$
3. **Examples**:
	1.  **TV**: For $f(x) = \frac{1}{2}|x-1|$, the dual representation is $TV(P,Q) = \frac{1}{2} \sup_{\|g\|_\infty \le 1} |E_P g - E_Q g|$.
	2.  **KL**: For $f(x) = x \log x$, $f^*(y) = e^{y-1}$, leading to $D_{KL}(P\|Q) = \sup_g (E_P g - E_Q[e^{g-1}])$. This is related to the Donsker-Varadhan representation.
	3.  **$\chi^2$**: For $f(x) = (x-1)^2$, $f^*(y) = y + \frac{y^2}{4}$, leading to $\chi^2(P\|Q) = \sup_g \frac{(E_P g - E_Q g)^2}{Var_Q g}$. This implies the **Hammersley-Chapman-Robbins (HCR) lower bound** for the variance of an unbiased estimator, which recovers the Cramér-Rao bound.
	4.  **JS**(Jensen-Shannon Divergence): The dual representation of JS divergence is used to derive the objective function for Generative Adversarial Networks (GANs). The goal is to $\min_G \max_D E_{x \sim P_{data}}[\log D(x)] + E_{z \sim \text{noise}}[\log(1-D(G(z)))]$.

---

### 5. Joint Range and Inequalities
To prove inequalities between two f-divergences (e.g., Pinsker's inequality $2 TV(P,Q)^2 \le D_{KL}(P\|Q)$), one can study their **joint range**.

> [!Definition]
> The joint range is the set of all possible pairs of divergence values: $R = \{ (D_f(P\|Q), D_g(P\|Q)) \}$ over all valid P and Q.

> [!tip] Theorem (Harremoës-Vajda '11)
>The joint range $R$ is equal to the convex hull of the joint range for binary distributions ($R_2$), and also equal to the joint range for distributions on 4 elements ($R_4$).

* **Implication**: To establish an inequality between $D_f$ and $D_g$, it is sufficient to prove it for binary distributions $P=(p, 1-p)$ and $Q=(q, 1-q)$.

**Examples of inequalities:**
* **TV vs. $H^2$**: $\frac{H^2}{2} \le TV \le \sqrt{H^2(1 - \frac{H^2}{4})}$
* **TV vs. KL**: $TV^2 \le \frac{1}{2} KL$ and $TV \le 1 - \frac{1}{2}\exp(-KL)$
* **KL vs. $\chi^2$**: $KL \le \log(1 + \chi^2)$

---

### Special Topic: Chain Rule for $H^2$

> [!NOTE] Theorem (Jayram '09)
> For all $P_{X^n}, Q_{X^n}$:
> $$
> H^2(P_{X^n}, Q_{X^n}) \le C \sum_{i=1}^n E_P\left[H^2(P_{X_i|X^{i-1}}, Q_{X_i|X^{i-1}})\right] \text{}
> $$
> with $C = \prod_{i=1}^\infty \frac{1}{1-2^{-i}} \approx 3.46$.

The proof is combinatorial and relies on several lemmas.

* **Lemma 1 ($L^2$ geometry)**: For distributions $P_0, ..., P_m$, $\frac{1}{m} \sum_{1 \le i < j \le m} H^2(P_i, P_j) \le \sum_{i=1}^m H^2(P_i, P_0)$. This holds because $H^2$ is an $L^2$ distance.
* **Lemma 2 (Cut-paste property)**: Using interpolating distributions $P^A$, if indicator vectors satisfy $a+b=c+d$, then $H^2(P^A, P^B) = H^2(P^C, P^D)$.
* **Lemma 3 (1-factorization of cliques)**: For even n, the complete graph $K_n$ can be decomposed into $(n-1)$ edge-disjoint perfect matchings.

The proof proceeds by induction on partitions of the index set $[n]$.