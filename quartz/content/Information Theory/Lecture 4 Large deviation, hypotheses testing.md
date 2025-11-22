
> [!NOTE] Definition of Types
> For an "empirical distribution" Q on X, let
> $$
> T_Q^n = \left\{ (x_1, \ldots, x_n) \in \mathcal{X}^n : \frac{1}{n} \sum_{i=1}^n \mathbf{1}(x_i = x) = Q(x), \ \forall x \in \mathcal{X} \right\}.
> $$
In other words, $T_Q^n$​ is the set of all length-n sequences with empirical distribution equal to Q)

**Why types?** Types encode all necessary information for $P(x^n)$: 

**Lemma 1.** For $x^n \in T_Q^n$ , then $$P(x^n) = e^{-n(D_{KL}(Q||P) + H(Q))}$$. 
*Pf.* $$P(x^n) = \prod_{i=1}^{n} P(x_i) = \prod_{x \in \mathcal{X}} \prod_{i:x_i=x} P(x)= \prod_{x \in \mathcal{X}} P(x)^{nQ(x)}$$ (by defn. of $T_Q^n$) $= exp(\sum_x nQ(x) \log P(x))$ $= exp(-n(D_{KL}(Q||P) + H(Q)))$. 
Another intriguing property is that, # of sequences in a given type is exponential in n, but # of different types is only polynomial in n. 

**Lemma 2.** # of different type classes $=\binom{n+|X|-1}{|X|-1} \le (n+1)^{|X|-1}$. *Pf.* \# of non-negative integer solutions to $\sum_{x\in\mathcal{X}}n_x=n$ is $\binom{n+|x|-1}{|x|-1}$. 

**Lemma 3.** $\frac{e^{nH(Q)}}{(n+1)^{|X|-1}} \le |T_Q^n| \le e^{nH(Q)}$ (or $|T_Q^n| \approx e^{nH(Q)}$ by ignoring polynomial factors). *Pf.* (Upper bound) $1 \ge P(X^n \in T_Q^n) = |T_Q^n| e^{-n(D_{KL}(Q||Q)+H(Q))} = |T_Q^n| e^{-nH(Q)}$. (Lower bound) $1 = \sum_P P(X^n \in T_P^n)$ $\le (n+1)^{|X|-1} \cdot max_P P(X^n \in T_P^n)$ (mode of a multinomial is $nQ$) $\le (n+1)^{|X|-1} \cdot |T_Q^n| \cdot e^{-nH(Q)}$. 

**Corollary** $$\frac{e^{-nD_{KL}(Q||P)}}{(1+n)^{|X|-1}} \le P(X^n \in T_Q^n) \le e^{-nD_{KL}(Q||P)}$$*Pf.* By Lemma 1 & 3. The above corollary, together with Lemma 2, leads to the following result known as **Sanov's theorem**. 

**Thm.** Let $|\mathcal{X}| < \infty$, and $\hat{P}$ be the empirical distribution (type) of $X_1, ..., X_n \sim P$, a strictly positive P. Let $\Sigma$ be a closed set of distributions with a non-empty interior. Then $P(\hat{P} \in \Sigma) = exp(-n \min_{Q \in \Sigma} D_{KL}(Q||P) + o(n))$. 

**Remark:** The map $P \mapsto arg \min_{Q \in \Sigma} D_{KL}(Q||P)$ is called the "information projection".


*Pf.* 
(Upper bound) $$P(\hat{P} \in \Sigma) = \sum_{Q \in \Sigma} P(X^n \in T_Q^n) \le \sum_{Q \in \Sigma} e^{-nD_{KL}(Q||P)}=\le (n+1)^{|X|-1} e^{-n \min_{Q \in \Sigma} D_{KL}(Q||P)}$$
(Lower bound) For any $Q \in \Sigma$, $$P(X^n \in T_Q^n) \ge \frac{1}{(n+1)^{|X|-1}} e^{-nD_{KL}(Q||P)}$$Choose $Q \to Q^*$ and apply continuity of $D_{KL}(Q||P)$.


## Information projection, exponential tilting and CGF 

A Corollary of Sanov's theorem is as follows. 

