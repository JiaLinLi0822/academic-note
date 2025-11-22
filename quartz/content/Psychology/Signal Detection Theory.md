# Signal Detection Theory

## Signal Detection Theory: Optimal Decisiono Making

### Measure Model

In standard signal detection theory, for a given decision, there are two possible states of the world, either there is a signal('S') or there is no signal('N'). Observer's task is to determine whether the world is in state 'S' or 'N'

The observer makes an observation of that world state, resulting in a measurement, which is a single number(decision variable ***x***) that summarizes the evidence concerning the state of the world. Note that the decision variable is noisy, that is , it varies from trail to trial even when the stimulus is fixed.

For any given state of the world, we assume that the distribution of $x$ is Gaussian. The variance of this distribution is fixed, not depending on whether a signal is present or not. Thus, the measurement model is
$$
p(x|N) = \frac{1}{\sqrt{2\pi}\sigma}exp[-\frac{(x-\mu_N)^2}{2\sigma^2}]
$$

$$
p(x|S) = \frac{1}{\sqrt{2\pi}\sigma}exp[-\frac{(x-\mu_S)^2}{2\sigma^2}]
$$

Signal typially lead to larger value of $x$ than when no signal is present, that is $\mu_N > \mu_N$

<img src="/Users/lijialin/Library/Application Support/typora-user-images/Screenshot 2024-11-19 at 10.26.28 PM.png" alt="Screenshot 2024-11-19 at 10.26.28 PM" style="zoom: 33%;" />

### Observer's perspective and the normative model

The measurement model describes the situation with which an observer is confronted. The world is either in state $S$ or $N$ and provides the observer with a noisy measurement $x$. The observer knows the value of $x$ and wants to infer whether the state of the world is $S$ or $N$.

For decision makers, they knows the value of $x$. What they don't know is the true state of the world(i.e., $S$ or $N$). Thus, from the decision-maker’s perspective,  $p(x|S)$ is the probability of getting the measurement $x$ (which the observer knows, and therefore is no longer random) given a particular state of the world *S* (which the observer doesn’t know). When a conditional probability is regarded in this way (the value to the left of the “|” is known and fixed, and the value after it is unknown and to be estimated or decided upon), it is referred to as a ***likelihood***

What decision should the observer make?

#### ML rule

A simple decision procedure is to choose the world state that is more likely, that is **(Fig. B)**

* Say 'yes' if $p(x|S) > p(x|N)$

* Say 'no' otherwise

That is called **the maximum-likelihood(ML) observer**. More concretely**(Fig. C)**

* Say 'yes' if $x>\frac{\mu_S+\mu_N}{2}$

* Say 'no' otherwise

In this rule, the observer should compare the evidence to a fixed criterion(here is $c$)

#### MAP rule

The ML rule may seem a little ad hoc, since likelihood is the probability of something you already know to be true (the measurement). Thus, one might prefere to adopt the following decision rule

* Say 'yes' if $p(S|x) > p(N|x)$

* Say 'no' otherwise

Note that the only difference between this decision rule and the decision rule above is the order of conditional probability. Here we can apply Bayes rule to each term above:
$$
P(S|x)=\frac{p(x|S)P(S)}{p(x)}
$$

$$
P(N|x)=\frac{p(x|N)P(N)}{p(x)}
$$

Here $P(S)$ and $P(N)$ is the prior probabilities, which are the probabilities of the two possible state of the world prior to collecting the evidence $x$.  $P(S|x)$ and $P(N|x)$ is the posterior probabilities, that is, they are the probabilities of the two possible states of the world *after* the measurement is made. 

Thus, the decision procedure above is known as **the maximum a posteriori(MAP) rule**, as it chooses the state of the world with maximum posterior probability. The term in the denominators in Eq.3 and Eq.4 is a nuisance term that ensures that $P(S|x) + P(N|x) = 1$. 

Actually, we don't need to calculate the denominators. By substituting Eq.3 and Eq.4 to the decision rule above and rearrange them, the decision rule becomes:

* Say 'yes' if $\frac{p(x|S)}{p(x|N)}>\frac{p(N)}{p(S)} = \beta_{opt}$
* Say 'no' otherwise

The value on the left-hand side is called a ***likelihood ratio***.  It is the ratio of the values of the two curves above the measurement. The right-hand side is called the prior odds, and provides a criterion, $\beta_{opt}$, that the likelihood ratio must exceed to say 'yes'.

If the prior odds are equal to one, in this case that is no-signal and signal trials are equally likely to occur, then MAP rule yields the same decision procedure as ML rule.

