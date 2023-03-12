---
{"dg-publish":true,"permalink":"/myochallenge-notes-2022-09-19/","created":"","updated":""}
---

It seems like curriculum learning does incredibly well (thanks to Pablo's incredible simulations). However, we still have a few noticeable "issues", of which the most prominent seem to be that the solution seems to be neither robust, nor elegant, nor effortless. This seems like a word salad of robot-hater words, but there is one that might actually affect our results in the competition - effort. So, I think we should proceed with the following goals now:
1. Submit the current solution to the leaderboard or at least score them ourselves to gain some perspective of where we stand at the moment.
2. Refine the solution

## Refine the solution
For the network to learn solutions that incorporate finger bending.

### Curriculum learning
#### Within the baoding balls task
Pablo has already implemented a curriculum that covers:
1. Gradually increasing the speed of the rotating balls
2. Reference State Initialization (RSI) for each speed
	- RSI in our case = initializing the ball positions randomly at the beginning of each episode 

One idea could be to include multiple starting hand configurations, e.g. bent fingers, as part of the RSI. Doing this systematically might help a lot.

#### With other myosuite tasks
An alternative way to learn this might be to train the hand on other myosuite tasks to learn useful "skills", for instance, bending fingers, making certain hand poses, holding objects. 

There seem to be at least a couple of ways to proceed here, noted below. But the problem to solve in both cases is some masking/matching of the input/observation space.

###### Use observations from different environments
1. Use "info" from the return of the step function for the different environments.
	- It has a bunch of useful info such as the time, observation dictionary, and state.
	- Once we have the observation dictionary, at least part of it, if not all, is common across the various myosuite tasks, e.g. _hand_qpos, hand_qvel_.
	- This way, we could maybe add additional rewards for solving other tasks.
2. Add or match the additional observations from the different tasks to the observations in the baoding balls task. Then mask certain observations features to solve each task. 

###### Train common policy network on different tasks 
Inspired from this deepmind [paper](https://arxiv.org/pdf/2010.14274.pdf)/[blog](https://sites.google.com/view/behavior-priors) on Behavioral priors for efficient RL. Basically train on multiple tasks, KL-regularize the policy network to be similar to all of them.


### Other approaches
#### Preference comparisons
Human or synthetic preference comparisons.
- Matthew Rahtz's github repository
	- It's old, not maintained, and written in tensorflow
	- Human preferences is only implemented for one environment: Car Racing
	- We would have to rewrite the code into Pytorch
- Imitation library's preference comparisons module
	- Human preferences doesn't exist here, but synthetic does
	- The developers agree that this is the least developed part of the repository: might be buggy?
	- might be easier to write functions/classes to implement human preferences within this one