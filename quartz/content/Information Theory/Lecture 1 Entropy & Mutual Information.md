
### Entropy 

> [!NOTE] Definition of Entropy
> For a discrete RV $X$ taking value in $\mathcal{X}$ with pmf $p$, its entropy $H(X)$ (or $H(p)$) is defined as $$H(X) := -\sum_{x \in \mathcal{X}} p(x) \log \frac{1}{p(x)}$$

 **Remarks:** 
 1. $0 \le H(X) \le \log |\mathcal{X}|$ (by Jensen: $H(X) \le \log \sum_{x \in \mathcal{X}} p(x) \frac{1}{p(x)} = \log |\mathcal{X}|$) 
 2. $H(X)$ can be finite or infinite when $|\mathcal{X}| = \infty$ 
 3. For continuous (or general) RVs, need to find a measure $\mu$ s.t. $X$ has a density $f$ w.r.t. $\mu$, and define the differential entropy $$h(X) := -\int_{\mathcal{X}} f(x) \log \frac{1}{f(x)} d\mu(x)$$ (usually lower-case h; value depends on the choice of $\mu$) 
 4. Base of log: for IT applications (only this lecture), take $\log = \log_2$ (bits); For other applications (all later lectures), $\log = \log_e$ (nats).
 
 **Why entropy?** Shannon (1948) shows that entropy characterizes the fundamental limit of source coding. 

### Source Coding Problem (for the i.i.d. case) 
**Given:** 
* an input alphabet $\mathcal{X}$ (e.g. all English letters {a, b, ..., z}) 
* a known pmf $p$ on $\mathcal{X}$ (i.e. the source distribution)
***Target:** find a map (i.e. code) $f: \mathcal{X} \rightarrow \{0,1\}^* := \bigcup_{n=1}^\infty \{0,1\}^n$, such that 
1. it's uniquely decodable, i.e. based on the concatenation $(f(x_1), \dots, f(x_m))$, one can uniquely decode $m$ and $(x_1, \dots, x_m) \in \mathcal{X}^m$ 
2. the expected code length $E[l(f(X))] = \sum_{x \in \mathcal{X}} p(x)l(f(x))$ is minimized (l(-) length of the code word (in bits))
---

**Example.** If $\mathcal{X} = \{a, b, c\}$ and $P = (\frac{1}{4}, \frac{1}{2}, \frac{1}{4})$ 
* (a) The code $a \rightarrow 00, b \rightarrow 10, c \rightarrow 11$ is uniquely decodable, e.g. `10001011` decodes into `babc`. The expected codelength is $2 \cdot \frac{1}{4} + 2 \cdot \frac{1}{2} + 2 \cdot \frac{1}{4} = 1.75$ bits. 
* (b) The code $a \rightarrow 0, b \rightarrow 1, c \rightarrow 10$ is NOT uniquely decodable. e.g. `10` decodes into either `ba` or `c`. 
* (c) The code $a \rightarrow 10, b \rightarrow 0, c \rightarrow 11$ is uniquely decodable and has a smaller expected codelength $2 \cdot \frac{1}{4} + 1 \cdot \frac{1}{2} + 2 \cdot \frac{1}{4} = 1.5$ bits < 1.75 bits for (a). 

Given a length profile $\{l_x\}_{x \in \mathcal{X}}$, is there a uniquely decodable code $f$ with $l(f(x)) = l_x$?


> [!Tip] Theorem (Kraft-McMillan) 
> A necessary and sufficient condition is 
> $$\sum_{x \in \mathcal{X}} 2^{-l_x} \le 1$$
> (Kraft inequality)

**Pf**. 
**(Sufficiency)** 
First note that for a full binary tree (i.e. a binary tree where each node has 0 or 2 children), then $\sum_{v \text{ is leaf node}} 2^{-\text{depth}(v)} = 1$. Because $\sum_{x \in \mathcal{X}} 2^{-l_x} \le 1$, one can construct a full binary tree s.t. $\mathcal{X} \subseteq \{\text{leaf nodes}\}$ and depth$(x) = l_x, \forall x \in \mathcal{X}$. 

