---
{"dg-publish":true,"permalink":"/code-dra-vs-max-ent-rl-refactored-v0/","created":"","updated":""}
---

[[DRA vs Max Ent RL\|DRA vs Max Ent RL]] [[code DRA vs maxEnt RL original\|code DRA vs maxEnt RL original]]
tags: #DRA #max-entropy #RL #research #project #inference #code


## Refactored code v0
### Generic functions
- softmax
- kl_mvn
- train

### Agent class
#### Methods
- act
- update mean q values
- update noise (sigma)

### Memory Resource Allocation classes
- DRA
- Equal precision
- frequency-based
- stakes-based

#### Methods
##### allocate_resources

### Trainer
#### Methods
##### run_episode
##### compute_expected_reward
##### compute_cost

### where does the train function go??
generic?