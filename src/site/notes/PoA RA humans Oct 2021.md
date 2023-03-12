---
{"dg-publish":true,"permalink":"/po-a-ra-humans-oct-2021/","created":"","updated":""}
---

links: [[DRA human experiments\|DRA human experiments]]

# Things to do
## Conceptual
#### Alternative statistical test
- [x] Sezen proposal
	- [x] Luigi said it won't work
- [x] [[Alt stat test DRA humans\|Bayesian Regression Model]]

#### Model likelihood, recovery
Compute likelihood of model given data:
*Option 1*
- [ ] fit params for each of the four models on training set
	- [ ] *how?*
- [ ] For test set, compute for each trial and for each model:
	- [ ] $p(Data|Model) = \int{p(Data|Model,\theta) p(\theta)}$ by sampling from $p(\theta)$?
- [ ] $LL=\sum_{trials} \log{p(D|M)_{trial}}$

*Option 2*
- [ ] fit params for each of the four models on training set
	- [ ] *how?*
- [ ] Compute ($\bar{q}$ and) $\sigma$ update(s) with each model for each trial. Assuming everything else given, simulate the choice on that trial many times to get $p(choice|everything, model)$ for each of the four models.

#### Model fitting
- [ ] Fit params $\Theta={\lambda,\sigma_\text{base}}$ for choice data on training set.


## Code
Problem is that for both of the points below, it would best to use the alternative statistical test, i.e. the Bayesian Regression Model, once I have coded it up such that it is ready to be employed. However, for now, I can implement both with the p-value of independent t-tests for the sake of completeness and on the side, read the book to figure out how the fuck I'm supposed to implement the Bayesian Regression Model.

#### Reduce no. of trials
- [ ] Use best $\Delta_{PMT}$
- [ ] See how far we can push it before the test doesn't separate the models

#### ...when varying subject params
- [ ] Pick a more reasonable range of $\{\lambda, \sigma_{base}\}$
- [ ] Pick $\Delta_{PMT}$ that best discriminates the models for each choice of $\{\lambda, \sigma_{base}\}$
	- [x] For current test, $\Delta_{PMT}=4$ seems to discriminate all models best.
	- [ ] Test this by producing a surface plot in $\{\lambda, \sigma_{base}\}$ classifying regions where each discrete value of $\Delta_{PMT}$ discriminates models best.

#### Adaptive learning rates (ADAM)
Important, but hella tedious.
- [ ] Implement for both q and sigma


### Archive:
#### Instantiate random seed
- [x] implement multiprocessing across runs instead of serial processing when training multiple models

#### Analytical gradient for other models
- [x] [[pseudocode for RA humans#Analytical gradient for other models\|pseudocode for RA humans#Analytical gradient for other models]]
- [x] Freq-based & Equal precision
- [x] Test and check speedup against current implementation

#### Other models
- [x] frequency-based with N(s) instead of N(s,a)
- [x] stakes-based

#### Analytical expected gradient for all models
- [x] replace $\pi$ and $\zeta$ with analytically computed expressions for the expected $\pi$ and $\zeta$ given the agent's choice instead of that for a single sample
- [x] do this for all models
- [x] compare speedup against current implementation

#### ~~Bayesian Optimization alternative~~
- [ ] ~~with max entropy acquisition function~~
- [ ] ~~test against scipy's routine (speed and accuracy)~~
- [ ] ~~test against analytical (speed and accuracy)~~

#### ~~Parallel trajectory sampling~~
- [ ] ~~Sample trajectories in parallel with an episodic memory buffer that tracks s, a, $\zeta$, etc. across time in a giant tensor~~
- [ ] ~~For each trajectory, compute expectation numerically for each (s,a) pair by sampling in parallel multiple actions in each state~~