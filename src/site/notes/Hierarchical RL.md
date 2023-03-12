---
{"dg-publish":true,"permalink":"/hierarchical-rl/","created":"","updated":""}
---

[[PKB MOC\|PKB MOC]]

# Hierarchical RL
Most of what I'm going to list here concerns abstracting time in RL, i.e. using the same algorithms to form models and learn at many different time-scales. Doing this introduces a hierarchy in that there are now many different levels of abstraction at which the same framework applies. Ideally, you want to share the knowledge across the models at the different levels of hierarchy (different time-scales) in a way that you can reuse bits of one part to solve the other parts.

## Options framework
The core idea of the options framework is to formulate options, which are a notion of actions that may extend arbitrarily in time and may be composed of many different lower-level/primitive actions. This was orginally forumated in [Sutton, Precup, and Singh 1999](https://people.cs.umass.edu/~barto/courses/cs687/Sutton-Precup-Singh-AIJ99.pdf). Options consist of three components:
- a policy $\pi: S\times A \rightarrow [0, 1]$
- a termination condition $\beta: S^+ \rightarrow [0, 1]$, and
- an initiation set $I \subseteq S$

An option $\langle I, \pi, \beta\rangle$ is only available in state $s_t$ if $s_t\in I$.

### Papers
- [[Machado et al 2017\|Machado et al 2017]]
- 

# References
- [2019 article summarizing HRL](https://thegradient.pub/the-promise-of-hierarchical-reinforcement-learning/)
- [2020 slides HRL overview](https://drive.google.com/file/d/14uR5pFKok8FwVsJfwlMi1izEo6ot9RxY/view)
	- [Corresponding reddit thread](https://www.reddit.com/r/reinforcementlearning/comments/f7w48f/state_of_the_art_in_hierarchical_reinforcement/)