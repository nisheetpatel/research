---
{"dg-publish":true,"permalink":"/code-dra-vs-max-ent-rl-refactored-v0-1/","created":"","updated":""}
---

[[DRA vs Max Ent RL\|DRA vs Max Ent RL]] [[code DRA vs maxEnt RL original\|code DRA vs maxEnt RL original]]
tags: #DRA #max-entropy #RL #research #project #inference #code

## Refactored code v0.1
- Committed to master on 26th April, 2022

### Tasks
```python
class Environment(Protocol):
	@property
	def action_space() -> gym.spaces.Discrete:
		...

	def reset() -> list[int]:
		...

	def step() -> Tuple[list, int, bool, dict]:
		...

@dataclass
class EnvironmentAttributes:
	n_states: int
	transitions: Optional[list[int, int]]
	rewards: Optional[list]

	@property
	def transition_matrix(self):
		# returns transition matrix for specified transitions
		pass

class ZiebartTask:
	pass

class Maze:
	pass

class Memory2AFC_RLstyle:
	pass

class BottleneckTask:
	"""yet to be coded with the Environment Protocol"""
	pass
	
class HuysTask:
	"""yet to be coded with the Environment Protocol"""
	pass
```

### Models
```python
class Agent(Protocol):
	
	@staticmethod
	def _index(state: list, action: int = None, n_actions: int = None) -> tuple:
	...
	
	def act(self, state, n_actions):
	...
	
	def update_values(self) -> None:
	...

def softargmax(x: np.array, beta: float = 1) -> np.array:
	pass

def standard_indexer(state: list, action: int = None, n_actions: int = None) -> tuple:
	"""Returns standard q-table index as tuple"""
	pass  

def memory_2afc_task_indexer(state: list, action: int = None, n_actions: int = 2):
	 """Returns q-table index for Memory_2AFC task as tuple"""
	pass

@dataclass
class DRA:
	_index = staticmethod(standard_indexer)
	
	def act(self, state:list, n_actions: int):
		pass

	def update_values(self, s, a, r, s1, prob_a) -> None:
		pass


@dataclass
class MaxEntRL:
	_index = staticmethod(standard_indexer)
	
	def act(self, state:list, n_actions: int):
		pass

	def update_values(self, s, a, r, s1, prob_a) -> None:
		pass


@dataclass
class MaxEntRL_2AFC:
	_index = staticmethod(standard_indexer)
	
	def act(self, state:list, n_actions: int):
		pass

	def update_values(self, s, a, r, s1, prob_a) -> None:
		pass
```


### Training
```python
def kl_divergence_MVN(m0, S0, m1, S1) -> float:
	"""
	KL-divergence from Gaussian m0,S0 to Gaussian m1,S1,
	expressed in nats. Diagonal covariances are assumed.
	"""
	pass

@dataclass
class Simulator:
	agent: Agent
	env: Environment

	def _record_choices(self, state, action, reward) -> None:
		"""Records agent's experience tuple."""
		pass
	
	def run_episode(self, updateQ=True):
		"""Runs episode, updates q-values, returns reward."""
		pass
	
	def update_agent_noise(self):
		"""Updates agent's (DRA) noise parameters."""
		pass
	
	def compute_expected_reward(self, n_episodes) -> float:
		pass
	
	def compute_cost(self) -> float:
		pass
```

## Workbench scripts
- Memory2AFC task
	- Doesn't work on PMT trials but fine otw
- Ziebart task
	- Can differentiate behavior between vanilla DRA and maxEntRL models but only in the case where all trajectories have the same reward
- Maze
	- Entropy a bit weird for maxEntRL but I think that, to some extent, that's an artefact of discretization