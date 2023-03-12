---
{"dg-publish":true,"permalink":"/regularization-in-rl/","created":"","updated":""}
---

[[DRA vs Max Ent RL\|DRA vs Max Ent RL]]

## Regularization in RL

- DRA's KL
- [Todorov's KL](https://www.pnas.org/doi/10.1073/pnas.0710743106)
	- Abstracts away from actions
	- KL on divergence term is of control dynamics from default dynamics
- [Linear RL](https://www.nature.com/articles/s41467-021-25123-3)
	- RL version of Todorov's work where dynamics are replaced by policies
- [SAC](https://arxiv.org/abs/1801.01290)/maxEnt RL
- Moskovitz+Botvinick's regularization 
	- unpublished
	- has two regularizing terms:
		1. the first term is the same as Linear RL, a KL between behavioral policy and default policy, which is learned here unlike linear RL/Todorov
		2. the second term is a complexity (max description length) penalty on the default policy
	- has an interesting information-theoretic story and easy implementation thanks to the legendary Kingma: both these terms are what come out of [variational dropout](https://arxiv.org/pdf/1506.02557.pdf)
- [Noisy Nets](https://arxiv.org/abs/1706.10295)
	- Noisy weights in a DQN
	- maybe only related to DRA and none of the others

## Ideas linked to DRA vs MaxEnt

- Read [lilian weng's blog on policy gradient algorithms](https://lilianweng.github.io/lil-log/2018/04/08/policy-gradient-algorithms.html)
	- DPG
	- DDPG
	- TRPO
	- PPO
	- SAC
- Read [noisynets](https://arxiv.org/abs/1706.10295) and [plappert et al 2017](https://arxiv.org/abs/1706.01905)