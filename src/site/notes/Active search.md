---
{"dg-publish":true,"permalink":"/active-search/","created":"","updated":""}
---

[[Half-baked ideas\|Half-baked ideas]]

# Active search
## Initial ideas
### GPTD
Use gaussian process temporal difference learning to learn value function

### O'Donoghue Bayesian value learning
- Might need to decouple updates to mean and standard deviation as in O'Donoghue et al. *ca.* 2016-2018
- 

### Graph Laplacian
- don't learn the value of state representation that keeps maintains distance in original space, e.g. graph laplacian
- could do this for non-tabular MDPs as well

### Acquisition function
- use an acquisition function to *actively* sample points in feature space
	- f(mean value, uncertainty in value)
	- akin to Bayesian optimization

#### Active search
- what points should an agent sample in order to evaluate current decision?
- keep all sampled points in a memory buffer?