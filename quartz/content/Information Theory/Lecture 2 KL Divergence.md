

> [!NOTE] Defn. (KL Divergence) 
> For two probability distributions P, Q over the same space, the Kullback-Leibler divergence (or the relative entropy) of P w.r.t. Q is $$D_{KL}(P||Q) := \begin{cases} E_{X \sim P}[\log \frac{dP}{dQ}(X)] & \text{if } P \ll Q \\ +\infty & \text{o.w.} \end{cases}$$

**Remark:** 
1. The above defn. covers both discrete and continuous cases, i.e. $D_{KL}(P||Q) = \sum_{x \in \mathcal{X}} p(x) \log \frac{p(x)}{q(x)}$, if p, q are pmfs. $D_{KL}(P||Q) = \int p(x) \log \frac{p(x)}{q(x)} d\mu(x)$, if p, q are pdfs w.r.t. $\mu$. 
2. This is a divergence rather than a distance, i.e. $D_{KL}(P||Q) \ne D_{KL}(Q||P)$. For this reason, we write $D_{KL}(P||Q)$ instead of $D_{KL}(P,Q)$. 
3. IT origin: $D_{KL}(P||Q)$ is the "redundancy" of using Q for source coding while the true distribution is P. $D_{KL}(P||Q) = \sum_x p(x) \log \frac{1}{q(x)} - H(P)$. The first term is the expected codelength of using Q for source P, and the second is the optimal expected codelength.

**Basic properties** 
**Property 1:** $D_{KL}(P||Q) \ge 0$, with equality iff $P=Q$. 
**Pf.** $D_{KL}(P||Q) = -E_P[\log \frac{dQ}{dP}] \ge -\log E_P[\frac{dQ}{dP}] = -\log \int \frac{dQ}{dP} dP = -\log \int dQ = 0$. Note: this gives the usual proof of $I(X;Y) = E_{XY}[\log \frac{P_{XY}(x,y)}{P_X(x)P_Y(y)}] = D_{KL}(P_{XY} || P_X P_Y) \ge 0$. Also, equality holds iff $P_{XY} = P_X P_Y$, i.e. X and Y are independent. 
**Property 2:** $(P,Q) \mapsto D_{KL}(P||Q)$ is jointly convex. 
**Pf.** Follows from the joint convexity of $(x,y) \mapsto x \log \frac{x}{y}$ over $\mathbb{R}_+^2$, whose Hessian is $[\begin{smallmatrix} 1/x & -1/y \\ -1/y & x/y^2 \end{smallmatrix}] \ge 0$. 
**Property 3 (Chain rule):** $$D_{KL}(P_{X^n}||Q_{X^n}) = \sum_{i=1}^n E_{P_{X^{i-1}}}[D_{KL}(P_{X_i|X^{i-1}}||Q_{X_i|X^{i-1}})]$$**Pf.** $D_{KL}(P_{X^n}||Q_{X^n}) = E_{P_{X^n}}[\log \frac{P_{X^n}}{Q_{X^n}}]$ $= E_{P_{X^n}}[\sum_{i=1}^n \log \frac{P_{X_i|X^{i-1}}}{Q_{X_i|X^{i-1}}}]$ $= \sum_{i=1}^n E_{P_{X^{i-1}}} E_{P_{X_i|X^{i-1}}}[\log \frac{P_{X_i|X^{i-1}}}{Q_{X_i|X^{i-1}}}]$ $= \sum_{i=1}^n E_{P_{X^{i-1}}}[D_{KL}(P_{X_i|X^{i-1}}||Q_{X_i|X^{i-1}})]$.


