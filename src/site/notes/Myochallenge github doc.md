---
{"dg-publish":true,"permalink":"/myochallenge-github-doc/","created":"","updated":""}
---

We need two documents. A README.md and docs/summary.md

# Summary of our approach

## Key components of the final model
1. On-policy learning with Recurrent PPO
	- LSTM layers in both actor and critic to deal with partial observations
2. Reference State Initialization (RSI) to guide initial exploration and stabilize learning
3. Curriculum learning to guide and stabilize training throughout
4. (Optional) Hierarchical mixture of ensembles for dealing with specialized tasks in phase 2

### Reference State Initiatlization (RSI)
Aside from the recurrent units in our networks, we used a few key components in our training. The first was Reference State Initialization (RSI). The schematic below ([source](https://bair.berkeley.edu/blog/2018/04/10/virtual-stuntman/)) helps us in summarizing the idea:
![Pasted image 20220925140512.png|200](/img/user/images/Pasted%20image%2020220925140512.png)![Pasted image 20220925140537.png|200](/img/user/images/Pasted%20image%2020220925140537.png)

The key insight here is that if we initialize the balls at various points in the target trajectory, then the model will experience a much higher density of rewards throughout the final trajectory much more efficiently.

### Curriculum learning
Second, we used a curriculum of training that gradually introduced the tougher aspects of the task. 

For phase 1, we increased the rotation speed gradually following the intuition that once the dynamics are learnt by the recurrent units, it should be fairly easy to exploit the same dynamical modes by making rotation speed faster. As one would expect, RSI was only needed at slower speeds since the models were already doing full rotations at faster speeds and hence had already explored all of those. The exact curriculum we used is in the Appendix.

For phase 2, following the same intuition, at slower rotation periods, we gradually introduced domain randomization, i.e. noisy task physics including ball size & mass, friction, and how far the targets spawned from the balls. Once the tasks were learned well at slower speeds, we gradually increased the rotation speeds. Arguably, a large portion of training went into adapting to the noisy physics, and one of the key changes that we had to make in order to be successful at faster rotation periods was to use much larger batch sizes for our networks to step meaningfully in the loss landscape. The full list of hyperparameters are in the appendix.

### Hierarchical mixture of expert ensembles
> [!NOTE] Optional and experimental
> Note that we didn't have enough time to deploy this successfully. In fact, our single model that performs the final task easily scored over 50%. The gains from this approach were marginal and ultimately not necessary for winning the Baoding balls challenge. Regardless, we list it here for completeness.

The final insight we tried to incorporate, albeit not so successfully, is to use a hierarchical model with a base network and specialist networks for subsets of the main task, along with a classifier that we trained to predict the identity of the relevant subset.

There were a couple of observations that led us to choose this approach. Crucially, we noticed that at faster rotation speeds, the networks often "forgot" some aspect of the tasks to maximize performance on the others. Here are some examples:
	- one of the tasks, usually the hold task
	- target positions far from the balls' initial locations
		- this was particularly disruptive in the hold task because the network learned to make the balls roughly follow the vector given by the "target error". Since the targets do not move for the hold task, the balls run into each other when the targets spawn roughly over $0.6\pi$ away from the balls.

Thus, trained a classifier to take in the first $k$ observations and predict the identity of the task (hold vs other), and also trained a hold network to take over from the base network at timestep $k$ to perform the task. Even with this change, the hold network did not perform well at large separations of the initial ball and target positions. So we trained another set of specialist hold networks that were preferentially exposed to targets and balls spawning roughly opposite of each other, and used this ensemble of hold networks with the base network and classifier. 

For the very final submission that scored 55%, we also used an ensemble of base networks (along with the classifier and hold networks).


# Appendix

## Curriculum 
### Phase 1

![Pasted image 20221108120209.png](/img/user/images/Pasted%20image%2020221108120209.png)

1. Hold the balls fixed, initialising them at random phases along the cycle (i.e. RSI, pink)
2. Rotate the balls with period 20, initialising with RSI (orange)
3. Rotate the balls with period 10, initialising with RSI (green)
4. Rotate the balls with period 8, initialising with RSI (blue)
5. Rotate the balls with period 7, initialising at a single fixed position (similar but not exactly the same initialisation as the original task, where there is some small noise in initialisation; purple)
6. Rotate the balls with period 5, initialising at a single fixed position (idem task 5; yellow)
7. Original task for phase 1 (black)

### Phase 2
All the trained models, environment configurations, main files, and tensorboard logs are all present in the `curriculum_steps` folder. We are omitting the figure from this document because it wouldn't be possible to make sense of the single plot. Roughly, we followed  these steps in order:

- Steps 01-14 train the model to rotate the balls in both directions starting from RSI static (hold) by slowly decreasing the period to (4.5, 5.5).
	- This was trained pre-emptively before the environment for phase 2 was released since we figured that one week would probably not be enough to train for phase 2.
- Steps 15 & 16 introduce the non-overlapping targets and balls.
- Step 17 starts retraining at a slower period 25 with slightly non-overlapping balls and targets.
- Steps 18-22 train the model at period 20 by introducing a the fully-noisy task physics and non-overlapping balls & targets (deviating by up to 0.6$\pi$)
- Steps 23-33 decrease the period to 8 and then to (4,6) with the full noise as in phase 2, i.e. final task. Here, we also switched to using a new set of hyperparameters which much bigger batch sizes to average over all the noise across task conditions in the gradient updates.


## Architecture, algorithm, and hyperparameters
### Architecture and algorithm
We use [RecurrentPPO from Stable Baselines 3](https://github.com/Stable-Baselines-Team/stable-baselines3-contrib/blob/c75ad7dd58b7634e48c9e345fca8ebb06af3495e/sb3_contrib/ppo_recurrent/ppo_recurrent.py) as our base algorithm with the following architecture for both the actor and the critic with nothing shared between the two:

obs --> 256 LSTM --> 256 Linear --> 256 Linear --> output

All the layers have ReLU activation functions and the output, of course, is the value for the critic and the 39-dimensional continuous actions for the actor. Here is the table of the (final) hyperparameters that we used:

| Hyperparameter                             | Value                                                      |     |
| ------------------------------------------ | ---------------------------------------------------------- | --- |
| Discount factor $\gamma$                   | 0.99                                                       |     |
| Generalized Advantage Estimation $\lambda$ | 0.95                                                       |     |
| Entropy regularization coefficient         | 3e-5                                                       |     |
| PPO clipping parameter $\lambda$           | 0.2                                                        |     |
| Optimizer                                  | Adam                                                       |     |
| learning rate                              | 2.5e-5                                                     |     |
| Batch size                                 | 1024 (sequential) transitions/env $\times$ 16 envs = 65536 |     |
| minibatch size                             | 1024 (sequential) transitions                              |     |
| state-dependent exploration                | True                                                       |     |
| max grad norm                              | 0.8                                                        |     |

