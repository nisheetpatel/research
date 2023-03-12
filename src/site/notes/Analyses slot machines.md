---
{"dg-publish":true,"permalink":"/analyses-slot-machines/","created":"","updated":""}
---

# Analyses

## Non-Bayesian

- Pool the data, fit and plot logit for each slot machine
	- For all blocks combined
	- Individually for each block
- Individually for each (subject, SM). Show plot per subject

> [!tip] Important note:
> Pooled data tells us absolutely nothing. For example, if half the subjects follow a frequency-based strategy and the other half follow a stakes-based strategy, the pooled data will show DRA-like trend in the slopes of the psychometric curves. Moreover, we might be capturing effects based on "wisdom of the crowd" which no individual might be following.

## Bayesian

The idea here is to infer the posterior over $\beta_\text{SM}$. Then, sample $\beta_\text{SM}$ from the posterior and check whether $\beta_1 > \beta_{2-3} > \beta_4$ effect is significant.

> [!warning] Caution with similarity tests
> The way we are doing similarity tests at the moment is quite flawed. In 4D space, if we check whether two points are similar with a threshold $\epsilon$, that selects a hypercube. On the other hand, if we check for an ordering as we do for DRA, that splits the space into half. Thus, it is possible that the test for DRA is much less strict compared to the test for the other models.


> [!tip] Alternative suggestion
> The proper way to do this might involve:
> 1. Build separate hierarchical models for each of the assumed strategies, i.e. properly restrict model's $\beta$, for instance, by setting ($\beta_1=\beta_2$, $\beta_3=\beta_4$) for the frequency-based model.
> 2. Compute Bayes factor for the models using the [Savage-Dickey density ratio](https://statproofbook.github.io/P/bf-sddr) since it won't be possible to obtain the model's likelihood.

### Hierarchical logistic regression

#### Across subjects, but not across slot machines

$\mu \sim p(\mu)$			    mean of group
$\sigma \sim p(\sigma)$		        std of group
$\alpha_i \sim \mathcal{N}(\mu, \sigma^2)$	    prior of intercept for each subject
$\beta_i \sim \mathcal{N}(\mu, \sigma^2)$	    prior of slope for each subject


#### Multi-level across subjects & slot machines:

- $i$ = subjects, $j$ = slot machines
- parameters: {$\alpha_{i,j}$, $\beta_{i,j}$}

The model probably looks something like this:

$\nu, \tau \sim p(\nu, \tau)$
$\nu_2, \tau_2 \sim p(\nu_2, \tau_2)$
$\mu_j \sim p(\nu, \tau)$
$\sigma_j \sim p(\nu_2, \tau_2)$
$\alpha_{i,j} \sim N(\mu_j, \sigma_j^2)$

Points to note:

- I should empirically get the parameters $\nu_\alpha$ and $\nu_\beta$ that initialize hyperpriors over $\alpha$ and $\beta$ by fitting a logistic regression curve to the pooled data across all subjects and slot machines. This might be crucial because bad priors can lead to terrible fits.
- The idea, if it is not clear from the equations, is that the highest level priors apply to each subject. Then, each subject's parameters $(\alpha, \beta)$ for the individual slot machines are drawn from their prior over $(\alpha, \beta)$ across all slot machines. This would then capture within-subject covariance of $\beta_j$. This sounds hella complicated at first, but the point is that if half the subjects are frequency-based and the other half are stakes-based, the group-level parameters are probably going to look like DRA, which would give us a false positive.


# Model fitting

Although people may think that RL models cannot really be fit, we can actually fit the individual models to the data, meaning that it is possible to compute the log-likelihood for each model.

Here's how we could do it for one subject, sequentially starting from the first trial:

- get the experience tuple $e$ from the actual subject's data
	- $e = (s, a, r, s')$
- force the model, e.g. DRA, to take the same action as subject
- compute $p(A_t = a | ...)$
- compute log likelihood for dataset:
	- $\log L(\theta, D) = \sum_i \log p(A_t = a | ...)$
- $\theta = \left(\alpha, \beta, \lambda, \sigma_\text{base}\right)$
	- $\alpha$ = learning rate
	- $\beta$ = softmax parameter
	- $\lambda$ = memory cost
	- $\sigma_\text{base}$ = memory baseline uncertainty


> [!info] Good news
> Luigi's group just released the packages to do this in python. We can use them.


# Alternative models

We should try and construct really solid alternative models using state-of-the-art methods to hammer the point that the effect we predicted and are showing is *really* something entirely new, which we would have never gotten to if we did not follow the normative approach. Here are the models we should construct:

### Standard RL model

We should try to show that a standard RL model doesn't fit this data


### Bayesian ideal observer model

The ideal observer would observe the values of each of the slot machines and update its beliefs about the value of the slot machine. However, it has no memory resource allocation built into it, meaning that if it sees something less frequently or if the stakes are not as high (on average), then the only thing constraining the precision with which it encodes the values of the slot machines is lack of data. In the limit of infinite data, these effects should go away, and we would expect this model to look like an equal precision model. With finite data, it may look like a frequency-based model because of the seriously low number of trials for the infrequent slot machines collected per subject.

The standard ones have something like a Gaussian inverse-Gamma prior with a softmax choice.