---
{"dg-publish":true,"permalink":"/slot-machines-model-fitting-methods/","created":"","updated":""}
---


#methods #DRA #human-memory #slot-machines #VBMC #fitting

Meeting with Luigi on April 6, 2023.

# Model fitting

## Likelihood for RL models

Our goal is to do statistical model fitting, i.e. find the likelihood of the data $D$ given the model $M$ and the model's parameters $\theta$.

$$p\left(D|M;\theta\right)$$

In our case, we will be fitting RL models, which people tend to think are not possible to get a likelihood for. However, that is not true at least in our case. First, it is important to note that there is no latent variable in our task that we have to marginalize over while computing the likelihood. Second, although the models are not entirely deterministic, it is possible to compute (in closed form) the probability of the subject's response on any trial. This quantity would be the likelihood on trial $t$:

$$p(a_t|o_t,M,o_{1:t-1},a_{1:t-1};\theta_t)$$

Then we simply multiply the likelihoods for all the trials sequentially (or add the log-likelihoods) while updating the model parameters with the relevant update rules, and voila, we have our desired quantity.


> [!tip] Eliminate the stochasticity in RL updates
> For the models that do some form of resource allocation, the updates to $\sigma$ (std. dev. of encoded memories) involve a sampling operation. However, for two alternatives, it is possible to do this in closed form with [[DRA analytical gradient for 2 options\|DRA analytical gradient for 2 options]]. Not only will this speed everything up, but it will allow computing the likelihood on any trial in closed form.


## Model fitting with pyVBMC

Now that we have a way to compute the log-likelihood, we can use [Luigi's pyVBMC package](https://github.com/lacerbi/pyvbmc) to find the model parameters that maximize the likelihood. VBMC takes in as input the log-likelihood function, the parameter vector, and spits out the posterior over parameters that maximize the likelihood.

Here are a few other things that pyVBMC requires:
1. starting point for parameters: 
	- A good practice is to run [pyBADS](https://github.com/acerbilab/pybads) a few (3-4) times which gives the *maximum aposteriori* (MAP) estimate, and use this as the starting point for pyVBMC. 
2. Bounds for parameters:
	- hard bounds beyond which no search will be performed
3. Plausible bounds for parameters:
	- where you expect the parameters to lie
4. Prior over parameters:
	- could initialize this around plausible bounds



# Models

1. DRA
2. Frequency
3. Stakes
4. Equal Precision
5. Standard RL
6. (Bayesian) ideal observer model

## RL models

The RL models (1-5) are already well-defined. To compute the likelihood and fit these, refer to the previous sections: [[Slot machines model fitting methods#Likelihood for RL models\|#Likelihood for RL models]] and [[Slot machines model fitting methods#Model fitting with pyVBMC\|#Model fitting with pyVBMC]].

## Ideal observer model

We would like to construct a couple of Bayesian observers, one which is like an oracle or a truly ideal observer, and a more realistic Bayesian observer.

These observers will have an independent Gaussian for the value of each slot machine with a prior $\mathcal{N}(\mu_0^{[i]},\sigma_0^{[i]})$, where ${[i]}\in\{1,...,4\}$ indexes the slot machine. The goal is to infer the mean value of each slot machine $\mu^{[i]}$ (and possibly also the subject's noise $\sigma^{[i]}$).

##### Models

1. Infer $\mu$ with fixed $\sigma=\sigma_0$
2. Infer $\mu$ with fixed but unknown $\sigma$
	- Like 3, but $\delta$ prior over $\sigma$
3. Infer $\mu$ and $\sigma$
	- Use normal inverse Wishart conjugate (or inverse gamma?) so updates are done in closed form

1 & 2 are expected to be bad, but 3 might be interesting.

##### Action models

For each of the models above, we could have one of the following action-selection models:

1. Bayesian decision theory
	- if $\mu^{[i]}>p_t$, where $p_t$ is price on trial $t$, say yes with probability 1
2. Softmax
	- $p(\text{yes}) = softmax\left(\left(\mu^{[i]}-p_t\right); \beta\right)$
3. Thompson sampling

1 will be terrible, but 2 and 3 might be interesting.