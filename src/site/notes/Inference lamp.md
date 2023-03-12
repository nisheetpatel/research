---
{"dg-publish":true,"permalink":"/inference-lamp/","created":"","updated":""}
---


[[DRA vs Max Ent RL\|DRA vs Max Ent RL]]
#research #project #max-entropy #DRA #inference #RL


# Inference lamp
[[@levineReinforcementLearningControl2018\|@levineReinforcementLearningControl2018]] casts the RL problem as a probabilistic inference problem. This is mighty powerful as it enables the use of a wide array of approximate inference tools amongst other things. One of the key points to take away from that work is how a generalization of the reinforcement learning or optimal control problem - maximum entropy reinforcement learning - is equivalent to exact probabilistic inference in the case of deterministic dynamics, and variational inference in the case of stochastic dynamics.

We need to see whether and how we could view DRA under the inference lamp. What would the problem become?


> [!Info] Update
> Look at Luigi's derivation of DRA as inference
