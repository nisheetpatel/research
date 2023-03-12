---
{"dg-publish":true,"permalink":"/myochallenge-phase-2-task-details/","created":"","updated":""}
---

[link](https://github.com/facebookresearch/myosuite/blob/main/myosuite/envs/myo/myochallenge/__init__.py) to the github file and relevant code snippet:

```python
register(id='myoChallengeBaodingP2-v1',
	entry_point='myosuite.envs.myo.myochallenge.baoding_v1:BaodingEnvV1',
	max_episode_steps=200,
	kwargs={
		'model_path': curr_dir+'/../assets/hand/myo_hand_baoding.mjb',
		'normalize_act': True,
		'goal_time_period': (4, 6),
		'goal_xrange': (0.020, 0.030),
		'goal_yrange': (0.022, 0.032),
		# + Randomization in physical properties of the baoding balls
		# Details will be released during the launch of Phase2
	}
)
```