Now use the coding scheme in the example, which results in a prefix code, i.e. no codeword is a prefix of the other. Easy to show that (Exercise) prefix codes are uniquely decodable. 

**(Necessity)** 
WLOG assume $|\mathcal{X}| < \infty$ and $l_{\text{max}} := \max_{x \in \mathcal{X}} l_x < \infty$. Use tensor power trick for uniquely decodable code $f$. 
$$
\begin{align}
\left( \sum_{x \in X} 2^{-l(f(x))} \right)^m
&= \sum_{x_1, \dots, x_m \in X} 2^{-\big(l(f(x_1)) + \cdots + l(f(x_m))\big)} \\
&= \sum_{x_1, \dots, x_m \in X} 2^{-\, l\big(f(x_1), \dots, f(x_m)\big)} 
\quad \text{(concatenation)} \\
&= \sum_{\ell=1}^{m l_{\max}} 2^{-\ell} \cdot 
   \#\{\text{concatenated codewords of total length } \ell\} \\
&\le \sum_{\ell=1}^{m l_{\max}} 2^{-\ell} \cdot 2^\ell 
\quad \text{(by uniquely decodable assumption)} \\
&= m l_{\max}.
\end{align}
$$
$$
\implies \sum_{x \in X} 2^{-l(f(x))} \;\le\; 
\big(m l_{\max}\big)^{1/m} \;\xrightarrow[m \to \infty]{}\; 1.
$$
 Using Kraft inequality, we obtain the following characterization of the smallest expected codelength.


> [!Tip] Theorem (Source coding theorem for uniquely decodable code)
> $$
> H(X) \le \min_{f: \text{uniquely decodable}} E[l(f(X))] < H(X) + 1
> $$

 **Pf.** 
 **(Upper bound)** $l_x = \lceil \log_2 \frac{1}{p(x)} \rceil$ satisfies Kraft inequality and $$\sum_{x \in \mathcal{X}} p(x) l_x < \sum_{x \in \mathcal{X}} p(x) (\log_2 \frac{1}{p(x)} + 1) = H(X) + 1.$$ **(Lower bound)** Easy to show via Lagrangian multipliers that $$\min_{l \in \mathbb{R}_+^{|\mathcal{X}|}} \sum_x p(x) l_x \quad \text{s.t.} \quad \sum_x 2^{-l_x} \le 1$$ The minimum is $H(X)$.

**Remark:** 
1. The gap between $H(X)$ and $H(X)+1$ could be significant (e.g. when $H(X) \ll 1$ bit). However, in practice, the alphabet $\mathcal{X}$ is usually "super-symbols", e.g. $\mathcal{X} = \{a, \dots, z\}^{256}$ instead of $\{a, \dots, z\}$. In such cases, $H(X)$ is large. 
2. Information theory is usually good at proving "robust" results even if a small error probability can be tolerated; in contrast, the above combinatorial argument fails to do so. See more details below.

### Asymptotic Equipartition Property (AEP) 
Another way to write the entropy is $$H(X) = E_{X \sim p} [\log \frac{1}{p(X)}]$$ (Warning: some of you might not be used to seeing the distribution $p$ to appear in BOTH the expectation AND the function) Therefore, if $X_1, \dots, X_n \stackrel{\text{i.i.d.}}{\sim} p$, LLN leads to (if $H(X) < \infty$) $$-\frac{1}{n} \log p(X_1, \dots, X_n) = -\frac{1}{n} \sum_{i=1}^n \log p(X_i) \xrightarrow{\text{a.s.}} E[\log \frac{1}{p(X)}] = H(X);$$ This suggests $p(X_1, \dots, X_n) \approx 2^{-nH(X)}$. For any $\epsilon > 0$, $P(p(X_1, \dots, X_n) \in [2^{-n(H(X)+\epsilon)}, 2^{-n(H(X)-\epsilon)}]) \rightarrow 1$. Call this set $T_n^\epsilon$ (typical set). 


> [!Tip] Theorem: Asymptotic Equipartition property
> The typical set $T_n^\epsilon$ satisfies that 
> 1. $P((X_1, \dots, X_n) \in T_n^\epsilon) \rightarrow 1$ as $n \rightarrow \infty$. 
> 2. $(1-o(1)) 2^{n(H(X)-\epsilon)} \le |T_n^\epsilon| \le 2^{n(H(X)+\epsilon)}$ as $n \rightarrow \infty$.

