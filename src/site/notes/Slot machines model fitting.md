---
{"dg-publish":true,"permalink":"/slot-machines-model-fitting/","created":"","updated":""}
---


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
