---
{"dg-publish":true,"permalink":"/roadmap-for-hallway-navigation-task/","created":"","updated":""}
---


# Roadmap for hallway navigation task

##### Resources

- [[Navigation task serotonin mice\|Navigation task serotonin mice]] 
- [[Code design for navigation task data generation\|Code design for navigation task data generation]]

## Implementation

#### v0.1 setup miniworld

(Felix)

- [x] Setup miniworld in a long corridor
- [x] Change wallpaper
- [x] Add options to change the gain
- [x] Add an option to produce the position glitch

#### v0.2 train RL agent + world model

(Felix)

- [x] train RL agent to navigate in the hallway
- [x] generate rollout trajectories
- [x] train VAE with collected rollouts
- [x] train MDN-RNN on the latent dynamics

#### v0.3 reproduce plots from mouse data

(Felix)

- [x] without position glitch
- [x] with position glitch
- [x] effect of learning

#### v0.3 refactor data generation code

(Nisheet)

- [x] Base collector class
- [x] Subclasses for:
	- [x] constant gain
	- [x] changing gain
	- [x] changing gain with glitch
- [x] collect rollouts function
- [x] save trajectories function