In other words, AEP states that for $X_1, \dots, X_n \stackrel{\text{i.i.d.}}{\sim} p$, the joint distribution of $X_1, \dots, X_n$ is "roughly" a uniform distribution over $\approx 2^{nH(P)}$ typical sequences.

### Source coding theorem with error probability 
**Diagram:** $X_1, \dots, X_n \stackrel{\text{i.i.d.}}{\sim} p \xrightarrow{\text{encoder}} Y \in \{0,1\}^* \xrightarrow{\text{decoder}} (\hat{X}_1, \dots, \hat{X}_n)$ with a block error guarantee $\mathbb{P}((X_1, \dots, X_n) \ne (\hat{X}_1, \dots, \hat{X}_n)) \le \delta$. 


> [!Tip] Title
> 1. **Achievability:** $\exists$ (encoder, decoder) s.t. $\frac{1}{n} E[l(Y)] \le H(P) + o(1)$ and $\delta = o(1)$. 
> 2. **Converse:** if $\delta = o(1)$, then ANY (encoder, decoder) pair satisfies $\frac{1}{n} E[l(Y)] \ge H(P) - o(1)$. 


**Pf.
(Achievability)** Consider an encoder-decoder pair that enumerates all typical sequences in $T_n^\epsilon$ and ignores all others. Then by AEP, 
error prob = $P((X_1, \dots, X_n) \notin T_n^\epsilon) \rightarrow 0$ - $l(Y) \le \log_2 |T_n^\epsilon| \le n(H(P) + \epsilon)$ deterministically. 
Since $\epsilon > 0$ is arbitrary, the achievability follows. 

**(Converse)** Fix any $\epsilon > 0$. Define two sets 
- $A = \{(X_1, \dots, X_n) : l(Y) > n(H(P) - 2\epsilon)\}$ 
- $B = \{(X_1, \dots, X_n) : (X_1, \dots, X_n) = (\hat{X}_1, \dots, \hat{X}_n)\}$. 
- By AEP and union bound, $P(T_n^\epsilon \cap B) \ge 1 - \delta - o(1)$.

Moreover,
$$|T_n^\epsilon \cap B \cap A^c| = |\{(X_1, \dots, X_n) \in T_n^\epsilon \cap B; l(Y(X_1, \dots, X_n)) \le n(H(P) - 2\epsilon)\}|\le |\{y \in \{0,1\}^*; l(y) \le n(H(P) - 2\epsilon)\}|$$ (by defn. of B, if $(x_1, \dots) \ne (x_1', \dots)$ are different, one must have $Y(x_1, \dots) \ne Y(x_1', \dots)$) $= \sum_{k=1}^{n(H(P)-2\epsilon)} 2^k < 2 \cdot 2^{n(H(P)-2\epsilon)}$ $\Rightarrow P(T_n^\epsilon \cap B \cap A^c) \le |T_n^\epsilon \cap B \cap A^c| \cdot 2^{-n(H(P)-\epsilon)}$ (by AEP) $< 2 \cdot 2^{-n\epsilon}$ 

Therefore, $$P(T_n^\epsilon \cap A \cap B) \ge 1 - \delta - o(1) - 2 \cdot 2^{-n\epsilon} = 1 - o(1)\Rightarrow \frac{1}{n} E[l(Y)] \ge (H(P) - 2\epsilon) P(A) \ge (1 - o(1))(H(P) - 2\epsilon)$$Since $\epsilon > 0$ is arbitrary, the converse follows.

By Markov's inequality. Since $\varepsilon > 0$ is arbitrary, the converse follows.


