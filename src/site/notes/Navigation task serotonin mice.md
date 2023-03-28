---
{"dg-publish":true,"permalink":"/navigation-task-serotonin-mice/","created":"","updated":""}
---


## Navigation task

#### Task design on miniworld

Use a long T-maze with a diagonally striped wallpaper. Say the actions available to the agent are discrete, and are up, down, left, right.

##### Visuomotor gain

Define 3 visuomotor gain modes $g$:

- Slow or $g_s=1:3$
	- Action $a$ executed once
- Normal or $g_n=1:1$
	- Action $a$ executed 3x
- Fast or $g_f=3:1$
	- Action $a$ executed 9x

##### Wallpaper phase

Define the phase of the wallpaper as $\theta$, where $\theta\in[0,\ 2\pi]$.


> [!danger] Skip for v0
> I suggest that we skip the wallpaper phase manipulation for our first attempt to make things simpler. It would unnecessarily complicate the task generation and honestly, I'm not 100% certain that it would be possible.


##### Pseudocode

- Sample a random time step $t_\text{switch}$
	- $t\sim U[t_{1/4}, t_{3/4}]$
	- uniformly at random from somewhere between 1/4 and 3/4th the time it takes the agent (empirically) to get to the end of the corridor
- Sample two random task modes
	- $g_0\sim\{g_s,\ g_n,\ g_f\}$
	- $g_\text{switch}\sim\{g_s,\ g_n,\ g_f\}$
- Sample two random phases
	- $\theta_0\sim[0,2\pi]$
	- $\theta_\text{switch}\sim[0,2\pi]$
- At $t_0$, start task with gain $g_0$ and phase $\theta_0$ 
- At $t_\text{switch}$, switch gain to $g_\text{switch}$ and set wallpaper phase at current location $x_{t_\text{switch}}$ to $\theta_\text{switch}$

> [!example] My terrible attempt at defining episodes
> - Regular episode:
> 	- $\left(g_{t=0},\theta_{t=0}\right)$
> - Gain-manipulation episode:
> 	- $\left(g_{t=0},\theta_{t=0},g_{t=t_\text{switch}}\right)$
> - Gain and phase manipulation episode:
> 	- $\left(g_{t=0},\ \theta_{x=0,t=0},\ g_{t=t_\text{switch}},\ \theta_{x=x_{t_\text{switch}},t=t_\text{switch}}\right)$
