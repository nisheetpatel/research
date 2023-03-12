---
{"dg-publish":true,"permalink":"/myochallenge-standardize-curriculum/","created":"","updated":""}
---

# Myochallenge

## Curriculum learning

### Notes

#### Things to vary across tasks

##### 0. Penalty for not keeping palm up
Add rewards keys for  keeping the palm up (or penalty otherwise). Relevant observation dimensions are 0 (pronation-supination) and 2 (flexion-extension).

##### 1. Ball rotation direction
This is part of the original task that we need to solve for the challenge itself. For now, our solution only has one direction. We may have to code this to be used flexibly across the curriculum

##### 2. Ball rotation speed
This is currently part of our curriculum as well. But of course, it is not systematic.

##### 3. Initial state
###### 3.1. Balls and targets
This is also part of the current curriculum. We learned about the idea from DeepMimic, which clearly expresses this as one of it's most important insights. Initializing the episode in a random state along multiple points in the desired trajectory allows much quicker local learning. Refer to the diagrams below for a better idea.

![Pasted image 20220925140512.png|200](/img/user/images/Pasted%20image%2020220925140512.png)

![Pasted image 20220925140537.png|200](/img/user/images/Pasted%20image%2020220925140537.png)

###### 3.1.1. Slow-start or lag-start targets
One of the issues is that especially when the target ball rotation speed gets too quick, the agent needs to really accelerate the balls in the initial frames since the balls start with zero velocity. One of the things we could try is to make the target speed increase gradually within an episode.

###### 3.2. Hand configuration
The hand always starts with the palm facing up and all fingers fully open. What might really help in making the solution robust is a hand configuration where the fingers start in a bent position.

##### 4. Episode termination criteria
I don't know if this is totally necessary, but we could also consider ending the episode earlier by lowering the ball-drop threshold.

In any case, we will need control over this because for point 6 later, say when we want to implement other tasks such as pose and reach, we would let the agent drop the balls without any consequence. In other words, we would basically remove the ball drop threshold and not let the episode terminate for the agent to solve the pose/reach task.

##### 5. Penalty for effort
I think it might be useful to gradually vary the penalty for effort. This should be easy enough since it is already one of the weighted reward keys.

##### 6. Task goal
Implement the following tasks by including flags in the config and copying relevant parts of the `_setup()` function from the respective tasks. Once you fuck with the action space yourself on mujoco's controllable environment, you quickly realize how much correlation there is in the action space. Like there are two independent dimensions for controlling flexion and extension of, for instance, fingers. The hope is that by doing other simpler tasks, we would be able to capture some core regularities in the mapping from action space to outcomes in the world.

###### 6.1. Pose task
###### 6.2. Reach task
###### 6.3. Hold task

##### 7. Frame-skip or frame-stacking?
When you fuck with mujoco's controllable environment, another thing that becomes really obvious is how slow the actions are in terms of exerting the change that they do. It is *definitely not* one time-step. And this seems very problematic for RL - the dynamics are non-Markovian. 

One way I was thinking of solving this was simply by skipping frames, enough that the next observation would be a direct consequence of the previous action and that the consequences of actions exerted a few timesteps before do not continue to linger.

An alternative to this might be to somehow include a history dependence. So you take into account k previous timesteps, in this case, simply with frame-stacking. This would be very simple to implement, but I'm not sure how frame-stacking works with LSTM networks.


### Code

#### Config implementation
- [x] Ball rotation direction
	- [x] config > task
- [x] Ball rotation speed
	- [x] config > goal_time_period
- [x] Initial state
	- [x] Randomized for balls & targets (synced)
		- [x] config > enable_rsi
	- [x] Hand configuration
		- [x] `_setup()` > `self.init_qpos[:-14] *= 0`
		- [x] Set `self.init_qpos[0] = -1.57 # Palm up`
		- [x] Randomly initialize `qpos[5:-14]` from `self.sim.model.jnt_range`
- [x] Episode termination criteria
	- [x] config > drop_th
- [x] Penalty for effort
	- [x] config > weighted_reward_key > act_reg
- [ ] Alternative tasks
	- [ ] [[Finger movement sub-task for Baoding\|Finger movement sub-task for Baoding]]
	- [ ] Pose
	- [ ] Reach
		- [ ] Set the argument "sites" in `super()._setup()`
		- [ ] `sites = dict_keys(['THtip', 'IFtip', 'MFtip', 'RFtip', 'LFtip'])`
	- [ ] Hold
		- [x] we won't need this
- [x] Frame-skip
	- [x] config > frame_skip