Fig. 1D illustrates the value of the likelihood ratio $\beta$ as a function of evidence $x$. As you can see, the likelihood ratio increases monotonically as a function of *x*. We denote $c_{opt}$ as the criterion to say 'yes', where the likelihood ratio is $\beta_{opt}$. Thus, we can rewrite the decision rule as follows:

* Say 'yes' if $x>c_{opt}$
* Say 'no' otherwise.

Note that for the MAP procedure, the optimal criterion $c_{opt}$ will depend on the means($\mu_S$ and $\mu_N$), the common standard deviation $\sigma$ and the prior odds

<img src="/Users/lijialin/Library/Application Support/typora-user-images/Screenshot 2024-11-20 at 11.11.55 AM.png" alt="Screenshot 2024-11-20 at 11.11.55 AM" style="zoom: 33%;" />

There are two possible tstates of the world and two possible decision outcomes, which are named as follows:

|                     |      |  Decision   |                   |
| :-----------------: | :--: | :---------: | :---------------: |
|                     |      |     Yes     |        No         |
| State  of the world |  S   |     Hit     |       Miss        |
|                     |  N   | False Alarm | Correct Rejection |

#### Maximum Expected Gain

Human performance can be compared to predicted optimal performance to determine human efficiency at a given task. For signal detection theory, we can develop the ideal observer as well.

FIrst of all, we need to decide on a 'cost function'. In this case, it represents that the observer is aware of the value of each possible decision outcome:

|          |      | Response    |            |
| -------- | ---- | ----------- | ---------- |
|          |      | Y           | N          |
| Stimulus | S    | V(S, 'yes') | V(S, 'no') |
|          | N    | V(N, 'yes') | V(S, 'no') |
|          |      |             |            |

Here, the values of this payoff matrix might be in units of monetary payoff or in units of psychological utility. Typically, the values associated with correct answers(hits, correct rejections) are positive and the other two values(False Alarm, Miss) are negative.

The ideal observer is supplied with a measurement $x$ and maps that measurement ot a response('yes' or 'no') by choosing the response that maximie the expevted gain. The expecyed gain of each response depends on the various probabilities and associated values:
$$
E[V(yes|x)] = V(S, yes)P(S|x)+V(N,yes)P(N|x)
$$

$$
E[V(no|x)] = V(S, no)P(S|x)+V(N,no)P(N|x)
$$

That is, the expected value of each response is equal to a sum over possible states of the world of the probability of that state given the evidence times the value of that response in that world state. Thus, the ideal observer responds 'yes' when $E[V(yes|x)]\geq E[V(no|x)]$. Now we can substitute Eq(5) and Eq(6) into this inequality and rearranging, we find that the ideal observer should

* Say 'yes' if $\frac{P(S|x)}{P(N|x)}\geq \frac{V(N,no) - V(N,yes)}{V(S,yes)-V(S,no)}$

* Say 'no' otherwise 

Note that the left side of the inequality is still posterior odds(compared to the MAP rule above). That is to say, the ideal observer says 'yes' when the posterior odds exceeds a criterion dereived from the payoff matrix. This criterion is the excess value of being correct on no signal trials divided by the excess value of being correct on signal trials.

Also, we can make the similar substitutions using Bayes rule, the decision rule becomes:

* Say 'yes' if $\frac{P(x|S)}{P(x|N)}\geq \frac{P(N)}{P(S)}\frac{V(N,no) - V(N,yes)}{V(S,yes)-V(S,no)} = \beta_{opt}$

* Say 'no' otherwise 

This is the decision rule based on the likelihood ration(compared to the ML rule above).

### Standardized Model

As we can see, all of the derivation of the normative model above are based only on the responses rates(i.e., hit, false alarm etc.) and the likelihood ratio. None of these values will change with a change of variables for the decision variable involving ahorizontal shift or a rescaling of the decision variable. Thus, we can apply a transformation so that the decision axis is in units of z-score for the no-signal distribution:
$$
y=\frac{x-\mu_N}{\sigma}
$$
This change of variables leads to the following model:
$$
p(y|N)=\frac{1}{\sqrt{2\pi}}exp[-\frac{y^2}{2}]
$$

