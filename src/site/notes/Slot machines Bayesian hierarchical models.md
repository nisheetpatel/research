---
{"dg-publish":true,"permalink":"/slot-machines-bayesian-hierarchical-models/","created":"","updated":""}
---


#DRA #human-memory #slot-machines 

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
