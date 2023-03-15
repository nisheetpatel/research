---
{"dg-publish":true,"permalink":"/serotonin-task-meeting-with-zach/","created":"","updated":""}
---


Felix, Gonzalo, Laurena, Nisheet, Zach


<div class="transclusion internal-embed is-loaded"><a class="markdown-embed-link" href="/navigation-task-serotonin-mice/" aria-label="Open link"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="svg-icon lucide-link"><path d="M10 13a5 5 0 0 0 7.54.54l3-3a5 5 0 0 0-7.07-7.07l-1.72 1.71"></path><path d="M14 11a5 5 0 0 0-7.54-.54l-3 3a5 5 0 0 0 7.07 7.07l1.71-1.71"></path></svg></a><div class="markdown-embed">





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


</div></div>


## Moving MNIST task

> [!bug] Plot predictions instead of reconstruction
> Instead of plotting the single trial video frames $x_1, ..., x_t$ and it reconstruction, i.e. $p(\hat{x}_t|z_t)$, we should plot the prediction given the history of observations and/or latents, i.e. $p(\hat{x}_t|\hat{z}_t) p(\hat{z}_{t}|z_{t-1},h_{t-1},a_{t-1})$. 
> 
> In other words, use the MDN-RNN to predict the next latent and use the VAE decoder to generate an observation based on that. Doing this will demonstrate exactly what happens at the time of the glitch in a human-interpretable manner.

> [!tip] Sort neurons in plots
> For the position glitch, direction glitch, digit glitch, etc., sort the neurons (latent dimensions) by their magnitude of response to each glitch at the time of the glitch. Maintain this sorting order across all glitches and then plot all of them together. This will help showcase the diversity of responses to the different features of the SPE in a visually appealing manner.

#### Task manipulations

- [ ] Speed glitch
- [ ] Bounce glitch
- [ ] Digit glitch with same digit class

For the last one, we would need to build a classifier that samples MNIST digits, classifies them into their respective digit classes, and then samples them appropriately. A little tedious, but very doable.

#### Non-learnable glitches: Random Pixel noise

We demonstrate the state prediction error (SPE) effect on several task manipulations, each of which evokes a varying magnitude of the SPE signal. The point we would like to make is that this prediction error is in the latent state space, which is highly structured, as opposed to the pixel space, which is kind of meaningless. If we can show that the random errors in pixel space, which are unpredictable, do not evoke a strong response as the structured errors do, that might be a nice figure to have. If we can control the magnitude of the noise (with the pixel )


> [!question] Low priority?
> I reckon that this point is lower priority than getting the hallway task working, but I do agree that it may be an important point to make for the neuroscientists out there.


## Other points of discussion

#### Points to emphasize in the article

Serotonin response captures:

1. a low-D (but not 1D) latent state prediction error signal
2. only to *attended* or *relevant* features of state
3. that is modulated by learning

Points 2 & 3 are subtle. In the model, this may only be demonstrated by restricting the training data in a manner that we can or cannot define what is *relevant* for the agent in its self-supervised learning task.

#### References

Added to [the Zotero library](https://www.zotero.org/groups/4868946/the_role_of_serotonin):

- Ranade & Mainen 2009
	- shows the multiplicity of responses of DRN neurons
	- low-D but not one-D signal
- Glascher, Daw, Dayan, and O'Doherty 2010
	- State prediction errors linked to Dopamine