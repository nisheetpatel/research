---
{"dg-publish":true,"permalink":"/myochallenge-priorities-2022-10-14/","created":"","updated":""}
---

# Priorities

## Cluster and scaling

- Use PhD Booster money to buy a ton of compute time on clusters?
- Cluster pipeline to try every possible experiment we can think of

## Pablo's suggestions

### Super-model for both tasks
- Either dumb one by manually combining ccw & cw
	- run this while I try to code new things to make best use of time
- or one trained with imitation learning

- probably the best idea:
	- a really ghetto supermodel class that takes in two fully trained agents, the best one for CW and the best one for CCW, and manually looks at the relevant observation dimensions to output

```python
model_cw = load_model(path_to_model_cw)
model_ccw = load_model(path_to_model_ccw)

@dataclass
class SuperModel:
	model_cw: Model
	env_cw: BaodingEnv
	model_ccw: Model
	env_ccw: BaodingEnv
	current_task: bool = 0 # CCW (change to Enum value)
	obs_list: field(default_factory = list, repr=False)

	def determine_task_type(self) -> bool:
		"""whether current trial requires CW/CCW rotation"""
		pass

	def update_task_type(obs_single) -> None:
		"""update current trial type based on new observation"""
		self.obs_list.append(obs_single)
		self.current_task = self.determine_task_type()

	def act(self, obs) -> np.ndarray:
		"""return action"""
		if not self.current_task: # CCW task
			self.model_ccw.act(obs)
			# also do lstm updates
		else:
			self.model_cw.act(obs)
```

### imitation learning
- Recurrent PPO or PPO with frame-stacking?
- learning from preferences

## Code and structure

### Design
- TDD / red-green-refactor from scratch
- Get inspiration from Alberto's original codebase

### Saving, loading, and tracking
- Systematize saving with curriculum and parameters
- save best model and last model
- track with w&b + tensorflow?

### Curriculum
- Construct a dataclass to instantiate a curriculum


```python

# custom type aliases
EnvConfig = dict[[str|dict], any]
Model = type(RecurrentPPO)
BaodingEnv = type(BaodingEnv)

env_config = dict(
	...
)

@dataclass
class Experiment:
	model: Model
	env: BaodingEnv
	curriculum: list[EnvConfig]

	def _get_env(env_config: EnvConfig) -> BaodingEnv:
		pass

	def _load_model(
		model_path: str = None,
		env_config: EnvConfig,
		) -> Model:
		pass
		
	def _check_convergence(self) -> bool:
		"""Check whether performance has converged."""
		pass
		
	def _run_single(self, model: Model, env: Env) -> None:
		"""Run single experiment until convergence."""
		pass

	def save(self, save_name: str = None) -> None:
		"""Save current model and environment."""
		pass

	def run(self) -> None:
		for env_config in curriculum:
			env = self._load_env(env_config)
			model = self._load_model(model_path, env_config)

			self._run_single(model, env)


```

---

## Hyperparameter tuning

Set up with ray.tune()

---

## Models 