$$
p(y|S)=\frac{1}{\sqrt{2\pi}}exp[-\frac{(y-d')^2}{2}]
$$

$$
d'=\frac{\mu_S-\mu_N}{\sigma}
$$

Here, $d'$ is a ratio between the strength the signal and the standard deviaition of the noise. That is, ***signal-to-noise ratio***. The model above is called standarized model since the mean of no-signal distirbution is zero, and the standard deviation of signal and no-signal distribution is 1**(Fig 2)**.

<img src="/Users/lijialin/Library/Application Support/typora-user-images/Screenshot 2024-11-21 at 1.43.12 PM.png" alt="Screenshot 2024-11-21 at 1.43.12 PM" style="zoom: 33%;" />

All three decision rules(ML, MAP and maximum expected gain) result in a criterion value of the likelihood ratio. For the standardized model, the relationship between the decision variable $y$, and the $\beta$ is particularly simple, showing that a criterion $\beta_{opt}$ on likelihood ratio corresponds to the decision variable $y$
$$
\beta_{opt}=\frac{p(y|S)}{p(y|N)}=\frac{exp[-\frac{(y-d')^2}{2}]}{exp[-\frac{y^2}{2}]}=exp[yd'-\frac{d'^2}{2}]
$$
We can set a criterion $c_{opt}$ to distinguish signal from no signal, Thus,
$$
\beta_{opt}=exp[c_{opt}d'-\frac{d'^2}{2}]
$$
We can solve the $c_{opt}$ by doing log transformation on Eq.12. Thus, we have:
$$
c_{opt}=\frac{d'}{2}+\frac{log\beta_{opt}}{d'}
$$
The optimal criterion on the decision variable is determined by, and monotonically increases with the optimal criterion on the likelihood ratio. If one requires a large likelihood ratio to say “yes”, that will lead to a large, conservative criterion on the decision variable. 

Similarily, for maximum expected gain model, $c_{opt}$ is determined by(substitutie $\beta_{opt}$ by Maximum expected gain decision rule):
$$
c_{opt}=\frac{d'}{2}+\frac{1}{d'}[log\frac{P(N)}{P(S)}+log\frac{V(N,no) - V(N,yes)}{V(S,yes)-V(S,no)}]
$$
The effects of priors and payoffs on the optimal criterion are additive.

With the standardized representation of signal detection theory (Fig. 3), the main benefit of signal detection is made clear: *the distinction between discriminability and bias*. The stronger the signal, the more accurately one can perform the task. In this representation, signal strength or discriminability corresponds to the separation between the two distributions $d'$. On the other hand, given a fixed amount of information (fixed *d*′), the bias toward responding “yes” or “no” is reflected in the position of the decision criterion and may be defined for the normative model as $c_{opt}-c_{ML}$, where $c_{ML}=\frac{d'}{2}$ is the neural, that is, the maximum-likelihood criterion. The optimal criterion, $c_{opt}$ , is determined by the priors and payoffs. If the payoffs are symmetric (equal benefits for hits and correct rejects, equal penalties for false alarms and misses), then the optimal behavior is MAP. If, in addition, there are equal priors ($P(S)=P(N)=0.5$ ), the optimal behavior is ML.

## Data Analysis: The Experimenter's Perspective

### Parameter estimation from data

We now examine signal detection theory from the perspective of the experimenter. From this perspective, the goal is to use behavioral data to infer something about how an observer’s decisions were made. In the case of signal detection theory, an experimenter might like to infer the model parameters from data.

Note that the full model includes details of the stimulus encoding($\mu_S, \mu_N, \sigma$), the prior($P(S)$) and the payoff matrix. However,  given that the general encoding full model is derived form the standardized model, the only parameters that we can estimate are $d'$ and $c$.

Looking at Fig.2, we can derive $d'$ and $c$ by
$$
d' = z[P(Hit)+z[P(Correct \ reject)]] \\
= z[P(Hit)+z[P(False \ alarm)]]
$$

$$
c=z[P(Correct \ reject)]
$$

From the perspective of data analysis, we can replace the theoretical probabilities  in Eq.15 and Eq.16 with their empirical estimates. Here, *P*(Hit) is replaced by the proportion of signal trials in which the observer’s response was “yes” and *P*(Correct reject) is replaced by the proportion of no-signal trials in which the observer responded “no”, etc.

### The psychometric function, varying signal strength

#### The ROC curve

The graph shown here are referred to as 'Receiver Operating Characteristics' or ROC curves. They illustrate the tradeoff between correct 'yes' answer(Hit) and incorrect 'yes' answers(False alarm) as the criterion varies. Each curve corresponds to a particular value of signal discriminability. 

For any given value of *d*′, a liberal (i.e., low) criterion leads to a large hit rate, but also a large false-alarm rate (toward the upper-right of the plot), whereas a conservative (i.e., high) criterion reduces both hit and false-alarm rates.

The negative diagonal on this plot corresponds to $P(hit) = 1-P(False \ alarm) = P(correct \ reject)$