**Corollary** 
$$\lim_{n \to \infty} \frac{1}{n} \log \frac{1}{P(\frac{1}{n} \sum_{i=1}^n X_i \ge r)} = \min_{Q: E_Q[X] \ge r} D_{KL}(Q||P)$$If $E_P[X] \ge r$ then one can choose $Q=P$ and then RHS = 0. Can we find the minimizer $Q^*$ if $E_P[X] < r$? 


> [!NOTE] Definition of Exponential tilt
> For $\lambda \in \mathbb{R}$, the exponential tilt of P along X is $$P_\lambda(dx) = exp(\lambda x - \psi(\lambda)) P(dx)$$
> where $\psi(\lambda) = \log E_P e^{\lambda X}$ is the cumulant generating function (CGF) of X. 

(Note: the family of $\{P_\lambda\}$ is called an "exponential family" in statistics, where $\psi(\lambda)$ is called the "log partition function". In particular, $E_{P_\lambda}[X] = \psi'(\lambda)$, and $\lambda \mapsto \psi(\lambda)$ is convex.) 

**Thm ("maximum entropy distribution")** 
If $E_P[X] < r$, and there exists $\lambda \in \mathbb{R}$ s.t. $E_{P_\lambda}[X] = r$. Then 
$$\min_{Q: E_Q[X] \ge r} D_{KL}(Q||P) = D_{KL}(P_\lambda||P)= \lambda r - \psi(\lambda)= \psi^*(r)$$
where $\psi^*$ is the convex conjugate of $\psi$.

*Pf.* 
Since $E_P[X] = \psi'(0) < r = \psi'(\lambda)$, by convexity of $\psi$ we have $\lambda > 0$. 
If $E_Q[X] \ge r$ and $\lambda \ge 0$, then $D_{KL}(Q||P) = E_Q[\log \frac{Q}{P}]$ $= E_Q[\log \frac{Q}{P_\lambda} + \log \frac{P_\lambda}{P}]$ $= D_{KL}(Q||P_\lambda) + E_Q[\lambda X - \psi(\lambda)]$ $\ge \lambda r - \psi(\lambda)$, and $D_{KL}(P_\lambda||P) = E_{P_\lambda}[\lambda X - \psi(\lambda)] = \lambda r - \psi(\lambda)$. 

By assumption, $r = E_{P_\lambda}[X] = \psi'(\lambda)$. Then $\psi^*(r) = \sup_{\lambda^* \in \mathbb{R}} \lambda^* r - \psi(\lambda^*) \le \sup_{\lambda^* \in \mathbb{R}} \lambda^* r - (\psi(\lambda) + (\lambda^* - \lambda)\psi'(\lambda)) = \lambda r - \psi(\lambda)$ by convexity of $\psi$. So $\psi^*(r) = \lambda r - \psi(\lambda)$. In other words, this result shows that the information projection yields an exponential tilt of P, and the value is given by the convex conjugate of the CGF of P.

---
## Large deviation in general alphabets: Cramer's Thm. 

> [!Tip] Cramer's Thm
>For i.i.d. $X_1, ..., X_n \sim P$ with $E_P[X] < r < ||X||_\infty$ then $$\lim_{n \to \infty} \frac{1}{n} \log \frac{1}{P(\frac{1}{n} \sum_{i=1}^n X_i > r)} = \psi^*(r) = \inf_{Q: E_Q[X] > r} D_{KL}(Q||P)$$ 
>where $\psi^*$ is the convex conjugate of the CGF $\psi(\lambda) = \log E_P e^{\lambda X}$. 

**Note:** This generalizes our previous results to arbitrary alphabets. Also, we'll present two different proofs, one probabilistic and one information-theoretic, to arrive at the quantities $\psi^*(r)$ and $\min_{Q: E[X]>r} D_{KL}(Q||P)$, respectively. These proofs will better illustrate the connections between different ideas. 

### Probabilistic proof 
**(Lower bound)** 
By Chernoff inequality. $$P(\frac{1}{n} \sum_i X_i > r) \le \inf_{\lambda \ge 0} e^{-\lambda n r} E_P[e^{\lambda \sum_i X_i}]= \inf_{\lambda \ge 0} exp(-n(\lambda r - \psi(\lambda))) = exp(-n \psi^*(r))$$this step uses $E_P[X] < r$. 

