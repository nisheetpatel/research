---
{"dg-publish":true,"permalink":"/myochallenge-phase-2-curriculum/","created":"","updated":""}
---


# To do

## Oct 28th

### Locally running experiments
- [x] Look at all currently running experiments

- [x] For yesterday's runs mark which one worked, which one didn't
	- [x] Kill one that really didn't work
	- [x] Visualize performance of the rest to get a better idea of what's happening
	- [x] Take notes and kill

- [x] For today's runs with 4 vs 16 envs with different hyperparameters
	- [x] Note any and all differences in performance at around 500k and 1M training steps
	- [x] Visualize performance of all of them
	- [x] Consider killing some if they're the same

> [!WARNING] Don't change num_envs
> num_envs = 4 consistently underperformed compared to num_envs=16


### Cluster
- [ ] Pull the git repo again on the cluster
- [ ] Try to run on experiment there

## Other ideas
- [x] Instead of uniformly sampling limit_init_angle, preferentially sample further ones.
	- [x] design beta distribution
	- Seems to hurt performance LOL

## Good networks

### 27 > 18-38-15
#### Config
- Period (10,10)
- size (20,22)
- mass (40,50)

#### Performance
- Pretty good performance with non-random task physics and angle 0.5 pi
	- ('Average len:', 196.64, '     ', 'Average rew:', 1018.0819973088933)
- Scores 750