**Notation**
* $x$ be our input source,
* $y$ be our target
* $z$ be our latent representation

### Mutual Information

Mutual information (MI) measures the amount of information obtained about one random variable after observing another random variable. Formally given two random variables $x$ and $y$ with joint distribution $p(x,y)$ and marginal densities $p(x)$ and $p(y)$ their MI is defined as the KL-divergence between the joint density and the product of their marginal densities

$$\begin{align}

I(x;y)&=I(y;x)\\

&=KL\Big(p(x,y)||p(x)p(y)\Big)\\

&=\mathbb{E}_{(x,y)\sim p(x,y)}\bigg[\log\frac{p(x,y)}{p(x)p(y)}\bigg]\\

&=\int dxdyp(x,y)\log\frac{p(x,y)}{p(x)p(y)}

\end{align}$$
### Information Bottlenecks

IB regards supervised learning as a representation learning problem, seeking a stochastic map from input data

$x$ to some latent representation $z$ that can still be used to predict the labels $y$ , under a constraint on its total complexity.

We assume our joint distribution $p(x,y,z)$ can be factorized as follows:

$$p(x,y,z)=p(z\mid x,y)p(y\mid x)p(x)=p(z\mid x)p(y\mid x)p(x)$$

which corresponds to the following Markov Chain

$$y\rightarrow x\rightarrow z$$

  Our goal is to learn an encoding that is maximally informative about our target $y$ measured by $I(y;z)$. We could always ensure a maximally informative representation by taking the identity encoding $x=z$ which is not useful. Instead we apply a constraint such that the objective is

$$\begin{alignat}{3}

&\underset{}{\text{max }} & \quad & I(y;z)\\

&\text{subject to } & \quad & I(x;z)\leq I_c

\end{alignat}$$

where $I_c$ is the information constraint. The Lagrangian of the above constrained optimisation problem which we would like to **maximise** is

$$\begin{align}

L_{IB}&=I(y;z)-\beta \big(I(x;z)-I_c\big)\\

&=I(y;z)-\beta I(x;z)

\end{align}$$

where $\beta\geq0$ is a Lagrange multiplier.

* Intuitively the first term encourages $z$ to be predictive of $y$, whilst the second term encourages $z$ to "forget" $x$.

* In essence, IB principle explicitly enforces the learned representation $z$ to only preserve the information in $x$ that is useful to the prediction of $y$, i.e., the minimal sufficient statistics of $x$ w.r.t. $y$.

  

### Variational Information Bottlenecks

**The first term**<br>
We can write out the terms in the objective as

$$I(y;z)=\int dydz p(y,z)\log \frac{p(y,z)}{p(y)p(z)}=\int dydz p(y,z)\log \frac{p(y\mid z)}{p(y)}$$

where $p(y\mid z)$ is defined as

$$p(y\mid z)=\int dx \frac{p(x,y,z)}{p(z)}=\int dx \frac{p(z\mid x)p(y\mid x)p(x)}{p(z)}$$

which is intractable. Let $q(y\mid z)$ be a variational approximation to $p(y\mid z)$. By using the KL divergence we can obtain a lower bound on $I(y;z)$

$$KL\Big(p(y\mid z)|| q(y\mid z)\Big)\geq0\Longrightarrow \int dy p(y\mid z)\log p(y\mid z)\geq \int dy p(y\mid z)\log q(y\mid z)$$

Hence we have that

$$\begin{align}

I(y;z)&= \int dydz p(y,z)\log p(y\mid z) - \int dy p(y)\log p(y)\\

&\geq \int dydz p(y, z)\log q(y\mid z) - \int dy p(y)\log p(y)\\

&=\int dxdydz p(z\mid x)p(y\mid x)p(x)\log q(y\mid z)

\end{align}$$

where the entropy of the labels $H(y)=- \int dy p(y)\log p(y)$ is independent of our optimisation and so can be ignored.

  
  

**The second term**<br>

We can write out the second term in the objective as

$$I(x;z)=\int dxdz p(x,z)\log \frac{p(x,z)}{p(x)p(z)}=\int dxdz p(x,z)\log \frac{p(z\mid x)}{p(z)}$$

Let $q(z)$ be a variational approximation to the marginal $p(z)$. By using the KL divergence we can obtain an upper bound on $I(x;z)$ as

$$KL\Big(p(z)|| q(z)\Big)\geq0\Longrightarrow \int dz p(z)\log p(z)\geq \int dz p(z)\log q(z)$$

Hence we have

$$\begin{align}

I(x;z)&=\int dxdz p(x,z)\log p(z\mid x) - \int dz p(z)\log p(z)\\

&\leq\int dxdz p(x,z)\log p(z\mid x) - \int dz p(z)\log q(z)\\

&=\int dxdz p(x)p(z\mid x)\log \frac{p(z\mid x)}{q(z)}

\end{align}$$

  

### Loss Function

Combining the above two bounds we can rewrite the Lagrangian which we would like to **maximise** as

$$\begin{align}

L_{IB}&=I(y;z)-\beta I(x;z)\\