**(Upper bound)** 
Since $E_P[X] < r < ||X||_\infty$, $\exists \lambda > 0$ s.t. $E_{P_\lambda}[X] = r + \epsilon$, where $P_\lambda$ is the exponential tilt of P. By LLN, $P_\lambda(\frac{1}{n} \sum_i X_i \in (r, r+2\epsilon)) = 1 - o(1)$ as $n \to \infty$. At the same time, for $x^n$ with empirical mean in $(r, r+2\epsilon)$, $\frac{dP_\lambda}{dP}(X_1, ..., X_n) = exp(\lambda \sum_i X_i - n\psi(\lambda)) \le exp(n(\lambda(r+2\epsilon) - \psi(\lambda)))$ $\implies P(\frac{1}{n} \sum_i X_i \in (r, r+2\epsilon)) \ge (1-o(1)) exp(-n(\lambda(r+2\epsilon) - \psi(\lambda)))$. Choosing $\epsilon \to 0^+$ completes the proof. 
### IT proof 
**(Upper bound)** Fix any Q with $E_Q[X] > r$. Then for $E_n = \{\frac{1}{n}\sum_i X_i > r\}$, $Q(E_n) = 1 - o(1)$ by LLN. By Lec 2, $Q(E_n) \log \frac{Q(E_n)}{P(E_n)} \le D_{KL}(Q_{X^n}||P_{X^n}) = n D_{KL}(Q||P)$. $\implies \frac{1}{n} \log \frac{1}{P(E_n)} \le \frac{D_{KL}(Q||P)}{Q(E_n)} - \frac{\log(Q(E_n))}{n} = (1+o(1))D_{KL}(Q||P)$. 

**(Lower bound)** Note $\tilde{P}_{X^n} \triangleq P_{X^n | \frac{1}{n}\sum_i X_i > r}$ has mean > r, with $\frac{1}{n} \log \frac{1}{P(E_n)} = \frac{1}{n} D_{KL}(\tilde{P}_{X^n} || P_{X^n})$. We argue that $\frac{1}{n} D_{KL}(\tilde{P}_{X^n}||P_{X^n}) = \inf_{Q: E_Q[X]>r} D_{KL}(Q||P)$. In fact, $D_{KL}(\tilde{P}_{X^n}||P_{X^n}) = \sum_{i=1}^n E_{\tilde{P}}[D_{KL}(\tilde{P}_{X_i|X^{i-1}} || P)]$ $\ge \sum_{i=1}^n D_{KL}(E_{\tilde{P}}\tilde{P}_{X_i|X^{i-1}} || P) \ge n D_{KL}(\frac{1}{n}\sum_i \tilde{P}_{X_i} || P)$. where $\bar{P} := \frac{1}{n}\sum_i \tilde{P}_{X_i}$ clearly satisfies $E_{\bar{P}}[X] = E_{\tilde{P}}[\frac{1}{n}\sum_i X_i] > r$. ---

---

## Simple hypothesis testing 

$H_0: X \sim P$ 
$H_1: X \sim Q$ 
For a test $T = T(X) \in \{0, 1\}$ (possibly randomized), 
define $\alpha = P(T=1)$ (Type I error) $\beta = Q(T=0)$ (Type II error) 

**Def.** Let $R(P,Q)$ denote the set of all achievable points $(\alpha, \beta) \in [0,1]^2$ when T ranges over all possible tests. 

**Basic properties.** 
1. $R(P,Q)$ is convex (Pf: consider a randomized combination of two tests). 
2. $(a, 1-a) \in R(P,Q)$ (Pf: consider $T \sim Bern(a)$ independent of X). 
3. $(\alpha, \beta) \in R(P,Q) \iff (1-\alpha, 1-\beta) \in R(Q,P)$ (Pf: replacing T by 1-T). 
4. **Neyman-Pearson:** likelihood ratio tests (LRT) attain the lower boundary of $R(P,Q)$, i.e., for $$T^* = \begin{cases} 1 & \text{if } \log \frac{p(x)}{q(x)} > \tau \\ \text{randomized} & \text{if } \log \frac{p(x)}{q(x)} = \tau \\ 0 & \text{if } \log \frac{p(x)}{q(x)} < \tau \end{cases}$$ then for any other test T, $\alpha(T) \ge \alpha(T^*) \implies \beta(T) \ge \beta(T^*)$. 

