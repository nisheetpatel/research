---
{"dg-publish":true,"permalink":"/dra-vs-max-ent-rl-code/","created":"","updated":""}
---

[[DRA vs Max Ent RL\|DRA vs Max Ent RL]]
tags: #DRA #max-entropy #RL #research #project #inference #code

# DRA vs max-entropy RL
## Code
- [[code refactoring DRA vs maxEnt RL\|code refactoring DRA vs maxEnt RL]]

## Ideas for further refactoring
### Tasks
##### Issues with Bottleneck
- [x] When going to second stage, indexing gets messed up because `next_state` returned to agent is 17 but real `next_state` is 0
- [ ] For states 1-6, MaxEnt agent should be updating the option-value of 0, not 1-6.

##### Make all environments RL style
- [x] Memory2AFC non-sequential with a transition matrix
- [x] Bottleneck task should pass environment protocol
- [x] Huys task should pass environment protocol
- [x] Build indexer for Bottleneck task

##### Send index info in info output from step
- [ ] See if it is a good idea to kill indexers and instead send index info in the info output from ```env.step()```
	- this might need q-tables to be initialized as matrices of size n_states $\times$ n_states
	- even this may not work for Bottleneck, Memory2AFC, Huys in their current format since they are not true MDPs

##### Other
- [ ] See if it is possible to implement generic step function

### Models
- [ ] See if it is possible to combine max-entropy models after making the Memory2AFC task non-sequential

### Training
- [ ] reduce sizes of run_episode() and update_agent_noise() by introducing other functions
- [ ] think about collecting experience tuples and updating later
- [ ] separate Simulator into EpisodeSimulator, Trainer, etc.

## Issues and bugs graveyard
[[DRA vs maxEnt RL code bugs graveyard\|DRA vs maxEnt RL code bugs graveyard]]