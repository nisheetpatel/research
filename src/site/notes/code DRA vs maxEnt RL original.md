---
{"dg-publish":true,"permalink":"/code-dra-vs-max-ent-rl-original/","created":"","updated":""}
---

[[DRA vs Max Ent RL\|DRA vs Max Ent RL]]
tags: #DRA #max-entropy #RL #research #project #inference #code

## Code original format
### Memory Resource Allocator class
#### Attributes
- model
- lmda
- sigma_base
- learn_PMT
- delta_PMT
- delta_1
- delta_2
- learning_rate_sigma
- learning_rate_q
- discount
- n_trajectories
- beta
- gradient
- n_gradient_updates
- update_frequency
- decay
- decay_1
- print_freq
- print_updates
- adapt_delta
- step_size_adapt_delta
- n_trials_adapt_delta

#### Methods
##### softmax
- super general, standalone function
- push out of class

##### act
- necessary

##### compute_norm_factor
- separate types of models into separate classes and implement this independently for each class

##### run_episode
- should be moved out of the class
- responsibility of a trainer object

##### compute_expected_reward
- also move out
- need to run episode to get this

##### kl_mvn
- super general, standalone function
- push out of the class

##### compute_cost
- maybe responsibility of trainer class?

##### minus_objective
- responsibility of trainer class

##### allocate_resources
- separate classes and implement individually for each
- PROBLEM:
	- needs to run episode

##### train
- the sole responsibility of trainer class

#### Properties
##### memory_table