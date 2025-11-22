![[Screenshot 2025-05-01 at 1.11.16 PM.png]]

- In order to backpropagation
![[Screenshot 2025-05-01 at 1.12.17 PM.png]]

### Definitions

- $\mathbb{E}$ : Expectation
- $p$: Density
	- $\mathbb{E}_{p(x)}[f(x)] = \int_x p(x)f(x)dx$
- $\nabla$: gradient


Goal: minimize $\mathbb{E}_{p(z)} [f_\theta(z)]$

$$
\begin{align}
\nabla_\theta \mathbb{E}_{p(z)} [f_\theta(z)] 
&= \nabla_\theta \left[ \int_z p(z) f_\theta(z) \, dz \right]\\
&= \int_z p(z) [\nabla_\theta f_\theta(z)] dz \\
&= \mathbb{E}_{p(z)} [\nabla_\theta f_\theta(z)]
\end{align}

$$

Minimize  $\mathbb{E}_{p_\theta(z)} [f_\theta(z)]$
$$
\begin{align*}

\nabla_\theta \mathbb{E}_{p_\theta(z)} [f_\theta(z)] &= \nabla_\theta \left[ \int_z p_\theta(z) f_\theta(z) \, dz \right] \\

&= \int_z \left[ \nabla_\theta p_\theta(z) f_\theta(z) \, dz \right] \\

&= \int_z \left[ f_\theta(z) \nabla_\theta p_\theta(z) \, dz \right] + \int_z \left[ p_\theta(z) \nabla_\theta f_\theta(z) \, dz \right] \\

&= \int_z \left[ f_\theta(z) \nabla_\theta p_\theta(z) \, dz \right] + \mathbb{E}_{p_\theta(z)} [\nabla_\theta f_\theta(z)]

\end{align*}
$$


$$
\begin{align*}

g_\theta(\epsilon, x) &= z \\

\epsilon &\sim \mathcal{N}(0, 1) \\

\mathbb{E}_{p_\theta(z)} [f_\theta(z)] &= \mathbb{E}_{p(\epsilon)} [f_\theta(g_\theta(\epsilon, x))] \\

\nabla_\theta \mathbb{E}_{p_\theta(z)} [f_\theta(z)] &= \nabla_\theta \mathbb{E}_{p(\epsilon)} [f_\theta(g_\theta(\epsilon, x))] \\

&= \mathbb{E}_{p(\epsilon)} \left[ \nabla_\theta f_\theta(g_\theta(\epsilon, x)) \right]

\end{align*}
$$
