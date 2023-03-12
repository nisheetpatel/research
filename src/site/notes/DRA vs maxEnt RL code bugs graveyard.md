---
{"dg-publish":true,"permalink":"/dra-vs-max-ent-rl-code-bugs-graveyard/","created":"","updated":""}
---

[[DRA vs maxEnt RL code\|DRA vs maxEnt RL code]]
tags: #DRA #max-entropy #RL #research #project #inference #code 

# DRA vs max-entropy RL
## Issues and bugs graveyard
#### Conceptual errors and issues
1. ~~Max entropy implementation:~~
	- ~~Should the max entropy agent have the ability to independently set the policy in each state, for example, by policy gradient?~~
	- ~~Read soft q-learning paper to understand this issue.~~
2. Setup and learning of the memory 2AFC task:
	- because of q-learning's optimistic bias issue, some values are never learnt if I initialize q-values to 0 (because agent keeps choosing the higher one)
	- I could circumvent this issue with initial training with a slow learning rate, but it remains problematic for PMT trials!
	- ~~Also, there is the issue of redundant coding of q-values that needs to be solved for this task. The standard option values appear in three spots in the table, and the bonus options should be fixed and known and not need to be learnt. When the agent is initialized, it should be imparted with this knowledge.~~

#### Bugs
1. Maze
	- [x] dimensions are being read and used wrong by the models
	- [x] I suspect the issue is in how q_idx is processed because maze is 2D
2. Memory2AFC
	- [x] Action space is not of type gym.spaces
	- [x] it should be gym.spaces.Discrete(2)
	- [x] conditions should be states, actions should be left, right
	- [x] ==PROBLEM== with this approach
		- [x] Entropy penalization may be wrong!