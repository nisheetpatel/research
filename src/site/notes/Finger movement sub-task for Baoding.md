---
{"dg-publish":true,"permalink":"/finger-movement-sub-task-for-baoding/","created":"","updated":""}
---

# Finger movement sub-task for Baoding balls

Part of the [[Myochallenge standardize curriculum\|curriculum]] for Myochallenge.

## Ingredients needed

> [!WARNING] Biggest hurdle
> This might be really difficult to implement, unfortunately, because we should not really change the observation space, but still somehow reward the agent for the desired movements! So it must be done internally, only through the rewards. 
> 
> This seems super weird because the agent wouldn't have neither the positions, nor the velocities, nor the errors from the desired finger trajectory! But maybe, the agent could learn it by looking at the dimensions corresponding to the target ball trajectories if we phase-lock them to the fingers? 


### Task config

For this to work, we can start with the following task initialization:
1. Hand
	- palm facing upwards as in baoding_v0
	- It might be best to always start from the same position, say when the fingers are all half-bent, such that the target motion is exactly the same.
	- Alternatively, we could try random finger initialization
2. Balls
	- Start the balls lower than the hand so they just fall down with gravity and the remove the episode termination threshold and rewards for balls.
3. Ball goal trajectory
	- If we implement it properly, this will still be part of the observation space
	- We can phase-lock them with the target finger trajectories 
		- Would need to find the correct phase somehow

#### Mujoco visualization

We need to add visual markers for the finger-tip targets without them being part of the observation space.


### State and target information

#### 1. Finger tips ids

Can access them how they do in pose_v0 task

- argument `sites` in `super()._setup()`
```python
sites = dict_keys(['THtip', 'IFtip', 'MFtip', 'RFtip', 'LFtip'])
```
if specified, it does the following in setup:
```python
self.tip_sids = []
self.target_sids = []

if sites:
for site in sites:
	self.tip_sids.append(self.sim.model.site_name2id(site))
	self.target_sids.append(self.sim.model.site_name2id(site+'_target'))
```

We don't need to go through that, since we don't want to add it to the observation space at all actually. Instead, we can set an instance variable for the environment that is not called tip_sids nor target_sids, but still does this:
```python
for isite in range(len(self.tip_sids)):
	self.sim.model.site_pos[self.target_sids[isite]] = self.sim.data.site_xpos[self.tip_sids[isite]].copy()

if restore_sim:
	self.sim.data.qpos[:] = qpos[:]
	self.sim.data.qvel[:] = qvel[:]
	self.sim.forward()
```

##### MujocoEnv setup()

Nothing special happens here actually. As you can see from the code snippet below, everything is controlled internally by means of rewards and observations:

```python
# resolve rewards
self.rwd_dict = {}
self.rwd_mode = reward_mode
self.rwd_keys_wt = weighted_reward_keys

# resolve obs
self.obs_dict = {}
self.obs_keys = obs_keys

# self._setup_rgb_encoders(obs_keys, device=None) # Removed as not using visual inputs
self.rgb_encoder = None # To compensate for above

observation, _reward, done, _info = self.step(np.zeros(self.sim.model.nu))

assert not done, "Check initialization. Simulation starts in a done state."

self.obs_dim = observation.size

self.observation_space = gym.spaces.Box(obs_range[0]*np.ones(self.obs_dim), obs_range[1]*np.ones(self.obs_dim), dtype=np.float32)
```


#### 2. Finger tip target trajectories

Can do this like how the goal trajectories are generated for both balls in baoding_v0.

The plots (x is x, y is y) below show the full possible range of finger tip locations (from palm fully open to all finger joints fully closed) and half range (from palm fully open to finger joints half-bent):
![finger_tips_full_range.png|300](/img/user/images/finger_tips_full_range.png)
![finger_tips.png|300](/img/user/images/finger_tips.png)
![Pasted image 20220927150536.png|300](/img/user/images/Pasted%20image%2020220927150536.png)

 Target trajectory:
 
- [x] Figure out whether the x or the y dimension corresponds to the length of the forearm
	- [x] it is y
	- [x] see code snippet below
- [ ] In that dimension, set goal trajectories for each of the four finger-tips to move such that the pinky and the index finger tips are phase-lagged by $\pi$.
- [ ] Allow deviations in the other two dimensions.
- [ ] The tip of the thumb must move simultaneously in the orthogonal dimension: x
	- [ ] Figure out the phase difference with the index from the YouTube video
	- [ ] Other YouTube video suggests that we could even skip the thumb, so this is definitely not the priority.

###### Figuring out dimensions

```python
import gym
import numpy as np
import matplotlib.pyplot as plt

# initialize environment with default config, i.e., fully open palm
env = gym.make("CustomMyoChallengeBaodingP1-v1", enable_rhi=True, enable_rsi=True)

# site names
site_names = ['THtip', 'IFtip', 'MFtip', 'RFtip', 'LFtip', 'target1_site']

# get ids for each of the finger tips and ball targets
site_ids = [env.sim.model.site_name2id(site_name) for site_name in site_names]

positions = []

for ep in range(100):
    _ = env.reset()
    
    # get position for each finger tip and ball target
    positions.append(env.sim.data.site_xpos[[site_ids]][0])

# tip positions array (n_runs x (n_finger_tips + n_ball_targets) x 3)
pos = np.array(positions)

# define color each tip
colors = ["violet", "blue", "cyan","teal","green","lime", "red", "gold"]

# 2d plot
# scatter plot
for i, color in enumerate(colors):
    plt.scatter(x=pos[:,i,0], y=pos[:,i,1], c=color)

# annotate the last point
for i, txt in enumerate(site_names):
    plt.annotate(txt, (pos[-1,i,0], pos[-1,i,1]))

plt.savefig("finger_tips_with_ball_traj.png")
plt.close()

# 3d plot
fig = plt.figure()
ax = fig.add_subplot(projection='3d')

# scatter plot
for i, color in enumerate(colors):
    ax.scatter(xs=pos[:,i,0], ys=pos[:,i,1], zs=pos[:,i,2], c=color)

plt.savefig("finger_tips_3d.png")
plt.close()
```


### Reward design

#### 3. Distance from target 

- pos_dist_i, $i\in{2,...,5}$

#### 4. Solved

- key already present, but need to change how rewards are computed from it
- this might be where we would need to reward the x dimension but not the y (or the other way around)

#### 5. Action regularization

- [x] Already present