### Data processing inequality (DPI) 
An important property of KL divergence. If $P_X, Q_X \xrightarrow{P_{Y|X}} P_Y, Q_Y$, then $$D_{KL}(P_X||Q_X) \ge D_{KL}(P_Y||Q_Y)$$ 
(i.e. distributions become "closer" after processing) 
**Pf. 
(Method 1: Convexity)** Verify that $E_{Q_{X|Y}}[\frac{P_X}{Q_X}] = \frac{P_Y}{Q_Y}$ (exercise). Then $D_{KL}(P_Y||Q_Y) = E_{Q_Y}[\frac{P_Y}{Q_Y} \log \frac{P_Y}{Q_Y}]$ $\le E_{Q_Y}[E_{Q_{X|Y}}[\frac{P_X}{Q_X} \log \frac{P_X}{Q_X}]]$ (Jensen's on xlogx) $= E_{Q_X}[\frac{P_X}{Q_X} \log \frac{P_X}{Q_X}] = D_{KL}(P_X||Q_X)$. 
**(Method 2: chain rule)** Let $P_{XY} = P_X P_{Y|X}$, $Q_{XY} = Q_X P_{Y|X}$. $D_{KL}(P_X||Q_X) = D_{KL}(P_X||Q_X) + E_{P_X}[D_{KL}(P_{Y|X}||P_{Y|X})] = D_{KL}(P_{XY}||Q_{XY})$ $= D_{KL}(P_Y||Q_Y) + E_{P_Y}[D_{KL}(P_{X|Y}||Q_{X|Y})] \ge D_{KL}(P_Y||Q_Y)$.

### Applications of DPI 
1. **DPI of mutual information:** if $X \rightarrow Y \rightarrow Z$, then $I(X;Y) \ge I(X;Z)$. **Pf.** $P_{XY}, P_X P_Y \xrightarrow{P_{Z|Y}} P_{XZ}, P_X P_Z$ (by Markov). $\Rightarrow I(X;Y) = D_{KL}(P_{XY}||P_X P_Y) \ge D_{KL}(P_{XZ}||P_X P_Z) = I(X;Z)$. 
2. **Fano's inequality:** if $X \sim \text{Unif}([M])$, then $P(X \ne Y) \ge 1 - \frac{I(X;Y) + \log 2}{\log M}$. **Pf.** The map $(x,y) \mapsto \mathbf{1}(x=y)$ is a form of data processing. $P_{XY}, P_X P_Y \rightarrow \text{Bern}(P(X=Y)), \text{Bern}(1/M)$. $\Rightarrow I(X;Y) = D_{KL}(P_{XY}||P_X P_Y) \ge D_{KL}(\text{Bern}(P(X=Y))||\text{Bern}(1/M))$. The rest follows from algebraic manipulation. 
3. **Contiguity:** For any event A, $P(A)\log\frac{P(A)}{eQ(A)} \le D_{KL}(P||Q)$. (So if $D_{KL}(P||Q) = O(1)$, then $Q(A) \rightarrow 0 \Rightarrow P(A) \rightarrow 0$). **Pf.** The map $x \mapsto \mathbf{1}(x \in A)$ is a data processing step. $P, Q \rightarrow \text{Bern}(P(A)), \text{Bern}(Q(A))$. $\Rightarrow D_{KL}(P||Q) \ge D_{KL}(\text{Bern}(P(A))||\text{Bern}(Q(A)))$.


### Dual representation of KL
**Donsker-Varadhan:** $D_{KL}(P||Q) = \sup_f E_P f - \log E_Q[e^f]$, where the sup is over all functions $f$ with $E_Q[e^f] < \infty$. 
**Gibbs variational principle:** $\log E_Q[e^f] = \sup_P E_P f - D_{KL}(P||Q)$. Both results have numerous applications in practice. 
### Application 
1: transportation inequalities 
**Example 1.1 (Pinsker's inequality):** Restricting Donsker-Varadhan to $f = \lambda g$ with $||g||_\infty \le 1$: $D_{KL}(P||Q) \ge \sup_{\lambda \in \mathbb{R}} \lambda E_P[g] - \log E_Q[e^{\lambda g}]$ Using Hoeffding's inequality, $\log E_Q[e^{\lambda g}] \le \lambda E_Q[g] + \frac{\lambda^2}{2}$. $D_{KL}(P||Q) \ge \sup_{\lambda \in \mathbb{R}} \lambda(E_P[g] - E_Q[g]) - \frac{\lambda^2}{2} = \frac{1}{2}(E_P[g] - E_Q[g])^2$. Taking the supremum over $g$ yields $D_{KL}(P||Q) \ge \frac{1}{2}(2 \cdot TV(P,Q))^2 = 2 \cdot TV(P,Q)^2$. 
**Example 1.2 (Bobkov & Götze):** The following are equivalent: 1. $E_Q[e^{\lambda(f - E_Q f)}] \le \exp(\frac{\lambda^2}{2}C)$ for all 1-Lip functions f. (Sub-Gaussianity of Q) 2. $W_1(P,Q) \le \sqrt{2C \cdot D_{KL}(P||Q)}$ holds for all P. (Transportation inequality) ($W_1$ is the Wasserstein-1 distance). 
### Application 2: variational inference 
**Setting:** A family of distributions $p_\theta(x^n, y^n)$ where we observe $y^n$ but not the latent variables $x^n$. 
**Difficulty:** The marginal likelihood $p_\theta(y^n) = \int p_\theta(x^n, y^n) dx^n$ is often intractable. **Evidence Lower Bound (ELBO):** From Gibbs variational principle, we can derive a lower bound on the log-likelihood: $\log p_\theta(y^n) = \sup_q E_{q(x^n)}[\log p_\theta(y^n|x^n)] - D_{KL}(q(x^n)||p_\theta(x^n))$ $= \sup_q E_{q(x^n)}[\log \frac{p_\theta(x^n, y^n)}{q(x^n)}] =: \text{ELBO}$ 

**Example 2.2 (EM algorithm):** The EM algorithm can be seen as coordinate ascent on the ELBO. - **E-step:** Fix $\theta^{(t)}$, find the best $q$, which is the posterior $q^{(t)}(x^n) = p_{\theta^{(t)}}(x^n|y^n)$. - **M-step:** Fix $q^{(t)}$, find the best $\theta$ by maximizing $E_{x^n \sim q^{(t)}}[\log p_\theta(x^n, y^n)]$. 

**Example 2.3 (VAE):** In a Variational Autoencoder, we introduce a parametric approximation $q_\phi(x^n|y^n)$ (the encoder) to the true posterior and then maximize the ELBO with respect to both the generative model parameters $\theta$ and the variational parameters $\phi$. --- 
### Application 3: adaptive data analysis 
**Problem:** How much bias is introduced when we choose a function $\phi_T$ to analyze based on the data $X^n$ itself? **Result (Russo & Zou '16):** If each $\phi_t$ is $\sigma^2$-sub-Gaussian under P, then the bias is bounded by the mutual information between the data and the choice of function: $$|E[P_n\phi_T] - E[P\phi_T]| \le \sqrt{\frac{2\sigma^2}{n} I(X^n;T)}$$ This shows that the more the choice of analysis $T$ depends on the data $X^n$, the larger the potential bias. --- ### Application 4: PAC-Bayes The PAC-Bayes inequality provides a high-probability bound on the performance of a weighted average of hypotheses. **PAC-Bayes inequality:** Fix a prior distribution $\pi$ on a hypothesis class $\Theta$. Then with probability $\ge 1-\delta$ (over the draw of data $X$), for all posterior distributions $\rho$ on $\Theta$: $$E_{\theta \sim \rho}[\text{error}(\theta)] \le E_{\theta \sim \rho}[\text{empirical\_error}(\theta)] + \sqrt{\frac{D_{KL}(\rho||\pi) + \log(1/\delta)}{2n}}$$ (This is a simplified version. The lecture shows a more general form.) This framework is powerful because $\rho$ can be a data-dependent posterior. It has been used to derive sharp, non-asymptotic bounds in various high-dimensional statistics problems. **Example 4.2:** Proving a sharp concentration inequality for the norm of a Gaussian vector: For $X \sim N(0, \Sigma)$, with high probability: $$||X||_2 \le \sqrt{\text{Tr}(\Sigma)} + \sqrt{2||\Sigma||_{op}\log\frac{1}{\delta}}$$ **Example 4.3:** Bounding the operator norm of the difference between the sample and true covariance matrices. The PAC-Bayes approach yields bounds that tightly capture the dependence on the "effective rank" $r(\Sigma) = \frac{\text{Tr}(\Sigma)}{||\Sigma||_{op}}$.
#### Application

1. DPI for mutual information: 
$$
I(X, Y) \geq I(X, Z)
$$
2. Fano's inequality
3. Contiguity

$$
\forall \text{ event } A:\quad P(A)\log\frac{e\,P(A)}{Q(A)} \;\le\; D_{\mathrm{KL}}(P\|Q)
$$


Dual representation of KL: move from distribution to functions

**Donsker–Varadhan.**

$$
D_{\mathrm{KL}}(P\|Q) \;=\; \sup_f \Big( \mathbb{E}_P f \;-\; \log \mathbb{E}_Q[e^f] \Big),
$$
where the sup is taken over all functions f with $\mathbb{E}_Q[e^f] < \infty$.

**Gibbs variational principle**




#### Application
##### Application 1: Transportation inequality

**Example 1.1** Restricting Donsker–Varadhan to $f = \lambda g$ with $\|g\|_\infty \le 1$
$$
D_{\mathrm{KL}}(P\|Q) \;\ge\; \sup_{\lambda \in \mathbb{R}, \, \|g\|_\infty \le 1} \; \lambda \, \mathbb{E}_P[g] \;-\; \log \mathbb{E}_Q[e^{\lambda g}]
$$
Proof:

**Example 1.2**
1. 
$$
\mathbb{E}_Q\!\left[e^{\lambda (f - \mathbb{E}_Q f)}\right] \;\le\; \exp\!\left(\tfrac{\lambda^2}{2}C\right) \quad \text{for all 1-Lip functions $f$ and $\lambda \in \mathbb{R}$;}
$$

2. 
$$
W_1(P,Q) \;\le\; \sqrt{2C \cdot D_{\mathrm{KL}}(P\|Q)} \quad \text{holds for all $P$.}
$$





##### Application 2: Variational Inference






Example 2.2  EM algorithm

ELBO: evidence lower bound




##### Application 3: Adaptive data analysis