### Joint entropy and mutual information 
Similar to $H(X) = E_X[\log \frac{1}{p(X)}]$, can also define 
- $H(X,Y) = E_{X,Y}[\log \frac{1}{p(X,Y)}]$ (joint entropy) 
- $H(Y|X) = E_{X,Y}[\log \frac{1}{p(Y|X)}] = H(X,Y) - H(X)$ (conditional entropy) 
- $I(X;Y) = H(X) + H(Y) - H(X,Y) = H(Y) - H(Y|X) = E_{X,Y}[\log \frac{p(X,Y)}{p(X)p(Y)}]$ (mutual information) 
- --- 
**Lemma.** $I(X;Y) \ge 0$ (non-negativity of mutual info) or equivalently, $H(X) \ge H(X|Y)$ (conditioning reduces entropy). 
**Pf.** There is a one-line proof using convexity / KL divergence (next lecture), but let's present a proof using typicality / AEP that will be useful later. 
* Define 
	- $T_n^\epsilon(X) = \{(x^n, y^n) : |\frac{1}{n} \sum_{i=1}^n \log_2 \frac{1}{p_X(x_i)} - H(X)| \le \epsilon\}$ 
	- $T_n^\epsilon(Y) = \{(x^n, y^n) : |\frac{1}{n} \sum_{i=1}^n \log_2 \frac{1}{p_Y(y_i)} - H(Y)| \le \epsilon\}$ 
	- $T_n^\epsilon(X,Y) = \{(x^n, y^n) : |\frac{1}{n} \sum_{i=1}^n \log_2 \frac{1}{p_{XY}(x_i, y_i)} - H(X,Y)| \le \epsilon\}$ 
	- and joint typical set $T_n^\epsilon = T_n^\epsilon(X) \cap T_n^\epsilon(Y) \cap T_n^\epsilon(X,Y)$. 
- For $(X_1, Y_1), \dots, (X_n, Y_n) \stackrel{\text{i.i.d.}}{\sim} p_{XY}$, LLN + union bound yields $P((X^n, Y^n) \in T_n^\epsilon) \xrightarrow{n \rightarrow \infty} 1$, from which one deduces that $|T_n^\epsilon| \ge (1 - o(1)) 2^{n(H(X,Y)-\epsilon)}$. Next draw $(\tilde{X}_1, \tilde{Y}_1), \dots, (\tilde{X}_n, \tilde{Y}_n) \stackrel{\text{i.i.d.}}{\sim} P_X P_Y$. Then $1 \ge P((\tilde{X}^n, \tilde{Y}^n) \in T_n^\epsilon)$ $= \sum_{(x^n, y^n) \in T_n^\epsilon} P(\tilde{X}^n = x^n, \tilde{Y}^n = y^n)$ $\ge (1-o(1)) 2^{n(H(X,Y)-\epsilon)} \cdot 2^{-n(H(X)+\epsilon)} \cdot 2^{-n(H(Y)+\epsilon)}$ $= (1-o(1)) 2^{-n(I(X;Y)+3\epsilon)}$ Taking log, $0 \ge -n(I(X;Y)+3\epsilon) + \log(1-o(1))$, so $I(X;Y)+3\epsilon \ge 0$, and $I(X;Y) \ge 0$ by taking $\epsilon \rightarrow 0^+$. 
- 
- 
- --- This is a fundamental inequality to prove other inequalities, e.g. 
1. $H(X_1, \dots, X_n) = \sum_{k=1}^n H(X_k | X^{k-1}) \le \sum_{k=1}^n H(X_k)$ 
2. If $P_{Y^n|X^n} = \prod_i P_{Y_i|X_i}$ then $I(X^n; Y^n) = H(Y^n) - H(Y^n|X^n)$ $= H(Y^n) - \sum_{i=1}^n H(Y_i|X_i)$ ($H(Y^n|X^n) = E[\log \frac{1}{P(Y^n|X^n)}] = E[\sum_i \log \frac{1}{P(Y_i|X_i)}]$) $\le \sum_{i=1}^n H(Y_i) - \sum_{i=1}^n H(Y_i|X_i) = \sum_{i=1}^n I(X_i; Y_i)$ 
3. If $P_{X^n} = \prod_i P_{X_i}$ then $I(X^n; Y^n) = H(X^n) - H(X^n|Y^n)$ $= \sum_i H(X_i) - H(X^n|Y^n)$ $\ge \sum_i H(X_i) - \sum_i H(X_i|Y^i)$ (by 1) $\ge \sum_i H(X_i) - \sum_i H(X_i|Y_i)$ (conditioning reduces entropy) $= \sum_i I(X_i; Y_i)$ 