&\geq \int dxdydz p(z\mid x)p(y\mid x)p(x)\log q(y\mid z) -\beta\int dxdz p(x)p(z\mid x)\log \frac{p(z\mid x)}{q(z)}\\

&=\int dxdydz p(z\mid x)p(y,x)\log q(y\mid z) -\beta\int dxdydz p(z\mid x)p(x,y)KL\Big(p(z\mid x)||q(z)\Big)\\

&=\mathbb{E}_{(x,y)\sim p(x,y), z\sim p(z\mid x)}\bigg[\log q(y\mid z)-\beta KL\Big(p(z\mid x)||q(z)\Big)\bigg]\\

&=J_{IB}

\end{align}$$

  

To compute the lower bound in practice make the following assumptions:

  

* We approximate $p(x,y)=p(x)p(y\mid x)$ using the empirical data distribution $p(x,y)=\frac{1}{n}\sum^{n}_{i=1}\delta_{x_i}(x)\delta_{y_i}(y)$ such that

$$\begin{align}

J_{IB}&= \int dxdydz p(z\mid x)p(y\mid x)p(x)\log q(y\mid z) -\beta\int dxdz p(x)p(z\mid x)\log \frac{p(z\mid x)}{q(z)}\\

&\approx \frac{1}{n}\sum^{n}_{i=1}\bigg[\int dz p(z\mid x_i)\log q(y_i\mid z)-\beta\int dz p(z\mid x_i)\log \frac{p(z\mid x_i)}{q(z)}\bigg]\\

&=\frac{1}{n}\sum^{n}_{i=1}\bigg[\int dz p(z\mid x_i)\log q(y_i\mid z)- \beta KL\Big(p(z\mid x_i)|| q(z)\Big) \bigg]

\end{align}$$

  

* By using an encoder parameterised as multivariate Gaussian

$$p_\phi(z\mid x)=\mathcal{N}\bigg(z;\boldsymbol{\mu}_\phi(x), \boldsymbol{\Sigma}_\phi(x)\bigg)$$

then we can use the reparameterisation trick such that $z=g_\phi(\epsilon,x)$ which is a deterministic function of $x$ and the Gaussian random variable $\epsilon\sim p(\epsilon)=\mathcal{N}(0,I)$.

  

* We assume that our choice of parameterisation of $p(z\mid x)$ and $q(z)$ allow for computation of an analytic KL-divergence,

  

Thus the final objective we would **minimise** is

$$J_{IB}=\frac{1}{n}\sum^{n}_{i=1}\Bigg[\beta KL\Big(p(z\mid x_i)|| q(z)\Big) - \mathbb{E}_{\epsilon\sim p(\epsilon)}\Big[\log q\big(y_i\mid g_\phi(\epsilon,x)\big)\Big]\Bigg]$$

where we have that

* $p_\phi(z\mid x)$ is the encoder parameterised as a multivariate Gaussian

$$p_\phi(z\mid x)=\mathcal{N}\bigg(z;\boldsymbol{\mu}_\phi(x), \boldsymbol{\Sigma}_\phi(x)\bigg)$$

* $q_\theta(y\mid z)$ is the decoder parameterised as an independent Bernoulli for each element $y_j$ of $y$ (for binary data)

$$q_\theta(y_j\mid z)=\text{Ber}\Big(\mu_\theta(z)\Big)$$

* $q(z)$ is the approximated latent marginal often fixed to a standard normal.

$$q_\theta(z)=\mathcal{N}\Big(z;\mathbf{0},\mathbf{I}_k\Big)$$

  

By using our parameterisation of the decoder $q_\theta(y\mid z)$ as an indepenedent Bernoulli we have that

$$-\log q_\theta(y\mid z)=-\Big[y\log \hat{y} + (1-y)\log(1-\hat{y})\Big]$$

i.e. this is the Binary Cross Entropy loss.

  

### Connection to Variational Autoencoder

The VAE is a special case of an unsupervised version of VIB with $\beta=1.0$ as they consider the objective

$$L=I(x;z)-\beta I(i;z)$$

where the aim is to take our data $x$ and maximise the mutual information contained in some encoding $z$, while restricting how much information we allow our representation to contain about the identity of each data element in our sample $i$. While this objective takes the same mathematical form as that of a Variational Autoencoder, the interpretation of the objective is very different:

* In the VAE, the model starts life as a generative model with a defined prior $p(z)$ and stochastic decoder $p(x|z)$ as part of the model, and the encoder $q(z|x)$ is created to serve as a variational approximation to the true posterior $p(z|x) = \frac{p(x|z)}{p(z)p(x)}$.

* In the VIB approach, the model is originally just the stochastic encoder $p(z|x)$, and the decoder $q(x|z)$ is the variational approximation to the true $p(x|z) = \frac{p(z|x)p(x)}{p(z)}$ and $q(z)$ is the variational approximation to the marginal $p(z) =\int dx p(x)p(z|x)$.


## VIB in reinforcement learning

We assume the supervising signal Y in RL to be the accurate value $R_t$ of a specific state $X_t$ for a fixed policy $\pi$ , which can be approximated by an n-step bootstrapping function

### References

* Original Deep VIB paper: https://arxiv.org/abs/1612.00410