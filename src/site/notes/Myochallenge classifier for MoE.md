---
{"dg-publish":true,"permalink":"/myochallenge-classifier-for-mo-e/","created":"","updated":""}
---

# Plan of action

## Code structure
### Agent
- Use RecurrentPPO agent as a classifier
- Gets as input observations from the environment given the action of the experts
- Outputs as actions 0 or 1 corresponding to env.which_task.value

### Environment
- Build env with gym api
```python

class ClassifierEnv:
	def __init__(self, env):
		self.env = env
		self.action_space = gym.Discrete(2)
		self.observation_space = self.env.observation_space
		
	
	def step(self, action):
		if action == 0:
			return self.env.step(action_ccw)
		else:
			return self.env.step(action_cw)
	
	def reset(self):
		return self.env.reset()

	def render(self):
		return self.env.render()
	
```