**Remark:** All inequalities that can be shown via 
* monotonicity: $H(X) \le H(X,Y)$ and 
* submodularity: $H(X_A) + H(X_B) \ge H(X_{A \cup B}) + H(X_{A \cap B})$ 
are called Shannon-type inequalities. 

**Why mutual information?** 
Shannon (1948) shows that it characterizes the fundamental limit of communications/channel coding and lossy compression. 
### Channel coding problem 
**Diagram:** 
Message $m \sim \text{Unif}(1, \dots, M)$ $\xrightarrow{\text{encoder}}$ Channel input $x^n \in \mathcal{X}^n$ $\xrightarrow[\text{known and given by nature}]{\text{Channel } P_{Y|X}}$ Channel output $y^n \in \mathcal{Y}^n$ $\xrightarrow{\text{decoder}}$ Message estimate $\hat{m}$ - $n$ channel uses are independent i.e. $P_{Y^n|X^n} = \prod_i P_{Y_i|X_i}$ - (block) error probability guarantee $P(m \ne \hat{m}) \le \delta$. **Goal:** Given $P_{Y|X}$, aim to send as many messages as possible, or equivalently, maximize the rate of communication $R_n = \frac{\log M}{n}$ (bits per channel use). 

> [!NOTE] Defn (channel capacity) 
> $$C = C(P_{Y|X}) = \max_{P_X} I(X;Y)$$
> with $P_{XY} = P_X P_{Y|X}$. 

(In other words, given the transition probability $P_{Y|X}$ from $X$ to $Y$, design an input distribution $P_X$ s.t. $I(X;Y)$ is maximized) 

**Examples.**
1. **Binary symmetric channel (BSC):** $X, Y \in \{0,1\}$. $P(Y=1|X=0) = P(Y=0|X=1) = \epsilon$. $I(X;Y) = H(Y) - H(Y|X) \le 1 - h_2(\epsilon)$, with equality iff $P_X = [\frac{1}{2}, \frac{1}{2}]$. ($h_2(\epsilon) = \epsilon \log_2 \frac{1}{\epsilon} + (1-\epsilon) \log_2 \frac{1}{1-\epsilon}$ is the binary entropy function) 
2. **Binary erasure channel (BEC):** $X \in \{0,1\}, Y \in \{0, \perp, 1\}$. $P(Y=\perp|X=x) = \epsilon$, $P(Y=x|X=x) = 1-\epsilon$. $I(X;Y) = H(X) - H(X|Y)$ $= H(X) - P(Y \ne \perp) H(X|Y \ne \perp) - P(Y = \perp) H(X|Y = \perp)$ $= H(X) - (1-\epsilon) \cdot 0 - \epsilon H(X)$ $= (1-\epsilon)H(X) \le 1-\epsilon$, with equality if $P_X = [\frac{1}{2}, \frac{1}{2}]$. 


