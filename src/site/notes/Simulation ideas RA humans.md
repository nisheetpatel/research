---
{"dg-publish":true,"permalink":"/simulation-ideas-ra-humans/","created":"","updated":""}
---

links: [[DRA human experiments\|DRA human experiments]]
tags: #project #science 

# Simulation ideas
- [ ] Automated search through the space of $\{\lambda, \sigma_{base}, \Delta_{PMT} \}$ for which combinations best discriminate the models according to the p-value metric

# Archive: 
## Adaptive delta
- Adaptive Delta
	- [x] Target ~84% choice accuracy for participant
	- [x] Lookup *Continuous* Staircase methods such as the one Luigi suggested over email
	- [x] Email the guy Alex asked to email
	- [x] Get back to them with proper code, work, plots

## Optimization method for alternative models
#### Analytical
- [x] Compute analytical gradient to follow for other models performing resource allocation
- [ ] ~~Implement and compare with numerical (BO)~~

#### Numerical
- Scipy minimize scalar
	- Luigi says that this is really bad for noisy function evaluations
	- It stops as soon as the function value is not greater than a couple of previous counts
- Bayesian optimization
	- Must use this one. However, be careful which package to use
	- Luigi recommends [emukit](https://emukit.github.io/) with MaxEntropyValue acquisition function
		- Beware of other acquisition functions because we have a noisy target
		- If possible, marginalize over GP hyperparameters (see their tutorials)

## Models
- [x] DRA
- [x] Equal precision
- [x] Frequency-based w/ (s,a)
- [x] [[DRA w analytical gradient\|DRA w analytical gradient]]
- [x]  [[Frequency-based w/ (s)\|Frequency-based w/ (s)]]
- [x] [[Stakes-based\|Stakes-based]]

- [x] Model params to run:
	- [x] $\delta\in\{(4,1),(4,2)\}$
	- [x] $\lambda \in \{0.05, 0.1, 0.2\}$
	- [x] $\Delta \in \{0.5, 1, 1.5, 2.5\}$
	- [x] $\sigma_\text{base} \in \{5\}$
	- [x] w/ and w/o learning during PMT
	- [ ] $\delta\in\{(3,1),(5,1),(6,1)\}$
	- [ ] $\sigma_\text{base} \in \{3,7,9\}$
	- [ ] $\lambda \in \{0.5,1\}$

## Results needed
- [x] [[Estimating sigma from PMT trials\|Estimating sigma from PMT trials]]
- [x] Don't average over actions
- [x] Plot p(choose better option) in the space of differences
	- Freq. based:   s1-s2, s3-s4
	- Stakes based: s1-s3, s2-s4
- [x] Multiple experiments w/ same params per model
	- 1 expt = running N subjects (restarts) for T trials
- [x] Multiple experiments w/ diff. params per model