---

## Notes for the future

### Concrete ideas
1. Try implementing the same curriculum on PPO + LSTM & PPO + frame-stacking. I suspect that PPO + frame-stacking might learn much faster and may also outperform PPO + LSTM.
2. Tune hyperparameters on a simpler task, e.g. pose or reach.

### Half-baked ideas
- Exploit the knowledge about the action space.
	- We know which dimension each muscle corresponds to, thanks to the mujoco interactive environment.
	- Design tasks to teach the network about *opposing* actions, e.g. flexion vs extension.


---

## Super hack!

### Observation space
Total 86 dimensions in this order:
- 23: joints
- 24: correspond to balls' positions and velocities, and targets' positions and errors
- 39: correspond to the action space

#### Joints

##### Decoding the acronyms

![Pasted image 20220926010650.png](/img/user/images/Pasted%20image%2020220926010650.png)

![Pasted image 20220926010629.png](/img/user/images/Pasted%20image%2020220926010629.png)


- Pro-sup:
	- pronation (palm facing down) and supination (palm facing up)
- Deviation
	- imagine waving hi without moving the forearm at all
- Flexion (and extension):
	- imagine making a japanese fan out of your hand, without moving the forearm
- CMC:
	- Where the wrist meets palm
	- Carpometacarpal
- MP:
	- These must be MCP for the thumb, i.e. base knuckle of the thumb
- IP:
	- This is the outer knuckle of the thumb
- 2: index, ..., 4: pinky
- MCP:
	- These are the main knuckles, i.e. where the fingers join the palm
	- Metacarpophalangeal
- PM:
	- must be phalanges medial, i.e. the knuckle used for dragon punch
- MD:
	- the last knucke before the the nail
- Abduction:
	- some kind of rotation

###### Function and range
`env.sim.model.jnt_range`

0. pro_sup
1. deviation
2. flexion
3. cmc_abduction
4. cmc_flexion
5. mp_flexion
6. ip_flexion
7. mcp2_flexion
8. mcp2_abduction
9. pm2_flexion
10. md2_flexion
11. mcp3_flexion
12. mcp3_abduction
13. pm3_flexion
14. md3_flexion
15. mcp4_flexion
16. mcp4_abduction
17. pm4_flexion
18. md4_flexion
19. mcp5_flexion
20. mcp5_abduction
21. pm5_flexion
22. md5_flexion

#### Balls, targets, and errors
- 3 x ball 1 position
- 3 x ball 1 velocity
- 3 x ball 2 position
- 3 x ball 2 velocity
- 3 x target 1 position
- 3 x target 2 position
- 3 x target 1 error
- 3 x target 2 error

### Action space

Total 39 dimensions in this order:

- ECRL
	- Extensor Carpis Radialis Longus
- ECRB
	- Extensor Carpis Radialis Brevis
- ECU
	- Extensor Carpi Ulnaris
- FCR
	- Flexor Carpi Radialis
- FCU
	- Flexor Carpi Ulnaris
- PL
	- Palmaris longus
- PT
	- Pronator teres
- PQ
	- Pronator
- EIP
	- Extensor Indicis Proprius
- EPL
	- Extensor Pollicis Longus
- EPB
	- Extensor Pollicis Brevis
- FPL
	- Flexor Pollicis Longus
- APL
	- Abductor Pollicis Longus
- OP
	- Opponens Pollicis
- FDS
	- Flexor Digitorum Superficialis (2- index, 3- middle, 4- ring, 5- little)
- FDP
	- Flexor Digitorum Profundus (2- index, 3- middle, 4- ring, 5- little)
- EDC
	- Extensor Digitorum Communis (2- index, 3- middle, 4- ring, 5- little)
- EDM
	- Extensor Digiti Minimi
- RI
	- Radial Interosseous (2- index, 3- middle, 4- ring, 5- little)
- LU-RB
	- Lumbrical (2- index, 3- middle, 4- ring, 5- little)
- UI-UB
	- Palmar or Ulnar Interosseous (2- index, 3- middle, 4- ring, 5- little)

---

## Archive

### Pablo's implementation

#### main.py
Do the following for a given config:
- define config
	- weighted reward keys
	- goal time period
- make parallel environments
	- fetch custom env from baoding.py (automatic given config)
- define evaluation callback
- define or load model
- learn for 10M timesteps & save
- evaluate model
	- render in environment
	- compute and print reward obtained

### Changes to make

#### main.py
1. Separate config creation from running the algorithm
	- use branch "nisheet", script "main_sb3.py"
2. 