> [!Tip] Thm (Shannon's channel coding theorem)
> Fix any $\epsilon > 0$ 
> 1. **Achievability:** if $R_n < C - \epsilon$ then $\exists$ (encoder, decoder) s.t. $P(m \ne \hat{m}) \rightarrow 0$ as $n \rightarrow \infty$. 
> 2. **(Weak) converse:** if $R_n > C + \epsilon$, then for any (encoder, decoder), $\liminf_{n \rightarrow \infty} P(m \ne \hat{m}) > 0$. 
> 3. (Strong converse: $\liminf_{n \rightarrow \infty} P(m \ne \hat{m}) = 1$; See Lec 4)

  In other words, the maximum rate of communication is (asymptotically) the channel capacity! **Achievability:** random coding & typicality. 
  
  **Random codebook:** generate $X_{(1)}^n, \dots, X_{(M)}^n \stackrel{\text{i.i.d.}}{\sim} P_X^{\otimes n}$. 
  **Encoder** for message $m \in [M]$:** send $X_{(m)}^n$. 
  **Decoder:** find the unique $m \in [M]$ s.t. $(X_{(m)}^n, y^n)$ is jointly typical (see defn. on page 8); if none or not unique, report failure. 
  **Analysis:** WLOG assume that the true message is $m=1$. Then $\hat{m}=m$ if: 1. $(X_{(1)}^n, y^n)$ is jointly typical; 2. none of $(X_{(2)}^n, y^n), \dots, (X_{(M)}^n, y^n)$ is jointly typical. By LLN, $P(\text{event } 1) = 1-o(1)$. For event 2, since $(X_{(2)}^n, Y^n) \sim P_X^{\otimes n} \otimes P_Y^{\otimes n}$ (independent!!), reversing the analysis on Page 8, $P((X_{(2)}^n, y^n) \text{ is jointly typical}) \le 2^{-n(I(X;Y)-3\epsilon)}$. So union bound gives $P(\text{event } 2) \ge 1 - M \cdot 2^{-n(I(X;Y)-3\epsilon)}$. If $\log_2 M < n(I(X;Y)-4\epsilon)$, then $P(\text{event } 2) \ge 1 - 2^{n(I-4\epsilon)} 2^{-n(I-3\epsilon)} = 1 - 2^{-n\epsilon} = 1 - o(1)$. Therefore, $P(\hat{m}=1) = P(\text{1 and 2}) = 1 - o(1)$. 
  
  **Remark:** 
  1. Random coding was a remarkable idea at the time, when algebraic codes were more popular. This also motivated the entire field of Probabilistic methods. 
  2. This coding scheme is computationally expensive. First efficient codes which attain the Shannon limit were found in 2000's, including the spatially coupled LDPC code and polar code. --- ### Weak converse: Fano's inequality (How IT-based ideas are robust to errors) 
  
  **Lemma. (Data processing inequality for MI)** If $X-Y-Z$ forms a Markov chain (i.e. $P_{XYZ} = P_X P_{Y|X} P_{Z|Y}$), then $I(X;Y) \ge I(X;Z)$. 
  **Pf.** Shannon-type inequalities: $I(X;Y) - I(X;Z) = H(X|Z) - H(X|Y)$ $= H(X|Z) - H(X|Y,Z)$ (By Markov) $= I(X;Y|Z) \ge 0$. 

> [!Tip] Thm (Fano's inequality)
> If $X \sim \text{Unif}([M])$, and $Y$ is an estimator for $X$. Then $$P(X \ne Y) \ge 1 - \frac{I(X;Y) + \log 2}{\log M}$$

   
   **Pf.** Let $E = \mathbf{1}(X \ne Y)$. Then $H(X|Y) = H(X) - I(X;Y) = \log M - I(X;Y)$ (since $X \sim \text{Unif}[M]$) On the other hand, $H(X|Y) \le H(X,E|Y) = H(E|Y) + H(X|Y,E)$ $\le H(E) + H(X|Y,E) \le \log 2 + H(X|Y,E)$ $= \log 2 + P(E=1) H(X|Y,E=1) + P(E=0) H(X|Y,E=0)$ $\le \log 2 + P(X \ne Y) \log(M-1) + P(X=Y) \cdot 0$ $\le \log 2 + P(X \ne Y) \log M$. Combining gives $\log M - I(X;Y) \le P(X \ne Y) \log M + \log 2$, rearranging yields the claim. --- To apply Fano's inequality, if $R_n > C + \epsilon$, $$P(m \ne \hat{m}) \ge 1 - \frac{I(m; \hat{m}) + \log 2}{\log M}$$ $$\ge 1 - \frac{I(X^n; Y^n) + \log 2}{\log M} \quad (\text{Markov structure } m - X^n - Y^n - \hat{m})$$ $$\ge 1 - \frac{\sum_{i=1}^n I(X_i; Y_i) + \log 2}{\log M} \quad (\text{Inequality on Page 9})$$ $$\ge 1 - \frac{nC + \log 2}{\log M} \quad (\text{defn. of } C)$$ $$\ge 1 - \frac{nC + \log 2}{n(C+\epsilon)} \xrightarrow{n \rightarrow \infty} 1 - \frac{C}{C+\epsilon} = \frac{\epsilon}{C+\epsilon} > 0,$$ establishing the weak converse.