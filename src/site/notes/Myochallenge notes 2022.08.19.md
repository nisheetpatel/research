---
{"dg-publish":true,"permalink":"/myochallenge-notes-2022-08-19/","created":"","updated":""}
---

# Myochallenge
Ideally, we would like to pre-train our model with imitation learning. We agreed that, if this is possible, it would annihilate the competition (unless they're using this as well).

## Imitation learning pre-training
### Behavioral Cloning
- Simplest form of imitation learning which I doubt would work in this case

### GAIL-like approaches
- Probably what we should try. 
- Lookup state-of-the-art approaches with code available
- Also [Behbahani et al. 2019](https://arxiv.org/abs/1811.03516) and [Li et al. 2020](https://ieeexplore.ieee.org/document/9231805)


> [!WARNING] Potential issues
> 1. Unlabelled observation space
>    - Biggest hurdle here would be if the Myochallenge observation space is unlabelled. If we don't know that, it may be impossible.
> 2. Going from video to demos
> 3. Other simpler problems to solve
>    - Using the correct state-space
>    - Using muscular action-action
>      - could we circumvent this by using measures of similarity in state-space?

---

## Classic RL-based
### SAC
- Currently implemented by Alberto

### Reward shaping
- Currently done for keeping balls stable

### HER
- Could be super useful; has worked very well in the past for motor learning
- Pablo is looking into this

### Prioritized experience replay
- Also currently implemented, but simply with `True` flag
- We need figure out the details of how it is implemented in the library 
	- **Key idea**: 
		- as long as the agent has a few successful episodes (e.g. 200 reward points w reward shaping of keeping balls stable), if you just ask the agent to use those trajectory to learn, then it should surely be able to learn.
		- some easy ways to do this:
			- reduce the size of prioritized experience replay buffer as soon as agent experiences some rewarding trajectories

### Curriculum learning 
#### w auxiliary tasks
- Simple task implemented by Alberto of keeping the balls from falling

#### by slowly increasing trajectory speed/length
- ??

---

## Half-baked ideas
### Hierarchical approaches
- Learn a bunch of policies for different tasks and have a higher-level controller decide which ones to deploy
	- reminiscent of the options framework
- Estimate speed of balls and have the controller modify the speed of the learned periodic policy based on the speed of the balls

##### Option Critic w automatic option discovery
- Doina Precup et al.'s Option critic
- Mark Bellemare et al.'s Automatic option discovery
- My man Sergey Levine of course has tons of other approaches for automated option discovery. Check them out if following this route

### Add momentum to action space
When training the musculoskeletal arm, the (RL) actor  may initially be trying really dumb things like flip-flopping between *contracting* and *expanding* muscles at each time-step. We know that this is going to look like a seizure and never going to be useful. 

Could we constrain the actor to have some momentum in its actions?
- Each dimension does this independently
- Could be as simple as adding the previous (continuous) action multiplied by a small constant (Euler method for linear differential equations)

### History of states w attention based transformer
Not entirely sure what this was about, but Alberto mentioned it to Pablo and they were discussing it. I was taking notes and looking up other things, but I heard the words "cool" and "very cool" thrown around, so I'm putting it here and maybe one of them can edit it later.

---

## RL libraries
- RL lib
- Baselines
- Dopamine