*Pf.* $\alpha(T) \ge \alpha(T^*) \implies E_P[T-T^*] \ge 0$. Since we obtain $E_P[(\frac{dQ}{dP} - e^{-\tau})(T-T^*)] \le 0$ (by distinguishing $\frac{dQ}{dP} \ge e^{-\tau}$), $E_P[\frac{dQ}{dP}(T-T^*)] \ge 0$ i.e., $E_Q[T-T^*] \ge 0 \implies \beta(T) \ge \beta(T^*)$. 

## Asymptotics:  Chernoff regime

 Consider $\begin{cases} H_0: & X^n \sim P^{\otimes n} \\ H_1: & X^n \sim Q^{\otimes n} \end{cases}$ with $n \to \infty$. 
 
 What are all possible values of $(\epsilon_0, \epsilon_1)$ s.t. $T_n$ with $\begin{cases} \alpha(T_n) \le e^{-n\epsilon_0} \\ \beta(T_n) \le e^{-n\epsilon_1} \end{cases}$ asymptotically? 
 
 In other words, what are the best tradeoffs between $(\epsilon_0, \epsilon_1)$, the error exponents on Type I & II errors? 
 
 **Thm ($\epsilon_0 - \epsilon_1$ tradeoff).** Assume $P \ll Q$ and $Q \ll P$. The upper boundary of all achievable $(\epsilon_0, \epsilon_1)$ pairs is given by $\begin{cases} \epsilon_0 = D_{KL}(P_\lambda || P) \\ \epsilon_1 = D_{KL}(P_\lambda || Q) \end{cases}$ for some $\lambda \in [0,1]$. where $P_\lambda \propto P^{1-\lambda}Q^\lambda$. 
 
 **Corollary.** $\max \min(\epsilon_0, \epsilon_1)$ achievable = $\max_{\lambda \in [0,1]} \min(D_{KL}(P_\lambda||P), D_{KL}(P_\lambda||Q))$ $= -\inf_{\lambda \in [0,1]} \log \int (dP)^{1-\lambda}(dQ)^\lambda$. 
 
 **Note:** This quantity, denoted by $C(P,Q)$, is called the **Chernoff information**.
 
  --- 
  
  *Pf of corollary.* For $P_\lambda = \frac{P^{1-\lambda}Q^\lambda}{Z}$ $D_{KL}(P_\lambda||P) = E_{P_\lambda}[\log \frac{P_\lambda}{P}] = E_{P_\lambda}[\lambda \log \frac{Q}{P} - \log Z]$ $D_{KL}(P_\lambda||Q) = E_{P_\lambda}[\log \frac{P_\lambda}{Q}] = E_{P_\lambda}[(1-\lambda) \log \frac{P}{Q} - \log Z]$ $\implies D_{KL}(P_\lambda||P) - D_{KL}(P_\lambda||Q) = E_{P_\lambda}[\log \frac{Q}{P}]$. Let $\lambda^*$ denote the minimizer of the convex function $\log \int P^{1-\lambda}Q^\lambda$. then $0 = \frac{d}{d\lambda} \log \int P^{1-\lambda}Q^\lambda |_{\lambda=\lambda^*} = \frac{1}{Z} \int P^{1-\lambda^*}Q^{\lambda^*} \log \frac{Q}{P} = E_{P_{\lambda^*}}[\log \frac{Q}{P}]$. For this $\lambda^*$, we have $D_{KL}(P_{\lambda^*}||P) = D_{KL}(P_{\lambda^*}||Q)$, and $D_{KL}(P_{\lambda^*}||P) = -\log Z = -\log \int P^{1-\lambda^*}Q^{\lambda^*} = -\inf_{\lambda \in [0,1]} \log \int P^{1-\lambda}Q^\lambda$. 
  
  **Back to the $(\epsilon_0, \epsilon_1)$ tradeoff:** **Achievability** A sufficient statistic is $L = \frac{1}{n} \sum_{i=1}^n L_i \triangleq \frac{1}{n}\sum_{i=1}^n \log \frac{P(X_i)}{Q(X_i)}$. So a natural test is $T_n = \mathbb{I}(L \ge r)$ for some threshold $r \in \mathbb{R}$. By large deviation: $\lim_{n \to \infty} \frac{1}{n} \log \frac{1}{P(L < r)} := \psi_P^*(r) = D_{KL}(P^*||P)$ $\lim_{n \to \infty} \frac{1}{n} \log \frac{1}{Q(L \ge r)} = \psi_Q^*(-r) = D_{KL}(Q^*||Q)$ where $\psi_P(\lambda) = \log E_P e^{\lambda L_1} = \log \int P^{1+\lambda}Q^{-\lambda}$ (similarly for $\psi_Q$) and $P^*(dx) = exp(\lambda_P^* \log \frac{p(x)}{q(x)} - \psi_P(\lambda_P^*)) P(dx)$ with $E_{P^*}[L_1] = r$. $Q^*(dx) = exp(-\lambda_Q^* \log \frac{p(x)}{q(x)} - \psi_Q(-\lambda_Q^*)) Q(dx)$ with $E_{Q^*}[L_1] = r$. Since $P^* \propto P^{1+\lambda_P^*}Q^{-\lambda_P^*}$ and $Q^* \propto P^{\lambda_Q^*}Q^{1-\lambda_Q^*}$ belong to the family $\{P_\lambda\}$, we conclude that $P^* = Q^* = P_{\lambda^*}$, where $\lambda^*$ is the solution to $E_{P_{\lambda^*}}[\log \frac{p(x)}{q(x)}] = r$. Therefore, by choosing r appropriately, this test asymptotically achieves all pairs $(\epsilon_0, \epsilon_1) = (D_{KL}(P_\lambda||P), D_{KL}(P_\lambda||Q))$ for all $\lambda \in [0,1]$. --- **Converse.** Suppose some test $T_n$ asymptotically attains $\alpha(T_n) \le e^{-n\epsilon_0}$ and $\beta(T_n) \le e^{-n\epsilon_1}$. **Weak converse (by DPI):** $D_{KL}(Bern(\alpha)||Bern(1-\beta)) \le nD_{KL}(P||Q)$ $D_{KL}(Bern(\beta)||Bern(1-\alpha)) \le nD_{KL}(Q||P)$ (They are insufficient to establish the tight $(\epsilon_0, \epsilon_1)$ tradeoff!) **Strong converse (on the whole likelihood ratio):** $\forall \gamma > 0$, $\alpha - \frac{\beta}{\gamma} \le P(\sum_{i=1}^n \log \frac{P}{Q}(X_i) < \log \gamma)$ $\beta - \gamma\alpha \le Q(\sum_{i=1}^n \log \frac{P}{Q}(X_i) > \log \gamma)$ *Pf.* Let $L = \sum_{i=1}^n \log \frac{P}{Q}(X_i) = \log \frac{P^{\otimes n}}{Q^{\otimes n}}(X^n)$. Then $\beta - \gamma\alpha = Q^{\otimes n}(T_n=0) - \gamma P^{\otimes n}(T_n=0)$ $= E_{P^{\otimes n}}[(\frac{1}{\gamma}e^{-L} - 1)\mathbb{I}(T_n=0)]$ $\le E_{P^{\otimes n}}[(\frac{1}{\gamma}e^{-L} - 1)\mathbb{I}(T_n=0, L < \log \frac{1}{\gamma})]$ $\le E_{P^{\otimes n}}[\frac{1}{\gamma}e^{-L} \mathbb{I}(L < \log \frac{1}{\gamma})] = Q^{\otimes n}(L < \log \frac{1}{\gamma})$. The other proof is similar. Returning to the converse: choose $\gamma = e^{-n\theta}$. then $e^{-n\epsilon_1} - e^{-n\theta}e^{-n\epsilon_0} \approx \beta - \gamma\alpha \le Q(\frac{1}{n} \sum_{i=1}^n \log \frac{P}{Q}(X_i) > \theta)$ $\implies e^{-n\epsilon_1} + e^{-n(\epsilon_0 + \theta)} \ge Q(\frac{1}{n} \sum_{i=1}^n \log \frac{P}{Q}(X_i) \le \theta)$ $\implies \min\{\epsilon_1, \epsilon_0 + \theta\} \le \psi_Q^*(\theta)$. If $\epsilon_0 \ge D_{KL}(P_\lambda||P) + \epsilon$, $\epsilon_1 \ge D_{KL}(P_\lambda||Q) + \epsilon$, choose $\theta = E_{P_\lambda}[\log \frac{P}{Q}] = D_{KL}(P_\lambda||Q) - D_{KL}(P_\lambda||P)$ (see previous page) $\psi_Q^*(\theta) = D_{KL}(P_\lambda||Q)$ (because $\lambda$ is the solution to $E_{P_\lambda}[\log \frac{P}{Q}]=\theta$) $\implies \min\{\epsilon_1, \epsilon_0 + \theta\} \ge \psi_Q^*(\theta) + \epsilon$, a contradiction! --- ## Special topic: Stein's regime, strong converse for channel coding, finite block length **Stein's regime:** $\begin{cases} H_0: & X^n \sim P^{\otimes n} \\ H_1: & X^n \sim Q^{\otimes n} \end{cases}$ Can we test $T_n$ s.t. $\alpha(T_n) \le \epsilon$ and we want to minimize $\beta(T_n) = e^{-nE}$. What's the largest possible value $E_n^*$ of E? From the Chernoff regime with $\epsilon_0 \to \infty$, we already know that **Thm** $E_n^* = D_{KL}(Q||P) + o(1)$. (Stein's lemma) Can we also get the next-order term? $E_n^* = D_{KL}(Q||P) + \sqrt{\frac{V(Q||P)}{n}}\Phi^{-1}(\epsilon) + O(\frac{\log n}{n})$ where $\Phi(z) = P(N(0,1) \le z)$. $V(Q||P) = Var_Q(\log \frac{Q}{P})$ (assumed to be $<\infty$). **Pf (achievability)** Consider the test $T_n = \mathbb{I}(\frac{1}{n} \sum_{i=1}^n \log \frac{P}{Q}(X_i) > r)$. By CLT, $\frac{1}{\sqrt{n}} \sum_{i=1}^n (\log \frac{P}{Q}(X_i) + D_{KL}(Q||P)) \xrightarrow{d} N(0, V(Q||P))$ under Q. So a threshold $r$ can be chosen to make $\beta(T_n) \approx e^{-nE_n^*}$. For $\alpha(T_n) = P(\frac{1}{n}\sum_i \log \frac{P}{Q}(X_i) > r)$, the CLT under P determines the error $\epsilon$. **(converse)** The strong converse from the previous section is used with a carefully chosen $\gamma$ to match the CLT scaling, which leads to the desired bound. **Note:** If one uses Berry-Esseen bounds, then under moment conditions, the $O(\frac{\log n}{n})$ factor can be improved to $O(\frac{1}{n})$. --- ## Strong converse for channel coding Recall from Lec 1: Message $m \sim Unif(\{1,...,M\})$ -> encoder -> Channel input $X^n \in \mathcal{X}^n$ -> Channel $P_{Y|X}$ -> Channel output $Y^n \in \mathcal{Y}^n$ -> decoder -> $\hat{m} \in \{1,...,M\}$. Communications aim to minimize error probability: $P(\hat{m} \ne m) \le \epsilon$. $R = \frac{\log M}{n}$. In Lec 1, we use Fano's inequality (i.e. DPI for KL) to prove the weak converse $R \le C$ for vanishing error $\epsilon=o(1)$, with $C = \max_{P_X} I(X;Y)$. What happens if $\epsilon = 0.01$, or even $\epsilon=0.999$? **Thm (strong converse).** For any fixed $\epsilon < 1$, $R \le C + o(1)$. **Remark:** This means that the communication problem has a "sharp" threshold on the error probability. When $R < C$, one can achieve arbitrarily low error probability, but when R > C, the success probability cannot be bounded away from zero. *Pf.* The communication problem is not binary hypothesis testing; instead, it is a recovery problem (i.e. recover the message m from $Y^n$). However, a useful idea is to reduce a recovery problem to detection: if one can distinguish between different inputs (recovery), then one can also distinguish from the case where the input and output are independent. This idea is also frequently used in statistical problems. --- Consider two scenarios (i.e. joint distributions on $(X^n, Y^n)$), $H_0: P_{m,X^n,Y^n,\hat{m}} = \frac{1}{M} P_{X^n|m} P_{Y^n|X^n} P_{\hat{m}|Y^n}$ $H_1: Q_{m,X^n,Y^n,\hat{m}} = \frac{1}{M} P_{X^n|m} Q_Y^{\otimes n} P_{\hat{m}|Y^n}$ (i.e. $(m,X^n)$ indep of $(Y^n, \hat{m})$) Then $\begin{cases} P(m=\hat{m}) \ge 1-\epsilon \\ Q(m=\hat{m}) = \frac{1}{M} \end{cases}$ and the likelihood ratio is $\frac{P_{m,X^n,Y^n,\hat{m}}}{Q_{m,X^n,Y^n,\hat{m}}} = \frac{P_{Y^n|X^n}}{Q_Y^{\otimes n}} = \prod_{i=1}^n \frac{P_{Y_i|X_i}}{Q_{Y_i}}$. Therefore, by strong converse, $1-\epsilon - \frac{\gamma}{M} \le P(\sum_i \log \frac{P_{Y_i|X_i}}{Q_{Y_i}} > \log \gamma)$. A technical difficulty: $P_{X^n}$ is often not a product distribution. Solution: When $|\mathcal{X}|<\infty$, can WLOG assume that all codewords $X^n$ have the same type $P_0$. In fact, since there are $\approx (n+1)^{|X|-1}$ types, one can find a type that changes the error probability to $\epsilon+o(1)$ while with a rate change at most $O(\frac{\log n}{n})$. When $X^n$ has type $P_0$ a.s., choose $Q_Y = \sum_x P_0(x) P_{Y|X=x}$. Then $E[\sum_i \log \frac{P_{Y_i|X_i}}{Q_{Y_i}}] = n I(P_0; P_{Y|X}) \le nC$ $Var(\sum_i \log \frac{P_{Y_i|X_i}}{Q_{Y_i}}) = n E_{P_0}[Var(-\log \frac{P_{Y|X}}{Q_Y} | X)] \le n Var(-\log \frac{P_{Y|X}}{Q_Y}) = O(n)$. Now choosing $\gamma = \frac{M}{2}(1-\epsilon)$ in the strong converse, Chebyshev's inequality yields $\log \gamma \le nC + O(\sqrt{n}) \implies R = \frac{\log M}{n} \le C + O(\frac{1}{\sqrt{n}})$. --- ## Converse for finite blocklength Is there a next-order upper bound on R? **Thm.** Suppose that the capacity-achieving distribution $P_X^*$ is unique, and $|\mathcal{X}|, |\mathcal{Y}| < \infty$. Under regularity conditions, $R \le C - \sqrt{\frac{V}{n}} \Phi^{-1}(1-\epsilon) + O(\frac{\log n}{n})$. with $V=E_{P_{x}^{*}}[Var(log\frac{P_{\gamma|x}}{P_{Y}^{*}})]$. *Pf sketch.* Using the previous analysis, and due to the uniqueness of $P_X^*$, we only need to deal with the input type $P_0 \approx P_X^*$. Then the result follows from Stein's regime as long as we can show the variance term matches. This follows from the following lemma. **Lemma.** Any capacity-achieving input $P_X^*$ satisfies $D_{KL}(P_{Y|X=x} || P_Y^*) \le C, \forall x \in \mathcal{X}$ $D_{KL}(P_{Y|X=x} || P_Y^*) = C, \forall x \in supp(P_X^*)$ *Pf.* $0 = \lim_{\epsilon \to 0^+} \frac{I(P_X^*+\epsilon(\delta_x - P_X^*); P_{Y|X}) - I(P_X^*; P_{Y|X})}{\epsilon} = (E_{\delta_x} - E_{P_X^*})[D_{KL}(P_{Y|X}||P_Y^*)]$. Choosing $P_X = \delta_x$ gives the first claim. The second claim follows from $C = E_{P_X^*}[D_{KL}(P_{Y|X}||P_Y^*)] \le C$, So the equality must hold for $x \in supp(P_X^*)$.